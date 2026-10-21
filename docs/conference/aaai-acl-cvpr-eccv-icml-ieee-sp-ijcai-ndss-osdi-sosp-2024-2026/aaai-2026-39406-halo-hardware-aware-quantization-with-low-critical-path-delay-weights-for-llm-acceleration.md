---
title: "HALO: Hardware-Aware Quantization with Low Critical-Path-Delay Weights for LLM Acceleration"
title_zh: HALO：面向LLM加速的低关键路径延迟权重硬件感知量化
authors: "Rohan Juneja, Shivam Aggarwal, Safeen Huda, Tulika Mitra, Li-Shiuan Peh"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39406/43367"
tags: ["query:litert-ops"]
score: 5.0
evidence: 对齐MAC电路特性的硬件感知量化
tldr: 该文针对传统量化与硬件无关、忽略乘加单元时序与能耗特性的问题，提出硬件感知的训练后量化框架HALO。它显式引入关键路径延迟等电路级硬件特征来指导权重量化，从而利用可用时序余量与节能空间。研究旨在提升现代加速器上的部署效率，为量化算子与硬件协同的高效执行提供方法支撑。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-001.webp\", \"caption\": \"\", \"page\": 3, \"index\": 1, \"width\": 3000, \"height\": 1688}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-002.webp\", \"caption\": \"\", \"page\": 3, \"index\": 2, \"width\": 3000, \"height\": 1688}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-003.webp\", \"caption\": \"\", \"page\": 5, \"index\": 3, \"width\": 1250, \"height\": 1000}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-004.webp\", \"caption\": \"\", \"page\": 5, \"index\": 4, \"width\": 1250, \"height\": 1000}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-005.webp\", \"caption\": \"\", \"page\": 5, \"index\": 5, \"width\": 1250, \"height\": 1000}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-006.webp\", \"caption\": \"\", \"page\": 5, \"index\": 6, \"width\": 1500, \"height\": 250}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-007.webp\", \"caption\": \"\", \"page\": 5, \"index\": 7, \"width\": 281, \"height\": 1491}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-008.webp\", \"caption\": \"\", \"page\": 5, \"index\": 8, \"width\": 625, \"height\": 500}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-009.webp\", \"caption\": \"\", \"page\": 5, \"index\": 9, \"width\": 625, \"height\": 500}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-010.webp\", \"caption\": \"\", \"page\": 5, \"index\": 10, \"width\": 625, \"height\": 500}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-011.webp\", \"caption\": \"\", \"page\": 5, \"index\": 11, \"width\": 625, \"height\": 500}]"
motivation: 传统量化与硬件无关，忽略MAC单元时序与能耗，难以发挥加速器效率。
method: 提出硬件感知训练后量化框架，引入关键路径延迟等电路特征指导量化。
result: 借助硬件时序与能耗特征利用时序余量，提升加速器上的部署效率。
conclusion: 该方法把电路级硬件特性纳入量化，为量化算子高效硬件执行提供支撑。
---

## Abstract
Quantization is critical for efficiently deploying large language models (LLMs). Yet conventional methods remain hardware-agnostic, limited to bit-width constraints, and do not account for intrinsic circuit characteristics such as the timing behaviors and energy profiles of Multiply-Accumulate (MAC) units. This disconnect from circuit-level behavior limits the ability to exploit available timing margins and energy-saving opportunities, reducing the overall efficiency of deployment on modern accelerators. To address these limitations, we propose HALO, a versatile framework for Hardware-Aware Post-Training Quantization (PTQ). Unlike traditional methods, HALO explicitly incorporates detailed hardware characteristics, including critical-path timing and power consumption, into its quantization approach. HALO strategically selects weights with low critical-path-delays enabling higher operational frequencies and dynamic frequency scaling without disrupting the architecture's dataflow. Remarkably, HALO achieves these improvements with only a few dynamic voltage and frequency scaling (DVFS) adjustments, ensuring simplicity and practicality in deployment. Additionally, by reducing switching activity within the MAC units, HALO effectively lowers energy consumption. Evaluations on accelerators such as Tensor Processing Units (TPUs) and Graphics Processing Units (GPUs) demonstrate that HALO significantly enhances inference efficiency, achieving average performance improvements of 270% and energy savings of 51% over baseline quantization methods, all with minimal impact on accuracy.

---

## 论文详细总结（自动生成）

# HALO 论文结构化总结

## 1. 核心问题与研究动机

- **背景**：LLM 参数规模快速增长，推理成本高；量化是降低模型体积与计算开销的关键手段。
- **核心问题**：现有量化方法大多**硬件无关**，只关注位宽、离群值或敏感权重，把乘加单元（MAC）视为黑盒，忽略了电路级时序与能耗特性。
- **关键观察**：
  - MAC 单元在 TPU 等加速器中占芯片面积约 77–80%、功耗约 50–89%。
  - 不同量化权重值会导致不同的**关键路径延迟**和**开关活动**，从而影响可运行频率与功耗。例如论文中 8-bit 权重 `64` 的最坏延迟约 265 ps，可支持约 3.7 GHz；`-127` 的最坏延迟约 522 ps，仅支持约 1.9 GHz。
  - 在 systolic array 中，最慢 MAC 单元决定全局时钟频率，因此权重取值会间接限制整个 tile/阵列的 DVFS 机会。
- **整体含义**：论文试图把**量化—电路时序—DVFS 调度**统一起来，在保持 LLM 精度的同时，利用低关键路径延迟权重提升频率、降低能耗，且不改变现有加速器数据流。

## 2. 方法论：HALO 框架

### 2.1 核心思想

- HALO 是一个**硬件感知的训练后量化（PTQ）框架**，输入包括：预训练权重、校准数据、梯度、tile 大小、目标频率、MAC 时序/功耗特征、DVFS 配置和用户设计目标。
- 输出：一组**Pareto 最优的量化模型 + DVFS 调度方案**，可部署到 GPU、TPU 或自定义加速器。
- 总体流程：
  1. **敏感度感知均匀量化**：保留离群值和极敏感权重。
  2. **关键路径延迟感知非均匀量化**：按 tile 敏感度分类，将低敏感 tile 量化为可高频运行的权重集合。
  3. **自适应 DVFS**：按 tile 的频率类别分组执行，减少 DVFS 切换次数。

### 2.2 关键技术细节

#### 阶段一：敏感度感知均匀量化

- **离群值提取**：使用 \(3\sigma\) 规则识别离群权重。
- **显著权重提取**：基于 Fisher 信息矩阵近似 Hessian：
  \[
  F=\frac{1}{|D|}\sum_{d\in D} g_d g_d^\top,\quad H\approx F
  \]
  权重敏感度记为 \(\Lambda_W=F\)，保留 top 0.05% 的显著权重。
- 离群值与显著权重合计不到总权重的 0.5%，被单独提取，采用 per-channel 量化，并以稀疏矩阵形式存储，交由 SpMV 引擎执行。

#### 阶段二：关键路径延迟感知非均匀量化

- 权重矩阵按固定 tile 划分，默认 \(128\times128\)，也评估 \(64\times64\)、\(32\times32\)。
- 每个 tile 的敏感度用梯度平方和归一化计算：
  \[
  \Lambda_{T_k}=\frac{\sum_{i,j} g_{k,i,j}^2}{\text{tile rows}\times\text{tile cols}}
  \]
- 对每层 tile 敏感度排序并计算累计分布，动态确定阈值 \(k\)，使约 95% 的总敏感度被保留；高敏感 tile 保留较高精度，低敏感 tile 更激进量化。
- 低敏感 tile 仅使用 **9 个可支持约 3.7 GHz 的权重值**；高敏感 tile 使用 **16 个可支持约 2.4 GHz 的权重值**。同一 tile 内 MAC 统一频率，保证同步数据流。

#### 阶段三：自适应 DVFS

- 对离群/显著权重的 SpMV 部分，选择满足时序约束的最低能耗电压-频率点：
  \[
  (V,f)=\arg\min_{(V_i,f_i)} E(V_i,f_i),\quad \text{s.t. } 1/f_i\ge \text{Critical-Path-Delay}
  \]
- 对高/低敏感 tile，分别按其权重集合的关键路径延迟选择 DVFS 点。
- 将同一频率类别的 tile 分组连续执行，每个频率类别只进行一次 DVFS 转换，降低切换开销。
- 论文假设的 DVFS 等级：
  - GPU：\((0.9V,1.5GHz)\)、\((1.0V,2.0GHz)\)、\((1.1V,2.8GHz)\)。
  - Systolic Array/TPU 类：\((1.0V,1.9GHz)\)、\((1.1V,2.4GHz)\)、\((1.2V,3.7GHz)\)。

### 2.3 算法流程概括

1. 从权重和梯度中提取显著值与离群值；
2. 对显著值和离群值单独量化并稀疏化；
3. 将剩余权重 reshape 成 tile；
4. 计算每个 tile 的敏感度；
5. 通过自适应阈值生成高/低敏感 mask；
6. 低敏感 tile 按高频权重集合量化，高敏感 tile 按较高精度集合量化；
7. 合并量化结果，生成最终权重矩阵；
8. 离线确定各 tile 的频率类别与 DVFS 调度。

## 3. 实验设计

### 3.1 模型与数据集

- **模型**：LLaMA2-7B、LLaMA2-13B、OPT-1.3B、OPT-2.7B、OPT-6.7B、OPT-13B、OPT-30B。
- **数据集**：WikiText2、C4。
- **评价指标**：语言建模困惑度（perplexity），序列长度 2048；越低越好。
- **量化设置**：权重 W8/W4/W3，激活统一 8-bit；对 attention 和 linear 等计算密集层量化，激活采用 per-token 静态量化。

### 3.2 对比基线

- FP16；
- RTN W8A8、W4A8、W3A8；
- SmoothQuant W8A8、W4A8、W3A8；
- GPTQ W4A8；
- ZeroQuant Local/Global W4A8；
- HALO 的 perf-opt、acc-opt、bal 三种配置；
- tile 大小消融：128×128、64×64、32×32。

### 3.3 硬件与仿真平台

- **Systolic Array**：自定义 SystemVerilog 设计，支持全局 DVFS，22nm 工艺综合，行为级仿真。
- **GPU**：扩展 AccelSim 模拟 NVIDIA 2080 Ti，DVFS 使用论文表 1 设置；能耗用 AccelWattch 和 GPUWattch 估算。
- **指标**：归一化执行时间、归一化能耗、困惑度、Pareto 前沿。

## 4. 资源与算力

- 论文**未明确说明**使用的 GPU 数量、训练/校准时长、校准数据规模、总计算量或能耗测量所需的实际机器规模。
- 文中仅说明：
  - 使用 Synopsys 工具做 8-bit MAC 静态时序分析；
  - systolic array 用 SystemVerilog 实现并在 22nm 工艺下综合；
  - GPU 结果通过 AccelSim、AccelWattch、GPUWattch 模拟/建模得到。
- 因此，无法从论文中判断其完整实验的算力开销与可复现实验成本。

## 5. 实验数量与充分性

### 5.1 实验规模

- 表 2 覆盖 4 个模型 × 2 个数据集（共 8 个模型-数据集列），并对比 FP16、RTN、SmoothQuant、GPTQ、ZeroQuant 及 HALO 多种配置。
- 执行时间与能耗图覆盖 7 个模型（LLaMA2-7B/13B、OPT-1.3B/2.7B/6.7B/13B/30B）。
- 包含：
  - 精度对比实验；
  - systolic array 性能与能耗实验；
  - GPU 性能与能耗实验；
  - tile size 消融（128/64/32）；
  - Pareto 前沿分析；
  - 离群/显著权重执行开销分析（低于总推理时间 1%）。
- 总体约有数十组模型、数据集、量化配置和硬件配置组合。

### 5.2 充分性与公平性评价

- **优点**：
  - 覆盖了主流 LLM 家族和多个规模，包含 30B 级模型；
  - 同时评估 TPU 类 systolic array 与 GPU；
  - 对比了 RTN、SmoothQuant、GPTQ、ZeroQuant 等代表性方法；
  - 做了 tile 粒度消融和 Pareto 权衡分析。
- **局限**：
  - GPTQ、ZeroQuant 的部分结果来自已有文献，并非全部在同一实验设置下复现；
  - 主要评价困惑度，缺少下游任务、零样本/少样本、长上下文等任务级评估；
  - 未系统评估不同校准集、不同随机种子、统计显著性；
  - 硬件结果为仿真/综合估计，缺少流片或真实加速器实测；
  - 对 DVFS 切换开销的假设较理想化，未给出实测开销分布；
  - 与 W4A8/W3A8 比较时，HALO 的有效位宽约为 3.0–3.96 bit，需注意比较口径。

## 6. 主要结论与发现

- HALO 能在精度接近 FP16 的情况下，通过选择低关键路径延迟权重提升可运行频率并降低能耗。
- 摘要声称相对基线量化方法平均获得 **270% 性能提升**和 **51% 能耗节省**；正文图 8 还报告相对 FP16 约 **353% 加速**、相对其他量化基线最高约 **87%** 加速。
- 在 systolic array 上，HALO 的能耗接近 W3A8/W4A8 水平，但执行速度显著更快；即使运行在更高电压频率点，总能耗也未高于 W8A8。
- tile 越小，性能越好且大模型困惑度更优：32×32 相比 128×128 可再提升最高约 15%，相比 64×64 提升约 7%。
- 离群值和显著权重的稀疏处理开销低于总推理时间的 1%，主要收益来自 tile 级量化与 DVFS。
- GPU 上 HALO 同样降低执行时间，并在 perf-opt/bal 配置下平均降低能耗，说明方法具有一定跨硬件可移植性。

## 7. 优点

- **问题视角新颖**：首次系统性地把 MAC 关键路径延迟、开关功耗和 DVFS 纳入 LLM 后训练量化。
- **软硬件协同但无需改硬件**：作为软件级方法可部署在现有 GPU/TPU 类架构上，兼容现有数据流。
- **细粒度且可调度**：tile 级敏感度分类与频率分组，兼顾精度、性能和 DVFS 切换开销。
- **对极端权重处理合理**：离群值和显著权重单独稀疏化，避免其破坏整体低比特量化。
- **多目标输出实用**：提供 perf-opt、acc-opt、bal 等 Pareto 配置，适配不同部署目标。
- **实验覆盖较广**：包含多个模型规模、两类硬件、tile 消融和能耗拆解。

## 8. 不足与局限

- **硬件依赖较强**：方法依赖精确的 MAC 时序/功耗表征、单元库和工艺参数；换工艺、换 MAC 设计后需重新表征。
- **DVFS 假设较理想**：假设频率等级离散且切换开销低，未充分实测真实 GPU/TPU 上的 DVFS 延迟和调度开销。
- **仿真验证为主**：systolic array 为 22nm 综合与行为仿真，GPU 为较旧的 2080 Ti 模拟，缺少真实加速器端到端部署结果。
- **精度评估维度有限**：主要用 WikiText2/C4 困惑度，未覆盖下游任务、鲁棒性、安全性和长上下文表现。
- **激活量化仍固定 8-bit**：未深入处理激活离群值和激活侧硬件时序，方法主要针对权重。
- **与部分基线比较不完全同设置**：GPTQ/ZeroQuant 结果部分引用文献，公平性受一定限制。
- **可扩展性与通用性待验证**：高频权重值集合仅 9/16 个，是否适用于更多模型、不同量化位宽和更复杂算子仍需验证。
- **perf-opt 配置精度下降更明显**：性能优先配置的困惑度高于 acc-opt/bal，实际部署需按场景权衡。

（完）
