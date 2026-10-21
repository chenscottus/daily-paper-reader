---
title: Deep Kernel Fusion for Transformers
title_zh: 面向Transformer的深度算子融合
authors: "Zixi Zhang, Zhiwen Mo, Yiren Zhao, Robert D. Mullins"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://aclanthology.org/2026.acl-short.15.pdf"
tags: ["query:litert-ops"]
score: 5.0
evidence: 通过深度算子融合降低访存以加速推理
tldr: "长上下文智能体推理受限于内存带宽而非算力，SwiGLU MLP块因权重超出缓存成为未被充分优化的瓶颈。本文提出DeepFusionKernel，通过深度算子融合减少HBM访存并提升缓存复用。在H100上相较SGLang最高提速13.2%，A100上提速9.7%，并配合内核调度器保持稳定加速。该工作体现了算子融合与内存布局优化对推理加速的价值。"
source: ACL-2026-Short
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-short/anthology-2026acl-short15/fig-001.webp\", \"caption\": \"\", \"page\": 8, \"index\": 1, \"width\": 6190, \"height\": 1751}]"
motivation: 长上下文推理受内存带宽限制，SwiGLU MLP块因权重超缓存成为未优化瓶颈。
method: 提出DeepFusionKernel，通过深度算子融合降低HBM访存并提升缓存复用，配合内核调度器。
result: "在H100上相较SGLang最高提速13.2%，A100上提速9.7%。"
conclusion: 表明算子融合与内存优化可显著提升推理加速效果且适配多平台。
---

## Abstract
Agentic LLM inference with long contexts is increasingly limited by memory bandwidth rather than compute. In this setting, SwiGLU MLP blocks, whose large weights exceed cache capacity, become a major yet under-optimized bottleneck in the Transformer architecture. We propose DeepFusionKernel, a deeply fused kernel that cuts HBM traffic and boosts cache reuse, delivering up to 13.2% speedup on H100 and 9.7% on A100 over SGLang. Integrated with SGLang and paired with a kernel scheduler, DeepFusionKernel ensures consistent accelerations across generation lengths, while remaining adaptable to diverse models, inference configurations, and hardware platforms.

---

## 论文详细总结（自动生成）

# 《Deep Kernel Fusion for Transformers》论文中文总结

## 1. 核心问题与整体含义（研究动机与背景）

- **背景转变：从算力瓶颈到内存带宽瓶颈。** 论文指出，面向智能体（agentic）的长上下文 LLM 推理需要处理极长上下文、大量持久状态和长输出（如大型代码库），每次推理处理的 token 数可达普通聊天负载的约 100 倍。在此场景下，KV 缓存与参数访问使瓶颈从原始算力转向内存容量与 HBM 带宽，导致可允许的 batch size 缩小、GEMM 输入变“胖”、Tensor Core 利用率下降。
- **被忽视的瓶颈：SwiGLU MLP。** 近期 GPU 优化多集中于注意力机制（FlashAttention、PagedAttention、FlashInfer 等），但在自回归解码中，SwiGLU MLP 块主导参数量与内存占用，其大权重超出缓存容量，成为 Transformer 中重要但优化不足的访存瓶颈。
- **核心问题：** 如何在内存带宽受限的解码场景中，减少 SwiGLU MLP 的 HBM 读写并提升缓存复用，从而在不增加 FLOPs 的前提下提升端到端推理吞吐。
- **整体含义：** 论文主张通过“深度算子融合”把 SwiGLU 中分离的 GEMM 与逐元素核合并为单一核，消除中间激活的物化与往返 HBM 流量，使现代 GPU 在带宽受限推理中更充分地发挥可用算力；并强调该优化可嵌入现有推理栈（SGLang），具有实际部署价值。

## 2. 方法论：核心思想与关键技术细节

- **核心思想：** 对 SwiGLU MLP 进行激进融合，将常见实现中的 4 次 kernel 启动（PyTorch）或 2 次 kernel 启动（SGLang/vLLM）合并为 1 个深度融合算子，让中间值在核内“流式”传递，避免大型临时张量的物化与额外 HBM 读写。
- **SwiGLU 计算流程：**
  - \(A_{gate}=XW_{Gate}\)，\(A_1=XW_{Up}\)；
  - \(A_{silu}=\mathrm{SiLU}(A_{gate})=\mathrm{Sigmoid}(A_{gate})\otimes A_{gate}\)；
  - \(A_2=A_1\otimes A_{silu}\)；
  - \(Y=A_2W_{Down}\)。
  - 其中 \(W_{Up},W_{Gate}\in\mathbb{R}^{d_{model}\times d_{ff}}\)，\(W_{Down}\in\mathbb{R}^{d_{ff}\times d_{model}}\)，且 \(d_{ff}\) 约为 \(d_{model}\) 的 3.5–4 倍。
- **融合边界划分：** 将 MLP 分为两个阶段：
  - 第一阶段：\(A_2=(XW_{Up})\otimes\mathrm{SiLU}(XW_{Gate})\)；
  - 第二阶段：\(Y=A_2W_{Down}\)。
  - 论文将融合重点放在第一阶段，因为其包含主导解码访存的 GEMM 与逐元素门控；真正含长程依赖的归约操作（如 Softmax）不适合跨 SM 流式融合。
- **实现层面的关键设计：**
  - **循环顺序与分块策略：** 系统探索行主序与列主序 tiling 的权衡。行主序 tiling 改善输入激活 \(X\) 的局部性，适合较大 batch 或激活主导访存的场景；列主序 tiling 更好复用权重 tile，适合小 batch、大模型、权重主导内存占用的智能体解码场景。
  - **tile 尺寸调优：** 在寄存器使用、共享内存占用与 Tensor Core 利用率之间平衡；附录显示共享内存容量是硬上限，过小 tile 又无法饱和内存控制器，最终在 A100 上自动调优收敛到 \((128,128)\) 配置，内存带宽利用率约 91.51%，单核延迟约 127.81 μs。
  - **Profiler 驱动的 kernel 调度器：** 由于最优配置依赖模型形状、batch size、GPU 微架构与互连，部署前先用短时间 profiling 在候选核中基准测试并选择最高吞吐版本；选定后通过 CUDA Graphs 捕获，避免推理时反复调度开销。
- **分布式适配：** 在张量并行（TP）下，对 \(W_{Up}\)、\(W_{Gate}\) 做列切分、对 \(W_{Down}\) 做行切分，使中间计算保持本地，整个 SwiGLU MLP 块只需一次 all-reduce，减少集合通信次数。

## 3. 实验设计：场景、Benchmark 与对比方法

- **任务场景：** 无传统意义上的训练数据集；实验为真实 LLM 自回归解码吞吐基准，覆盖普通解码与智能体长生成场景。
- **模型与精度：** LLaMA 3.1 70B，FP16；输入 prompt 长度固定为 1，仅测量解码阶段。
- **硬件与并行：** 4 张 NVIDIA A100 80GB SXM 或 4 张 H100 80GB SXM，单节点，TP=4。
- **软件栈：** 所有端到端实验在 SGLang 推理框架内完成，启用 FlashInfer 注意力后端与 CUDA Graphs，以降低 kernel 启动与 CPU 开销。
- **Benchmark 设置：**
  - 主吞吐实验：输出长度 1024，batch size 从 1 到 64，每个配置重复 4 次，报告均值与标准差。
  - 长生成实验：输出长度从 1024 扩展到 4096、16384，batch size 取 1、4、16，覆盖单查询、轻并发与中等并发。
- **对比方法：**
  - PyTorch 朴素分布式推理；
  - SGLang 默认 kernel；
  - vLLM；
  - 附录额外对比 torch.compile（Inductor 后端）；
  - 微基准层面比较 SGLang、vLLM 与 DeepFusionKernel 的单 MLP 块延迟。
- **评估指标：** 端到端解码吞吐（tokens/s）及标准差；辅以单 MLP 延迟、tile 配置下的计算/带宽利用率与延迟。

## 4. 资源与算力

- **推理算力：** 明确使用 4×A100 80GB SXM 与 4×H100 80GB SXM，单节点 TP=4；未使用多节点集群。
- **训练算力：** 论文不涉及模型训练，因此没有训练时长、训练 GPU 小时等数据。
- **未明确说明的内容：** 未报告实验总时长、总 GPU 小时、重复实验的总资源消耗、集群规模与具体互连拓扑细节；仅提及 A100 SXM 中 GPU 对通过 NVLink（600 GB/s）连接、其余走 PCIe Gen4（64 GB/s），并承认未穷尽评估不同集群互连。
- **调度开销：** 声称 kernel 选择仅需推理前一次短 profiling，之后由 CUDA Graphs 捕获，无持续调度开销，但未量化 profiling 的具体时间成本。

## 5. 实验数量与充分性

- **实验规模概览：**
  - 主表（Table 1）：2 种 GPU × 8 个 batch size × 3 种框架，共 48 个吞吐配置点，并给出 DeepFusionKernel 相对 SGLang 的加速比。
  - 长生成表（Table 2）：2 种 GPU × 3 个 batch size × 3 种输出长度 × 3 种方法，共 54 个配置点。
  - torch.compile 对比（Table 3）：7 个 batch size，3 种方法。
  - 单 MLP 微基准（Table 4）：3 种方法的延迟对比。
  - tile 尺寸敏感性分析：对 \((d_B,d_{model},d_{ff})\) 在 {32,64,128,256} 中做自动调优扫描，并报告共享内存耗尽、寄存器压力与最优平衡三类现象。
- **充分性评价：**
  - **优点：** 覆盖两代 GPU、多个 batch size、三种输出长度、端到端与微基准两个层级，并报告均值与标准差，重复 4 次，具备一定统计意识；tile 敏感性分析有助于解释性能来源。
  - **不足：** 模型仅 LLaMA 3.1 70B 一个，未覆盖不同规模、不同架构（如 MoE、MLA 类模型）；精度仅 FP16；并行仅 TP=4 单节点；batch 上限 64；未做严格的组件消融（如单独去除调度器、单独改变 tiling 的端到端贡献），调度器带来的收益与融合本身收益未完全分离。
  - **公平性：** 对比在同一 SGLang 框架、相同 FlashInfer 与 CUDA Graphs 设置下进行，baseline 为 SGLang 默认 kernel 与 vLLM，整体较为公平；但 DeepFusionKernel 额外获得针对硬件的候选核 profiling 与选择，属于方法自带的调优优势，论文对此有说明但未量化其独立贡献。此外，未与 TensorRT-LLM、Blockbuster 等同样具备融合能力的系统做端到端直接对比。

## 6. 主要结论与发现

- **端到端加速：** 相对 SGLang，DeepFusionKernel 在 A100 上最高提升 9.7%，在 H100 上最高提升 13.2%；在长生成与不同并发下相对 SGLang 和 vLLM 均保持吞吐优势。
- **瓶颈转移规律：** A100 上小 batch 时收益最大，因为工作负载强烈受内存带宽限制，减少读写直接提升吞吐；H100 算力更强（1979 TFLOPs/s vs A100 的 312 TFLOPs/s），但内存带宽仍是显著限制，融合仍有可测加速，只是在极大 batch 下边际收益递减。
- **长上下文稳定性：** 随着输出长度从 1024 增至 16384，注意力与 KV 缓存成本上升，但 MLP 仍占每 token 成本的显著部分，因此融合收益持续存在。
- **微基准验证：** 单 MLP 块延迟相对 SGLang 降低 5.89%、相对 vLLM 降低 5.53%；作者指出该比例会跨 LLaMA 3.1 70B 的 80 层累积，从而解释端到端提升。
- **调度器有效性：** profiling 选择加 CUDA Graphs 捕获可避免运行时分支与调度开销，使加速在不同生成长度上较一致。
- **与 torch.compile 对比：** Torch Inductor 在该高性能 LLM 推理场景中明显不具竞争力，小 batch 下吞吐低至约 1/126，并在长序列下出现 OOM。

## 7. 优点（方法与实验设计亮点）

- **问题定位精准：** 抓住注意力优化之外被相对忽视的 SwiGLU MLP 访存瓶颈，动机与 agentic 长上下文场景结合紧密。
- **融合深度较高：** 将两个 GEMM、SiLU 与逐元素乘合并为单一算子，直接消除中间激活的物化与 HBM 往返，且不增加 FLOPs。
- **硬件感知的工程细节：** 系统讨论行/列主序 tiling 的适用条件，并结合共享内存上限、寄存器压力与 Tensor Core 利用率调优 tile，具有较强可操作性。
- **调度器设计务实：** 用预推理 profiling 替代静态启发式，适应模型形状、batch、GPU 与互连差异；配合 CUDA Graphs 避免运行时开销。
- **真实框架集成：** 在 SGLang 中端到端评测，而非仅做孤立 kernel 微基准，提升了部署可信度；并保持与现有 all-reduce 通信模式兼容。
- **实验层级较完整：** 端到端吞吐、长生成扫描、单 MLP 微基准、tile 敏感性、torch.compile 对比相互印证；报告标准差并重复 4 次。
- **可复现性：** 论文提供开源代码链接，并列出模型与框架许可证。

## 8. 不足与局限

- **模型与场景覆盖有限：** 仅评测 LLaMA 3.1 70B 一个模型，未覆盖不同规模、MoE、MLA 等架构；仅 FP16，未涉及 FP8/INT4 等低精度或量化推理；仅测解码阶段，未评估预填充阶段。
- **硬件与并行范围有限：** 仅 A100 与 H100 两代 GPU，均为 4 卡单节点 TP=4；未评估更多 GPU、跨节点互连、NVLink 拓扑差异及 B200 等新硬件。作者在 Limitations 中亦承认未穷尽不同集群互连，且未量化 GPU 间通信带来的性能波动。
- **加速幅度与波动：** 端到端提升为 1.3%–13.2%，部分配置（

例如 batch 较大或输出极长时）收益收窄甚至落入测量波动范围，说明融合主要解决 MLP 访存瓶颈，而注意力与 KV 缓存成本上升会稀释其端到端占比；实际部署需结合工作负载分布评估收益，而不能仅看最高加速比。

- **收益归因仍不够细。** 论文将深度融合、tile 自动调优、profiling 选择与 CUDA Graphs 捕获作为组合方案报告，但缺少逐项消融，难以判断端到端提升中有多少来自消除中间激活 HBM 流量、多少来自更优 tile 配置或调度开销降低。
- **正确性与数值误差讨论不足。** 融合改变 SiLU、逐元素乘与 GEMM 的执行顺序和中间精度路径，论文未系统报告与基线在 logits、生成文本或数值误差上的对比，也未讨论不同 GPU 架构上 FP16 累加与舍入差异对生成质量的影响。
- **自动调优的部署成本与过拟合风险。** Profiler 驱动的候选核选择依赖模型形状、batch size 与硬件配置；论文未量化 profiling 时间、候选核数量及跨配置切换成本，也未说明在动态 batch、连续批处理或请求分布漂移时是否需要重新调优。
- **真实服务负载覆盖有限。** 实验以固定输出长度和固定 batch size 为主，未覆盖 continuous batching、动态请求到达、prefix caching、prefill/decode 分离部署等生产推理常见机制；这些机制会改变 MLP 与注意力的成本占比及 kernel 调度行为。
- **预填充与混合阶段缺失。** 论文仅测解码阶段且 prompt 长度固定为 1，未评估长 prompt 预填充、chunked prefill 或 prefill-decode 混合负载，因此结论主要适用于自回归解码的 MLP 访存瓶颈。
- **能效与尾延迟未评估。** 除吞吐外，未报告功耗、每 token 能耗、P99 延迟或不同并发下的延迟稳定性；对智能体长生成场景而言，尾延迟与能效同样影响部署决策。
- **可复现性仍有边界。** 虽提供开源代码与许可证信息，但 tile 自动调优、profiling 选择及 SGLang/CUDA Graphs 集成细节对复现结果影响较大，缺少完整配置矩阵与调优脚本说明会限制独立复现。

## 9. 总体评价与启示

- **总体评价：** 这是一篇问题定位清晰、工程实现扎实的系统优化论文。其核心贡献不在于提出新的数学算子，而在于把 SwiGLU MLP 的 GEMM 与逐元素门控做深度算子融合，并以硬件感知 tiling、profiling 选择和 CUDA Graphs 将其落地到 SGLang，展示了内存带宽受限解码场景中“减少 HBM 往返”仍具可观收益。
- **可信度与外推边界：** 端到端与微基准结果相互印证，A100/H100 两代 GPU、多个 batch 与输出长度使结论具有一定稳健性；但单模型、单精度、单节点 TP=4 的设置意味着加速幅度不能直接外推到 MoE、MLA、低精度量化、跨节点大规模并行或预填充主导的负载。
- **对实践者的启示：** 在带宽受限的自回归解码中，应优先检查 SwiGLU MLP 的中间激活物化与 kernel 启动次数；小 batch、大权重场景更适合列主序 tiling 以复用权重 tile；部署前用短 profiling 选择候选核并以 CUDA Graphs 捕获，可把调优优势转化为稳定吞吐；TP 下保持中间计算本地、仅一次 all-reduce 的切分方式有利于控制通信。
- **未来方向：** 可扩展到更多模型家族与 MoE/MLA 架构、FP8/INT4 量化推理、prefill 与 chunked prefill、跨节点互连与新一代 GPU；补充逐组件消融、数值正确性、能效与尾延迟评估；并与 TensorRT-LLM、Blockbuster 等融合系统做端到端对比；同时研究动态批处理与请求分布变化下的自动调优鲁棒性。
- **一句话结论：** 论文证明了“把 SwiGLU 的 GEMM 与门控逐元素运算深度融合”是在不增加 FLOPs 前提下缓解解码访存瓶颈的有效手段，但其收益大小高度依赖模型形状、batch、硬件与负载阶段，适合作为推理栈中的一项硬件感知优化而非普适万能加速。

（完）
