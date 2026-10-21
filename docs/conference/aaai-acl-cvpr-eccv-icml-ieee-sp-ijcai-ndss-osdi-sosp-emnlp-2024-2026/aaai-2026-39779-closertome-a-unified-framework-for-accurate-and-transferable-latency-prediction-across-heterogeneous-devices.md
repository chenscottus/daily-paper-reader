---
title: "CloserToMe: A Unified Framework for Accurate and Transferable Latency Prediction Across Heterogeneous Devices"
title_zh: CloserToMe：跨异构设备准确且可迁移的延迟预测统一框架
authors: "Cheng Tang, Guochong Sui, Wenqi Lou, Zihan Wang, Jiayi Tuo, Wenqian Xie, Yinkang Gao, Yixuan Zhu, Lei Gong, Chao Wang, Xuehai Zhou"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39779/43740"
tags: ["query:coral-npu"]
score: 5.0
evidence: 面向NPU与边缘加速器后端的延迟预测
tldr: 在云边异构设备上为神经网络挑选最优优化策略需要准确高效的延迟预测，但现有方法把硬件视为黑盒直接回归延迟，忽略网络结构与设备执行行为的交互。作者提出CloserToMe统一建模框架，显式刻画结构与硬件执行特性的关系以提升跨设备迁移能力。该工作有助于在含NPU、FPGA等边缘加速器上快速选择部署策略，支撑低延迟推理优化。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39779/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 3003, \"height\": 1435}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39779/fig-002.webp\", \"caption\": \"\", \"page\": 3, \"index\": 2, \"width\": 3980, \"height\": 2034}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39779/fig-003.webp\", \"caption\": \"\", \"page\": 6, \"index\": 3, \"width\": 2316, \"height\": 417}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39779/fig-004.webp\", \"caption\": \"\", \"page\": 6, \"index\": 4, \"width\": 1909, \"height\": 653}]"
motivation: 异构边缘与云端加速器上需要准确延迟预测来快速选择模型优化策略。
method: 提出统一建模框架，显式建模神经网络结构与设备执行行为的交互关系。
result: 实现跨GPU、NPU、FPGA等异构设备的准确且可迁移的延迟预测。
conclusion: 为跨硬件后端的优化策略选择提供了更可靠的延迟建模支撑。
---

## Abstract
Hardware accelerators such as GPUs, NPUs, and FPGAs are essential to meeting AI’s computational demands. With the proliferation of heterogeneous devices across cloud and edge, various model optimization techniques adapt to diverse hardware characteristics through operator transformations and structural modifications.  Accurate, efficient latency prediction enables rapid selection of optimal strategies across hardware backends.
Many existing methods treat hardware as a black-box executor, directly regressing latency without explicitly modeling the intricate interactions between neural network (NN) structures and device-specific execution behaviors. To address these challenges, we introduce a new modeling perspective that captures the interaction between neural architectures and hardware execution. To capture device-specific characteristics, we propose two complementary modeling strategies. The Device Behavior Signature Selector (DBSel) characterizes hardware execution behavior by selectively probing a small set of representative architectures, forming a compact, workload-driven profile. In parallel, we construct capability vectors that capture the hierarchical memory of each device and compute characteristics, providing a structured abstraction of its architectural capacity. To unify both behavioral and structural views, we introduce the Hardware–Operation Dialogue Module (HODM), which models fine-grained interactions between neural operators and hardware properties. Together, these components empower CloserToMe to deliver accurate and transferable latency predictions across unseen and diverse platforms.

---

## 论文详细总结（自动生成）

# CloserToMe 论文中文结构化总结

## 1. 核心问题与研究背景

- **应用背景**：GPU、NPU、FPGA、CPU 及移动 SoC 等异构加速器已成为承载 AI 计算的主要平台。模型优化技术（算子替换、结构改写、图级优化）需要针对不同硬件的执行特性做适配，因此“快速、准确、可迁移”的推理延迟预测是跨后端选型与调度决策的关键前提。
- **核心痛点**：
  - 早期方法用 FLOPs 等代理指标间接估计延迟，误差大。
  - 算子级查表法（如 ProxylessNAS、Once-for-All）与 kernel 级回归加和（如 nn-Meter）忽略端到端执行路径，无法反映图级优化与硬件调度带来的延迟变化。
  - 端到端预测器（LAD、BRP）依赖大规模 profiling（如 10 万样本）与设备特定分布，换硬件需重训练。
  - FLAT/FLAN 虽通过 few-shot 适配与编码设计改善迁移，但仍**把设备表示为不透明的可学习标识符**，没有显式建模“网络结构 × 设备执行行为”的交互，且更偏重延迟排序而非绝对延迟。
- **论文主张**：应从“硬件黑盒回归”转向“结构能力 + 行为特征 + 算子-硬件交互”的显式联合建模，从而在**未见过的异构设备**上同时保证绝对延迟精度与可迁移性。

## 2. 方法论

### 2.1 总体框架

- 流程：先用 **DBSel** 在目标（测试）设备上探测少量代表性架构，得到 **DBS**（Device Behavior Signature）→ 与网络结构编码拼接 → 经 **DGF + 图注意力** 的两级 GNN 提取算子级/负载级特征 → 与 **HCV**（Hardware Capability Vector）共同送入 **HODM** → 融合表示经 MLP 回归绝对延迟。
- 设计哲学：**行为视角（DBS，工作负载驱动的动态响应）+ 结构视角（HCV，物理可解释的静态能力）** 双通道互补，再由 HODM 统一。

### 2.2 DBSel：代表性签名选择

- 动机：同一网络在不同设备上因调度策略、算子融合、底层流水线差异而延迟迥异，需用极少量探测刻画设备“性格”。
- 三个组件：
  1. **设备级相似度建模**：对随机抽取的 p 个探针架构，在训练设备与测试设备上取延迟向量，用**余弦相似度与皮尔逊相关的混合分数** \(S_{ij}\) 度量训练设备 i 与测试设备 j 的相似性。
  2. **温度缩放权重初始化**：对 \(S_{ij}\) 按每个测试设备做 z-score 标准化（均值 \(\mu_j\)、标准差 \(\sigma_j\)），乘缩放因子 \(\alpha_{sim}\) 后过温度 softmax 得对齐权重 \(W_{ij}\)；再与均匀先验按 \(\beta\) 插值，并对所有测试设备取平均，得到设备重要性权重 \(w_i\)（式 1）。
  3. **多样性–覆盖度感知采样**：
     - 首个代表架构由 `DiversityCoverageScore` 选出（式 2）：第一项为按 \(w_d\) 加权的跨设备延迟方差（鼓励高判别性），第二项奖励平均延迟接近全局中位数的候选（避免极端离群），由 \(\gamma\) 权衡。
     - 后续架构用**加权最远点采样**迭代选取：候选需最大化“与已选集合的最小加权距离 \(d_i^{(t)}\)”与“增量覆盖增益 \(c_i^{(t)}\)（相对当前延迟范围的外扩程度）”的组合目标；迭代过半（\(t>k/2\)）后切换为覆盖优先的加权形式，由 \(\alpha_{cov}\) 控制（算法 1）。
- 输出：k 个代表架构在目标设备上的延迟向量即 DBS，作为该设备的紧凑行为画像。

### 2.3 图交互建模（结构编码）

- 基于 **Dense Graph Flow (DGF)**，节点特征由算子嵌入 \(O\) 与邻接矩阵 \(A\) 驱动：\(X^{l+1}=\sigma(OW_o^l)\odot(A X^l W_f^l)+X^lW_f^l+b_f^l\)，即“算子感知门控 × 邻域聚合 + 残差恒等路径”（式 3）。
- 叠加**成对图注意力**分支：\(W_p\) 投影到注意力空间，中心节点与邻居做点积打分，LeakyReLU 激活后 softmax 聚合（式 4），再经算子门控与 LayerNorm（式 5）；借鉴 GCNII 的残差/恒等映射以缓解深层过平滑、保留高阶邻域依赖。
- 两级 GNN：第一级刻画结构语义，第二级从工作负载视角进一步精炼延迟相关特征。

### 2.4 HCV：硬件能力向量

- 观察：尽管微架构差异巨大，GPU/MLU/移动 SoC 等均可映射为**统一的“计算层 + 存储层”两级抽象**（多级存储层次、并行核、MAC 单元）。
- 统一 schema 覆盖：峰值吞吐、核级并行度、频率、峰值带宽、片上存储容量、DRAM/HBM 容量；同时纳入线性尺度特征（如 clock MHz、FP32 TFLOPS）与架构级指标（如 GPU 的 SM 数/每 SM 的 FP32 AU、FPGA 的 DSP 数量、层间带宽 GB/s）。
- 填充规则：原生有该指标则直填；无对应但有**功能等价资源**则用等价量替代；否则标记为不适用。所有维度**数值零填充 + 可用性指示位**，使模型能区分“真实为零”与“未定义/无关”，并保持可张量化。

### 2.5 HODM：硬件–算子对话模块

- **FiLM 调制**：由 HCV 经两个共享 MLP 生成 \(\gamma,\beta\)，对每个算子节点做通道级仿射变换 \(\tilde X=\gamma\odot X+\beta\)，赋予动态硬件感知；在未见平台上仅需少量样本微调 \(\gamma,\beta\) 即可快速适配。
- **双向交叉注意力**：
  - 正向：\(\tilde X\) 作 Query，HCV 作 Key/Value，缩放点积 + softmax 聚合得 \(Z\)，再以可学习标量 \(\alpha_{res}\) 做残差融合得 \(X'\)（式 7–8）——表达“网络如何查询硬件”。
  - 反向：HCV 作 Query、\(\tilde X\) 作 Key/Value，得 \(H'\)——表达“硬件关注网络的哪些区域”。
- **门控融合**：\(\text{OUTPUT}=\lambda X'+(1-\lambda)H'\)，\(\lambda\in[0,1]\) 可学习（式 9）。

## 3. 实验设计

- **数据集 / 搜索空间**：沿用 HW-NAS-Bench 设置，覆盖 **NASBench-201**（cell-based）与 **FBNet**（macro-level）两类搜索空间；另自建扩展数据集，覆盖 8 个真实异构 SoC / 云端平台。
- **设备划分（关键设计）**：不采用随机划分，而沿用 FLAT 的**相关性感知划分**——按设备间延迟的负 Spearman 相关构建相似度图，用谱二分产生低相关硬件组，得到 **N@1–N@4（NASBench-201）与 F@1–F@4（FBNet）** 四种划分，测试设备全程不参与训练。
- **被测设备**：1080Ti（1/32 batch）、2080Ti（1/32）、Titan RTX（1/32）、Titan XP、Eyeriss FPGA、Raspberry Pi 4、Samsung A50、Essential PH-1、Pixel 2；扩展集含 Jetson AGX、Jetson NX、Jetson TX2、Tesla P4、P100、Titan XP、V100、寒武纪 MLU270。
- **评价指标**：RAE（\(|\hat y-y|/y\)）、绝对误差 CDF、以及 5%/10%/15% 误差阈值下的预测覆盖率。
- **对比基线**：#FLOPs、LAD、BRP、nn-Meter、FLAT（均为开源实现，统一在 A6000 上评测）。

## 4. 资源与算力

- 文中仅说明：**所有模型在 NVIDIA A6000 GPU 上训练，所有基线也在 A6000 上评测**。
- **未披露**：GPU 数量、单次训练时长、总 GPU 小时、参数量/模型规模、DBSel 探测的 p 与 k 取值、探测开销（在目标设备上实际运行的测量成本）等。
- 因此该工作的**训练成本与探测成本无法从论文中定量评估**，这对“轻量、快速适配”的核心卖点而言是一个信息缺口。

## 5. 实验数量与充分性

- **规模概览**：
  - 表 1：F@1–F@4 四个划分、合计约 34 个“设备×划分”条目，5 个基线 + 本文方法，报告均值±标准差。
  - 图 3：NASBench-201 的 N@1–N@4，三个误差阈值下的覆盖率对比。
  - 图 4：FPGA（ZC706）、RTX 2080Ti、Samsung A50 三平台的绝对误差 CDF。
  - 表 2：8 个真实平台的 RAE + 三档覆盖率，与 FLAT 逐项对比。
  - 表 3：5 个变体（Baseline / A / B / C / Ours）× 4 个平台的消融。
- **充分性评价**：
  - **优点**：覆盖 cell-based 与 macro-level 两类搜索空间、CPU/GPU/FPGA/NPU/移动 SoC 多硬件族、多种 batch size；采用低相关设备划分，泛化评估比随机划分更严苛、更客观；消融呈递进式（先验结构建模 → 随机 DBS → 劣质 DBS → DBSel），能分离各组件贡献；表 1 给出标准差，具备一定统计信息。
  - **不足**：对比基线止于 FLAT，论文虽在引言提及 FLAN 却未纳入实验；缺少对探针数量 p、代表数 k、\(\gamma/\beta/\alpha_{cov}/\tau\) 等超参的敏感性分析；未做多次随机种子/多划分重复的显著性检验；未验证“延迟预测 → 下游 NAS 选型收益”的端到端闭环；表 2 仅与 FLAT 单基线对比，说服力弱于表 1。

## 6. 主要结论与发现

- **整体精度领先**：在表 1 绝大多数设备上取得最低 RAE，且在 F@1、F@2 两个划分的**全部 9 个设备**上均排名第一。
- **困难平台收益显著**：Titan RTX（F@3）相对 FLAT 误差降低约 **64%**；移动端 Pixel 2（F@4）降低超 **53%**。
- **NASBench-201 上覆盖率优势明显**：N@3 上 15% 误差阈值内达 **75.39%**，超 FLAT **17.75 个百分点**；N@4 等困难设定同样保持优势。
- **真实平台验证**：8 设备中多数 RAE 更低（如 Titan XP 0.1604→0.1200、V100 0.1678→0.1546），三档误差覆盖率在几乎所有平台提升（如 P100 的 15% 覆盖率 55.88%→69.00%）。
- **失败案例与解释**：Jetson TX2 上误差反而高于 FLAT（1.0610 vs 0.8194），作者归因于其与训练设备在硬件层次与算力上的距离过大——说明方法在极端分布外设备上仍有边界。
- **消融结论**：HCV+HODM（Variant A）已稳定优于 one-hot 标识符基线；
