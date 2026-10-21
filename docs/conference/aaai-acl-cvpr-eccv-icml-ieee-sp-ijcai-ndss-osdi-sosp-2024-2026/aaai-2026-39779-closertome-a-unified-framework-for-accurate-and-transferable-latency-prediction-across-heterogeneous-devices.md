---
title: "CloserToMe: A Unified Framework for Accurate and Transferable Latency Prediction Across Heterogeneous Devices"
title_zh: CloserToMe：面向异构设备的准确可迁移延迟预测统一框架
authors: "Cheng Tang, Guochong Sui, Wenqi Lou, Zihan Wang, Jiayi Tuo, Wenqian Xie, Yinkang Gao, Yixuan Zhu, Lei Gong, Chao Wang, Xuehai Zhou"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39779/43740"
tags: ["query:litert"]
score: 6.0
evidence: 跨GPU、NPU、FPGA硬件后端并结合算子变换的延迟预测
tldr: 面向异构设备选择最优模型优化策略需要准确的延迟预测，但现有方法把硬件当作黑盒直接回归，忽略了网络结构与设备执行行为的交互。本文提出统一的建模框架，显式刻画算子变换与结构修改在不同硬件后端上的执行特性。实验表明该框架能在GPU、NPU与FPGA之间实现更准确且可迁移的延迟预测，从而加速后端策略选择。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39779/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 3003, \"height\": 1435}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39779/fig-002.webp\", \"caption\": \"\", \"page\": 3, \"index\": 2, \"width\": 3980, \"height\": 2034}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39779/fig-003.webp\", \"caption\": \"\", \"page\": 6, \"index\": 3, \"width\": 2316, \"height\": 417}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39779/fig-004.webp\", \"caption\": \"\", \"page\": 6, \"index\": 4, \"width\": 1909, \"height\": 653}]"
motivation: 异构设备上选择模型优化策略需要准确延迟预测，现有黑盒回归方法忽略结构与硬件的执行交互。
method: 提出统一建模框架，显式刻画神经网络结构与GPU、NPU、FPGA等设备执行行为之间的交互关系。
result: 实验表明该框架在多种硬件后端上取得更准确且可跨设备迁移的延迟预测效果。
conclusion: 显式建模结构-硬件交互可提升延迟预测的准确性与可迁移性，助力跨后端策略快速选择。
---

## Abstract
Hardware accelerators such as GPUs, NPUs, and FPGAs are essential to meeting AI’s computational demands. With the proliferation of heterogeneous devices across cloud and edge, various model optimization techniques adapt to diverse hardware characteristics through operator transformations and structural modifications.  Accurate, efficient latency prediction enables rapid selection of optimal strategies across hardware backends.
Many existing methods treat hardware as a black-box executor, directly regressing latency without explicitly modeling the intricate interactions between neural network (NN) structures and device-specific execution behaviors. To address these challenges, we introduce a new modeling perspective that captures the interaction between neural architectures and hardware execution. To capture device-specific characteristics, we propose two complementary modeling strategies. The Device Behavior Signature Selector (DBSel) characterizes hardware execution behavior by selectively probing a small set of representative architectures, forming a compact, workload-driven profile. In parallel, we construct capability vectors that capture the hierarchical memory of each device and compute characteristics, providing a structured abstraction of its architectural capacity. To unify both behavioral and structural views, we introduce the Hardware–Operation Dialogue Module (HODM), which models fine-grained interactions between neural operators and hardware properties. Together, these components empower CloserToMe to deliver accurate and transferable latency predictions across unseen and diverse platforms.

---

## 论文详细总结（自动生成）

## 1. 核心问题与研究背景

- **核心问题**：在云—边异构硬件（GPU、NPU、FPGA、CPU/移动 SoC 等）上，如何对神经网络推理延迟进行**准确且可迁移**的预测，从而快速选择模型结构、算子变换和后端优化策略。
- **背景动机**：同一网络在不同硬件上的延迟差异巨大；传统 FLOPs 代理指标、算子级查找表、kernel 级回归或平台特定 GNN 预测器，往往把硬件视为黑盒或仅用设备 ID 编码，**没有显式建模神经网络结构与设备执行行为之间的交互**。
- **整体含义**：作者提出 CloserToMe，试图统一“**硬件结构能力**”与“**工作负载驱动的执行行为**”两个视角，提升对未见设备的绝对延迟预测精度与跨平台泛化能力，而不仅是延迟排序。

## 2. 方法论

### 2.1 核心思想
- 将延迟预测建模为：**神经网络图结构 + 设备行为签名 + 硬件能力向量 + 算子—硬件交互模块** 的联合学习。
- 设备表示由两部分互补构成：
  - **行为视角**：DBSel 在目标设备上轻量探测少量代表性架构，形成 Device Behavior Signature（DBS）。
  - **结构视角**：构造 Hardware Capability Vector（HCV），编码计算、存储、带宽、并行度等物理能力。
- 通过 **Hardware–Operation Dialogue Module（HODM）** 让算子特征与硬件特征双向交互，最后由 MLP 输出延迟。

### 2.2 DBSel：设备行为签名选择
- **目标**：从训练架构池中选出少量代表性架构，在目标设备上测量其延迟，构成紧凑的 DBS。
- **设备相似度**：对训练设备与测试设备的探测延迟向量，联合使用余弦相似度和 Pearson 相关得到混合相似度 \(S_{ij}\)。
- **设备权重**：对相似度做 z-score 标准化，经温度缩放 softmax 得到对齐权重 \(W_{ij}\)；再对测试设备平均，并与均匀先验插值，得到最终设备权重 \(w_i\)。
- **首个代表架构选择**：使用 Diversity–Coverage Score，平衡：
  - 在相似设备上的延迟方差（多样性）；
  - 平均延迟接近全局中位数（代表性，避免极端离群）。
- **后续架构选择**：采用加权最远点采样，综合“与已选集合的多样性距离”和“对延迟范围覆盖的增量增益”；后半程引入覆盖项权重 \(\alpha_{cov}\)。
- **算法流程**：随机抽取探测模型 → 计算设备相似度 → 初始化设备权重 → 选首个代表 → 迭代选 \(k\) 个代表 → 返回 DBS。

### 2.3 图交互建模
- 使用 Dense Graph Flow（DGF）与图注意力机制建模算子间结构依赖。
- DGF 更新同时包含：
  - 操作感知门控的邻域聚合；
  - 残差/恒等映射，缓解深层 GCN 过平滑。
- 图注意力分支通过中心节点与邻居节点的点积相似度计算注意力权重，经 LeakyReLU、softmax 聚合，并进行 LayerNorm 与操作感知门控。

### 2.4 HCV：硬件能力向量
- 编码 **Peak Throughput、Core-Level Parallelism、Frequency、Peak Bandwidth、On-chip Memory、DRAM/HBM Capacity** 等能力级指标。
- 对 GPU、FPGA 等不同设备族采用“原生指标—功能等价指标—不适用”的统一 schema。
- 对缺失维度进行零填充，并附带**可用性指示位**，以区分真实零值与不适用指标。
- HCV 提供物理可解释、可张量化的设备结构抽象。

### 2.5 HODM：硬件—算子对话模块
- 输入为 GNN 产生的算子特征张量 \(X\) 与硬件能力向量 \(H\)。
- **FiLM 调制**：用由 \(H\) 经 MLP 学到的 \(\gamma,\beta\) 对每个算子节点特征做通道级仿射变换，使预测器具备动态硬件感知。
- **双向交叉注意力**：
  - 以算子特征为 Query、硬件向量为 Key/Value，得到“网络如何查询硬件”的表示 \(X'\)；
  - 反向以硬件为 Query、算子为 Key/Value，得到“硬件关注哪些网络区域”的表示 \(H'\)；
  - 通过可学习门控 \(\lambda\) 融合：\(\text{OUTPUT}=\lambda X'+(1-\lambda)H'\)。
- 最终融合表示送入 MLP 预测绝对延迟。

## 3. 实验设计

- **数据集/搜索空间**：
  - **NASBench-201**：cell-based 搜索空间。
  - **FBNet**：macro-level 搜索空间。
  - 遵循 HW-NAS-Bench 的评测思路。
- **硬件平台**：
  - 桌面/云端 GPU：1080Ti、2080Ti、Titan RTX、Titan XP、P100、V100、P4 等。
  - 移动/边缘 SoC：Raspberry Pi 4、Samsung A50、Essential PH-1、Pixel 2、Jetson AGX Xavier、NX、TX2 等。
  - FPGA：Eyeriss FPGA、Xilinx ZC706。
  - NPU：Cambricon MLU270。
- **跨设备划分**：
  - 采用 FLAT 中的相关性感知设备划分：基于设备间延迟的负 Spearman 相关构图，谱二分得到不相似硬件组。
  - 形成 N@1–N@4（NASBench-201）与 F@1–F@4（FBNet）四组迁移设置；测试设备严格 held-out。
- **评价指标**：
  - **RAE**：相对近似误差，越低越好。
  - **CDF**：绝对预测误差累积分布。
  - **Coverage**：预测落在 5%、10%、15% 相对误差阈值内的比例。
- **对比方法**：
  - #FLOPs、LAD、BRP、nn-Meter、FLAT。
- **实验组**：
  - 表 1：FBNet F@1–F@4 多设备 RAE 对比。
  - 图 3：NASBench-201 N@1–N@4 在 5%/10%/15% 误差阈值下的覆盖率。
  - 图 4：FPGA ZC706、RTX 2080 Ti、Samsung A50 三平台 CDF。
  - 表 2：八种异构 SoC/云设备上与 FLAT 的 RAE 与覆盖率对比。
  - 表 3：四平台消融实验，比较 Baseline、Variant A/B/C 与完整模型。

## 4. 资源与算力

- 文中明确提到：所有模型在 **NVIDIA A6000 GPU** 上以相同设置训练。
- 但**未明确说明**：
  - 使用了多少张 A6000；
  - 单次训练/总训练时长；
  - GPU 小时数；
  - 探测与推理开销。
- 因此，算力规模只能确认 GPU 型号，无法量化总计算成本。

## 5. 实验数量与充分性

- **实验数量**：较为丰富，至少包括：
  - 2 个 NAS 搜索空间；
  - 4 组 FBNet 跨设备划分；
  - 4 组 NASBench-201 跨设备划分；
  - 3 个平台的 CDF 分析；
  - 8 个真实异构设备的对比；
  - 4 个平台的组件消融。
- **充分性**：
  - 覆盖 GPU、FPGA、移动 SoC、NPU 等多类设备，跨设备 hold-out 设计较严谨。
  - 消融实验较好地区分了 HCV/HODM、DBS 随机采样与 DBSel 代表性采样的贡献。
  - 使用开源 baseline 与统一训练设置，公平性较好。
- **仍需注意**：
  - 表 2 中并非所有设备均优于 FLAT，例如 NX、TX2、Cambricon MLU270 的部分指标不占优。
  - 未报告多次运行的均值/方差、统计显著性检验。
  - 未系统分析超参数敏感性、探测样本数 \(p\)、签名大小 \(k\) 的影响。
  - 对 Transformer、LLM、动态 shape、不同 batch size 等场景覆盖有限。

## 6. 主要结论与发现

- CloserToMe 在大多数跨设备设置下取得更低的 RAE 和更高的误差阈值覆盖率。
- 显式建模硬件能力与执行行为，比仅使用设备 ID 或黑盒回归更有效。
- DBS 的质量很关键：随机采样的 DBS 已有增益，但 DBSel 选择的代表性 DBS 效果最好；不具代表性的极端采样可能误导预测器。
- HCV + HODM 的结构化硬件建模本身已带来稳定提升；再加入 DBS 后进一步提升。
- 在困难平台（如 Titan RTX、Pixel 2 等）上相对 FLAT 有明显误差下降；但 Jetson TX2 因与训练设备硬件层级差异较大，误差仍偏高。

## 7. 优点

- **双视角设备建模**：同时刻画“物理结构能力”和“工作负载执行行为”，比单一设备 ID 编码更有信息量。
- **轻量探测思路**：DBSel 只需在目标设备上测量少量代表性架构，兼顾迁移性与 profiling 成本。
- **交互建模清晰**：HODM 使用 FiLM + 双向交叉注意力 + 门控融合，较自然地建模算子与硬件之间的双向依赖。
- **实验设计较严谨**：采用相关性感知的设备划分，测试设备严格未见，避免随机划分带来的乐观偏差。
- **消融有针对性**：区分了 HCV/HODM、DBS 随机采样、非代表性采样与 DBSel 的贡献。

## 8. 不足与局限

- **并非零样本迁移**：DBSel 仍需在目标设备上进行少量延迟探测，因此不是完全无需目标设备数据的预测。
- **HCV 依赖人工 schema**：指标选择、功能等价映射和零填充/可用性指示可能引入归纳偏置，跨设备族扩展需人工维护。
- **设备与工作负载覆盖有限**：主要验证 CNN/NAS 搜索空间，未充分覆盖 Transformer、LLM、动态控制流、动态 batch、多租户运行时等。
- **部分设备结果不占优**：在 NX、TX2、MLU270 等平台上相对 FLAT 的优势不稳定，说明极端异构设备仍是难点。
- **缺少成本与统计报告**：未报告训练时长、GPU 数量、预测器自身开销、重复实验方差和显著性检验。
- **动态编译/运行时优化建模仍有限**：虽然 HODM 试图捕捉执行交互，但对 tiling、fusion、调度策略等运行时行为的显式建模仍较间接。
- **应用限制**：若目标设备探测成本高、硬件指标难以获取或设备族差异过大，方法收益可能下降。

（完）
