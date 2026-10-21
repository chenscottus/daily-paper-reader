---
title: "QFlash: Bridging Quantization and Memory Efficiency in Vision Transformer Attention"
title_zh: QFlash：弥合视觉Transformer注意力中的量化与内存效率
authors: "Sehyeon Oh, Yongin Kwon, Jemin Lee"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://www.ijcai.org/proceedings/2026/0167.pdf"
tags: ["query:litert-ops"]
score: 7.0
evidence: 面向GPU高效执行的整数量化注意力内核
tldr: FlashAttention的在线softmax依赖浮点运算保证数值稳定，使注意力算子难以完全量化。作者提出QFlash，把softmax整体搬到整数域并实现为单个Triton内核，针对累加尺度爆炸、GPU移位指数运算低效和量化粒度约束三大障碍给出解决方案。在ViT、DeiT、Swin等七个注意力负载上取得最高6.73倍加速。该工作为量化注意力算子的硬件高效执行提供了端到端内核级范式。
source: IJCAI-2026-Accepted
selection_source: conference_retrieval
motivation: FlashAttention的在线softmax依赖浮点运算，导致注意力算子难以实现全整数量化。
method: 提出QFlash，将softmax完全置于整数域并实现为单个Triton内核，解决尺度爆炸与量化粒度约束。
result: 在ViT、DeiT、Swin等七个注意力负载上相比I-ViT等基线取得最高6.73倍加速。
conclusion: 该工作证明整数域注意力内核可在GPU上高效执行，为量化算子内核设计提供可复用经验。
---

## Abstract
FlashAttention improves efficiency through tiling, but its online softmax still relies on floating-point arithmetic for numerical stability, making full quantization difficult.
We identify three main obstacles to integer-only FlashAttention: (1) scale explosion during tile-wise accumulation, (2) inefficient shift-based exponential operations on GPUs, and (3) quantization granularity constraints requiring uniform scales for integer comparison.
To address these challenges, we propose QFlash, an end-to-end integer FlashAttention design that performs softmax entirely in the integer domain and runs as a single Triton kernel.
On seven attention workloads from ViT, DeiT, and Swin models, QFlash achieves up to 6.73x speedup over I-ViT and up to 8.69x speedup on Swin, while reducing energy consumption by 18.8% compared to FP16 FlashAttention, without sacrificing Top-1 accuracy on ViT/DeiT and remaining competitive on Swin under per-tensor quantization.
Our code is publicly available at https://github.com/EfficientCompLab/qflash.

---

## 论文详细总结（自动生成）

# QFlash 论文中文总结

## 一、核心问题与研究动机

- **背景**：Transformer 自注意力的计算与内存开销随序列长度呈 \(O(N^2)\) 增长。FlashAttention 通过分块（tiling）和在线 softmax 将中间矩阵保留在片上，显著降低显存访问，但分块在线 softmax 为保持数值稳定仍依赖浮点运算。
- **核心问题**：现有量化注意力工作难以同时实现“全整数”和“融合内核”：
  - I-BERT、I-ViT 虽量化了 Softmax，但未采用 FlashAttention 式分块融合，内存瓶颈未解决；
  - QAttn、INT-FlashAttention 只量化矩阵乘法，Softmax 仍为浮点；
  - 因此缺少一种端到端、全整数、融合的注意力内核。
- **三大技术障碍**：
  - **C1 尺度爆炸**：分块累加中，整数指数近似误差与尺度变化跨 tile 传播，易导致数值不稳定；
  - **C2 GPU 低效**：移位式指数近似需分离整数/小数部分，涉及整数除法，而 GPU 上整数除法远慢于乘法/移位；
  - **C3 量化粒度约束**：融合注意力中跨 tile 的 running max 比较要求统一尺度，per-token 量化会引入尺度不匹配和反量化开销。
- **整体含义**：论文试图证明在商用 GPU 上，注意力（含 Softmax）可以完全在整数域完成，并同时获得延迟、能耗和精度上的可行折中，为量化注意力算子提供内核级设计范式。

## 二、方法论

### 2.1 核心思想

- 保留 FlashAttention 的分块融合结构，但将 QK^T、Softmax、PV 及归一化全部量化为 INT8/INT32 运算，实现单个 Triton 内核。
- 采用 **per-tensor 量化粒度**，保证整个注意力计算共享统一尺度，从而支持整数直接比较，避免反量化。
- 通过预计算常数、移位运算和“ScaleRelease”机制，控制跨 tile 累加的尺度传播与溢出。

### 2.2 关键技术细节

- **量化定义**：对矩阵 \(X\)，尺度 \(s_X=\|X\|_\infty/(2^{b-1}-1)\)，量化值 \(\hat X=\mathrm{round}(X/s_X)\)；矩阵乘为 INT8×INT8→INT32，输出尺度 \(s_C=s_A\cdot s_B\)。
- **整体流程（Algorithm 1）**：外层遍历 Query 块，内层遍历 Key/Value 块，单次输出一个 \(\hat O_i\)。主要步骤包括：
  1. 计算 Query–Key 分数；
  2. 计算行最大值；
  3. 更新跨 tile running max；
  4. 计算 Score Intensity（整数指数）；
  5. 计算 Max-Offset Factor；
  6. Requantization；
  7. 计算行和；
  8. 计算 Value MatMul；
  9. 累加分母；
  10. 累加分子；
  11. 归一化。
- **ShiftExp2 整数指数近似**：
  - 利用 \(e^{s_X\hat X}=2^{s_X\log_2 e\cdot\hat X}=2^{\tilde s_X\hat X}\)；
  - 将输入分解为整数部分 \(Q\) 与小数部分 \(R\)，其中 \(R\in(-1,0]\)；
  - 对小数部分采用线性近似 \(2^{\tilde s_X R}\approx \tilde s_X(R/2+\lfloor1/\tilde s_X\rfloor)\)，其中 \(\lfloor1/\tilde s_X\rfloor\) 可预计算；
  - 最终结果为“线性近似项 × \(2^Q\)”，用移位和乘加实现，避免显式指数与整数除法。
- **Requantization**：预计算定点乘子 \(M_r=\lfloor(s_X/s_Y)\cdot2^r\rfloor\) 和移位位数 \(r=b-n\)，通过 \((\hat X\cdot M_r)\gg r\) 将 INT32 结果压回 INT8。
- **ScaleRelease**：在分子/分母跨 tile 累加时，将逆尺度近似为整数进行除法，避免反量化到浮点，同时控制溢出和尺度增长（对应 C1）。
- **归一化**：分子分母均为整数，直接整数除法，尺度在比值中抵消，输出 INT8 及输出尺度 \(s_O=s_V\)。
- **C2 优化**：将移位指数中原本需要整数除法的分解改为预计算常数与乘加/移位组合，降低 GPU 指令开销（细节在附录 B.2）。
- **C3 处理**：选择 per-tensor（或 per-head）粒度，放弃 per-token，以统一尺度换取整数直接比较和内核融合。

## 三、实验设计

- **硬件与软件**：NVIDIA GeForce RTX 5090；TVM 0.8、PyTorch 2.7.1（CUDA 12.8）、Triton 3.3.1。
- **工作负载**：从 ViT、DeiT、Swin 中提取 7 个独特注意力配置（A1–A7），输入分辨率 224×224：
  - A1–A3：ViT/DeiT-Tiny/Small/Base，序列长度 197，头维度 64；
  - A4–A7：Swin-T/S 四个 stage，窗口数、头数、序列长度不同。
- **批次设置**：batch size 1（实时推理）与 batch size 8（吞吐推理）。
- **评测指标**：
  - 推理延迟；
  - Tensor Core 利用率（Nsight Compute，IMMA/HMMA）；
  - 能耗（nvidia-smi 记录功耗，Nsight Compute 统计操作数）；
  - 量化精度：SQNR、MSE；
  - 端到端 Top-1 准确率。
- **对比方法**：
  - 整数基线：I-ViT（TVM INT8）；
  - 量化方法：I-BERT、FQ-ViT、QAttn、INT-FlashAttention（Full/Half）；
  - 浮点/混合基线：FlashAttention-2（FP16）、Torch FP32；
  - 精度表中还列出各方法的 BOPs 与是否全整数。
- **精度评测设置**：仅量化注意力模块，其余层保持浮点，以隔离注意力量化的影响；激活采用动态量化；各对比方法使用其原始实现和原始量化粒度。

## 四、资源与算力

- 文中明确使用 **单张 NVIDIA RTX 5090 GPU** 完成全部实验。
- 软件栈为 TVM 0.8、PyTorch 2.7.1（CUDA 12.8）、Triton 3.3.1。
- 该工作属于推理内核设计与评测，**未涉及模型训练**，因此没有训练时长、训练卡数或多机规模等信息。
- 能耗通过 `nvidia-smi` 记录，操作数与利用率通过 Nsight Compute 统计；但未说明实验重复次数、方差、GPU 频率/功耗限制等细节。
- 未报告 CPU、内存带宽、显存占用等配套资源信息。

## 五、实验数量与充分性

- **实验规模大致为**：
  - 延迟实验：7 个 workload × 2 个 batch size × 约 8 种方法，约 100 余组配置；
  - Tensor Core 利用率：A2、A7 两个 workload，batch=8；
  - 能耗：仅 A2，batch=8；
  - SQNR/MSE：A2、A7 两个 workload；
  - Top-1：ViT-S/B、DeiT-T/S/B、Swin-T/S 共 7 个模型，对比 7 种方法；
  - 消融/分析：正文未集中呈现，尺度管理、GPU 友好指数、粒度约束等分析主要放在附录。
- **充分性**：
  - 覆盖了 ViT、DeiT、Swin 三类主流视觉 Transformer 及不同规模，batch 1/8 两档，具备一定代表性；
  - 同时报告延迟、能耗、SQNR、Top-1 和 Tensor Core 利用率，维度较完整；
  - 但能耗与 SQNR 仅覆盖 1–2 个 workload，统计支撑偏弱；缺少系统的消融实验汇总、端到端模型延迟/显存、长序列和不同 GPU 架构验证。
- **公平性**：
  - 精度对比使用各方法原始实现和原始粒度，并列出 BOPs，整体较透明；
  - 但 I-ViT 为 TVM 实现，可能并非 GPU 上最优的整数基线；QFlash 的 per-tensor 与混合精度/细粒度方法比较时，精度差异部分来自粒度设计而非纯内核质量；
  - Top-1 仅量化注意力模块，属于隔离性评测，不能直接等同于全模型全量化部署效果。
- **数字一致性提示**：正文报告相对 I-ViT 的 Swin 加速为 **8.53×**，而摘要/元数据中出现 **8.69×**，存在版本或笔误差异，阅读时需以正文表格/实验描述为准。

## 六、主要结论与发现

- **可行性**：QFlash 证明了 Softmax 可以在整数域完成，并以单个 Triton 内核实现端到端整数 FlashAttention。
- **延迟**：
  - batch=1 时，相对 I-ViT 在 ViT/DeiT 上最高 **4.54×**、Swin 上最高 **7.54×** 加速；
  - batch=8 时，分别最高 **6.73×** 和 **8.53×** 加速；
  - 在多数 workload 上不慢于 FP16/混合精度内核。
- **能耗**：在 A2、batch=8 下，QFlash 能耗 **754.6 μJ**，低于 FlashAttention-2 的 **929.6
