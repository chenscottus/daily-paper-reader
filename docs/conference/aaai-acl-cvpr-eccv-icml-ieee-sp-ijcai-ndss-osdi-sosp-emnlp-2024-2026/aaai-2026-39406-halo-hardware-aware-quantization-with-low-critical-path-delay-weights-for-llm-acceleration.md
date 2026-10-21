---
title: "HALO: Hardware-Aware Quantization with Low Critical-Path-Delay Weights for LLM Acceleration"
title_zh: HALO：面向LLM加速的低关键路径延迟权重量化
authors: "Rohan Juneja, Shivam Aggarwal, Safeen Huda, Tulika Mitra, Li-Shiuan Peh"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39406/43367"
tags: ["query:coral-npu"]
score: 4.0
evidence: 面向加速器高效部署的硬件感知量化
tldr: 量化是高效部署大模型的关键，但传统方法硬件无关、仅受位宽约束，未考虑乘累加单元的时序行为与能耗特性，限制了加速器上的效率。作者提出HALO，一种显式建模关键路径延迟等电路级硬件特性的训练后量化框架。该框架利用可用时序余量与节能空间，在保持精度的同时提升现代加速器上的部署效率。其硬件感知量化思路与在边缘加速器上部署量化模型的需求相关，但本文面向LLM与云端加速器场景。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-001.webp\", \"caption\": \"\", \"page\": 3, \"index\": 1, \"width\": 3000, \"height\": 1688}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-002.webp\", \"caption\": \"\", \"page\": 3, \"index\": 2, \"width\": 3000, \"height\": 1688}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-003.webp\", \"caption\": \"\", \"page\": 5, \"index\": 3, \"width\": 1250, \"height\": 1000}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-004.webp\", \"caption\": \"\", \"page\": 5, \"index\": 4, \"width\": 1250, \"height\": 1000}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-005.webp\", \"caption\": \"\", \"page\": 5, \"index\": 5, \"width\": 1250, \"height\": 1000}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-006.webp\", \"caption\": \"\", \"page\": 5, \"index\": 6, \"width\": 1500, \"height\": 250}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-007.webp\", \"caption\": \"\", \"page\": 5, \"index\": 7, \"width\": 281, \"height\": 1491}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-008.webp\", \"caption\": \"\", \"page\": 5, \"index\": 8, \"width\": 625, \"height\": 500}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-009.webp\", \"caption\": \"\", \"page\": 5, \"index\": 9, \"width\": 625, \"height\": 500}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-010.webp\", \"caption\": \"\", \"page\": 5, \"index\": 10, \"width\": 625, \"height\": 500}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39406/fig-011.webp\", \"caption\": \"\", \"page\": 5, \"index\": 11, \"width\": 625, \"height\": 500}]"
motivation: 传统量化方法硬件无关、仅限位宽约束，未考虑MAC单元时序与能耗等电路特性，限制了加速器部署效率。
method: 提出硬件感知训练后量化框架HALO，显式建模关键路径延迟等电路级硬件特性来指导权重量化。
result: 在保持模型精度的同时利用时序余量与节能空间，提升现代加速器上的部署效率。
conclusion: 打通了电路级硬件行为与量化模型部署之间的脱节。
---

## Abstract
Quantization is critical for efficiently deploying large language models (LLMs). Yet conventional methods remain hardware-agnostic, limited to bit-width constraints, and do not account for intrinsic circuit characteristics such as the timing behaviors and energy profiles of Multiply-Accumulate (MAC) units. This disconnect from circuit-level behavior limits the ability to exploit available timing margins and energy-saving opportunities, reducing the overall efficiency of deployment on modern accelerators. To address these limitations, we propose HALO, a versatile framework for Hardware-Aware Post-Training Quantization (PTQ). Unlike traditional methods, HALO explicitly incorporates detailed hardware characteristics, including critical-path timing and power consumption, into its quantization approach. HALO strategically selects weights with low critical-path-delays enabling higher operational frequencies and dynamic frequency scaling without disrupting the architecture's dataflow. Remarkably, HALO achieves these improvements with only a few dynamic voltage and frequency scaling (DVFS) adjustments, ensuring simplicity and practicality in deployment. Additionally, by reducing switching activity within the MAC units, HALO effectively lowers energy consumption. Evaluations on accelerators such as Tensor Processing Units (TPUs) and Graphics Processing Units (GPUs) demonstrate that HALO significantly enhances inference efficiency, achieving average performance improvements of 270% and energy savings of 51% over baseline quantization methods, all with minimal impact on accuracy.

---

## 论文详细总结（自动生成）

# HALO 论文结构化中文总结

## 一、核心问题与研究动机

- **背景张力**：Transformer 类 LLM 规模约每两年增长 100 倍，而硬件性能提升仅约 3.1×，推理成本急剧上升（LLaMA-65B、GPT-4-1.76T 等），量化成为降低显存与计算开销的关键手段。
- **核心问题**：现有量化方法（GPTQ、AWQ、SmoothQuant、ZeroQuant 等）本质上是**硬件无关**的算法层技术，只关注位宽、离群值与敏感权重，把乘累加单元（MAC）当作黑盒，忽略了电路级特性——**关键路径延迟（critical-path delay）与开关活动导致的功耗差异**。
- **关键观察（论文的立论基础）**：
  - 在 TPU 类加速器中，MAC 单元占芯片面积 77–80%、占总功耗 50–89%。
  - 同一 8-bit MAC 单元在不同量化权重下延迟差异巨大：权重 64 的最大延迟约 265 ps（可达 3.7 GHz），权重 −127 约 522 ps（仅 1.9 GHz）。
  - 功耗同样随权重位模式变化（开关活动不同），低延迟权重往往也低功耗。
  - 在脉动阵列中所有 MAC 同步锁步运行，**最慢单元决定全局时钟**，因此权重取值直接约束可用频率与 DVFS 空间。
- **整体含义**：量化不仅应"压位宽"，还应"挑选对电路友好的权重取值"，从而与 DVFS 联合优化，在几乎不损精度的前提下同时提升吞吐与能效。作者称 HALO 是首个把电路级时序与 DVFS 约束引入量化流程的框架。

## 二、方法论

### 核心思想
将训练后量化（PTQ）与 DVFS 统一为一个硬件驱动的优化问题：先按**权重敏感性**保护精度关键权重，再按**关键路径延迟**对剩余权重做非均匀量化并分块（tile），最后按 tile 的时序等级分配电压-频率档位，输出一组 Pareto 最优的"量化模型 + DVFS 调度"组合。

### 关键技术细节（三阶段，对应 Algorithm 1）

**Stage 1：敏感性感知的均匀量化（保护精度）**
- 用 3σ 规则识别离群值（outliers）；用 Fisher 信息矩阵近似 Hessian（H ≈ F），其中
  F = (1/|D|) Σ_{d∈D} g_d g_dᵀ，
  以权重敏感度 Λ_W = F 排序，保留 top 0.05% 的显著权重（salient）。
- 离群值 + 显著权重合计 < 0.5%，单独做 per-channel 量化，并以稀疏矩阵（val 值向量 + idx 索引向量）形式打包，交由 SpMV 引擎执行 res[i] = val[i] × b[idx[i]]。

**Stage 2：关键路径延迟感知的非均匀量化（挖掘硬件空间）**
- 权重张量切分为固定 tile（默认 128×128，另测 64×64、32×32）。
- Tile 敏感度用 Fisher 对角近似：
  Λ_{Tk} = Σ_{i,j} g²_{k,i,j} / (tile rows × tile cols)。
- 动态阈值映射：将层内 tile 敏感度降序排列、累积归一化，取使累积敏感度达到设定比例（如 95%）的索引比例 k 作为分界，生成布尔掩码，把 tile 分为高/低敏感两类。
- 低敏感 tile 只用 9 个可在 3.7 GHz 运行的权重取值（激进量化 + 激进超频）；高敏感 tile 只用 16 个 2.4 GHz 级别的取值（保精度、适度超频）。**同一 tile 内所有 MAC 共享同一频率**，从而不破坏同步数据流。

**Stage 3：自适应 DVFS**
- 对稀疏的离群/显著权重：其取值覆盖整个 8-bit 范围，故选择满足所有可能取值时序约束的最低能耗点：
  (V, f) = argmin E(V_i, f_i)，s.t. 1/f_i ≥ Critical-Path-Delay。
- 对高/低敏感 tile：分别绑定到对应 VF 档位；论文假设的档位为
  - GPU：(0.9 V, 1.5 GHz)、(1.0 V, 2.0 GHz)、(1.1 V, 2.8 GHz)
  - 脉动阵列（TPU 类）：(1.0 V, 1.9 GHz)、(1.1 V, 2.4 GHz)、(1.2 V, 3.7 GHz)
- **开销控制**：把同一频率档的 tile 分组连续执行，每个频率等级只触发一次切换；单次切换为数十纳秒到数微秒，远小于 LLM 推理时延（Llama2-13B 约 53 ms、OPT-30B 超 120 ms），因此开销可忽略。量化与频率分配均离线确定，执行顺序不影响数据流、时序与精度。
- 提供 perf-opt / acc-opt / bal 三种配置，对应不同精度-性能-能耗偏好，并给出 Pareto 前沿与"膝点"。

## 三、实验设计

- **模型**：LLaMA2-7B/13B；OPT-1.3B/2.7B/6.7B/13B/30B。
- **数据集**：WikiText2 与 C4，序列长度 2048。
- **指标**：困惑度（PPL）作为精度度量；归一化执行时间与归一化能耗（分 core / buffer / register / L2 / static 等分量）；另用有效位宽 B_eff = Σ P_i b_i 描述异构量化。
- **对比方法**：FP16 理想基线；RTN（W8A8 / W4A8 / W3A8）；SmoothQuant（W8A8/W4A8/W3A8）；GPTQ（W4A8）；ZeroQuant 的 ZQ-Local 与 ZQ-Global（W4A8）。激活统一 8-bit，仅权重量化。
- **硬件与仿真平台**：
  - 脉动阵列：自研模拟器 + SystemVerilog 实现（含全局 DVFS 控制），22 nm 工艺综合，行为级仿真验证。
  - GPU：扩展 AccelSim 建模 NVIDIA 2080 Ti，能耗用 AccelWattch / GPUWattch 估算。
- **消融维度**：tile 大小（128/64/32）、三种配置（perf-opt/acc-opt/bal）、精度-性能 Pareto 曲线、能耗分量拆解、稀疏权重处理的时间占比。

## 四、资源与算力

- 论文**未明确报告**训练/校准所需的 GPU 型号、数量或总机时。HALO 属于训练后量化，仅需少量校准数据计算梯度（Fisher 近似），算力需求本应很低，但具体规模未给出。
- 评估侧全部为**仿真/综合**：22 nm 综合的脉动阵列设计 + SystemVerilog 行为仿真；GPU 侧为 AccelSim/AccelWattch/GPUWattch 建模的 RTX 2080 Ti。**没有在真实 TPU/GPU 上做端到端实测**。
- 因此，论文的"270% 性能提升、51% 节能"均来自模拟器估算，而非硅上测量。

## 五、实验数量与充分性

- **规模**：Table 2 覆盖 4 个模型 × 2 个数据集 = 8 组精度实验，每组含约 10 种方法/配置；图 8、10 各覆盖 7 个模型的时延与能耗；图 9 给出 4 个模型的 Pareto 曲线；图 11 为 6 个模型 × 4 种 tile 配置的时延对比；图 12、13 为 GPU 上 7 个模型 × 4 种配置的时延与能耗。整体约 30–40 组主要实验，覆盖面在同类工作中属中等偏上。
- **充分之处**：模型家族、规模跨度（1.3B–30B）较广；同时给出精度、时延、能耗三视角；有 tile 粒度消融与能耗分量拆解；Pareto 分析使权衡关系清晰。
- **不足与公平性风险**：
  - **比较基准不对等**：HALO 的加速主要来自"允许更高频率/更多 DVFS 档位"，而 W8A8 等基线基本在固定（标称）频率下评估，二者并非同一约束下的对照；"相对基线 270% 提升"的归因需要谨慎解读。
  - **部分基线数据直接引用**他人论文（GPTQ、ZQ-Local/Global 标注 * 取自 Yao et al. 2024），并非在同一套流程、同一校准集下复现，且仅覆盖部分模型，横向可比性有限。
  - **缺少真实硬件验证**与端到端实测延迟；DVFS 档位是假设值而非实测标定。
  - **缺少下游任务评估**（仅困惑度），也没有指令微调/生成质量评测。
  - 没有与传统 PTQ + 相同 DVFS 策略的组合对照，因而难以拆分"量化贡献"与"DVFS 贡献"。

## 六、主要结论与发现

- 权重取值显著影响 MAC 的关键路径延迟与功耗，因而量化与 DVFS 存在此前被忽视的联合优化空间。
- HALO 在多数模型上把 PPL 退化控制在 0.5 以内（acc-opt / bal 配置），同时实现平均 **270% 性能提升**、平均 **51% 节能**；相对 FP16 最高约 353% 加速，相对其他量化基线最高约 87% 加速。
- 稀疏（离群/显著）权重处理耗时不到总推理时间的 1%，主要收益来自 tile 级策略。
- 能耗方面：HALO 能耗在 W3A8 的 12% 以内、W4A8 的 10% 以内，但速度快得多；即使运行在更高 VF 点，总能耗也不高于 W8A8——因为更高频率带来的动态功耗被 MAC 内部开关活动下降所抵消。
- **更小的 tile 更好**：32×32 相比 128×128 最高再快 15%，相比 64×64 快 7%，且大模型上 PPL 也更优，说明细粒度时序控制同时利于硬件效率与精度。
- GPU 上同样有效，模型越大收益越明显（量化瓶颈与访存压力更严重时，硬件感知量化的价值更大）。
- bal 配置对应 Pareto 前沿的"膝点"，是实际部署的推荐折中。

## 七、优点

- **视角新颖**：把"电路级时序/功耗"引入量化决策，跳出了纯位宽与离群值处理的思路，属于算法-架构协同设计（co-design）的有益探索。
- **可落地性考虑较周到**：保持 tile 内统一时钟、按频率分组调度、每档只切换一次，兼容现有脉动阵列数据流；作者强调其为"软件层、正交于硬件"的技术，不需改动加速器架构。
- **精度保护机制合理**：3σ 离群值 + Fisher 信息敏感度 + 动态（按

累积敏感度）阈值划分 tile 的组合，既把极少数关键权重隔离保护，又避免全局统一高位宽的浪费，层次清晰、可解释性强。

- **实验维度较全**：覆盖 1.3B–30B 多个模型家族、两个数据集、精度/时延/能耗三视角，并有 tile 粒度、配置档位、能耗分量的消融，Pareto 前沿分析使“精度—性能—能耗”的权衡关系直观可读。

- **工程细节有交代**：给出了稀疏权重的打包格式与 SpMV 执行方式、频率切换的分组策略与开销量级估算、三种预设配置的适用场景，读起来像一份可实现的方案而非纯概念框架。

- **诚实地报告了反直觉结果**：例如“运行在更高 VF 点总能耗却不高于 W8A8”“更小 tile 反而精度更好”，并给出机理解释（开关活动下降、时序控制更细），这类发现对后续 co-design 研究有参考价值。

## 八、局限与不足

- **硬件验证缺位**：全部结论来自 22 nm 综合 + 行为级仿真与 AccelSim/AccelWattch 建模，无硅上实测。模拟器对关键路径延迟、开关功耗、DVFS 切换开销的建模误差会直接传导到“270% 加速、51% 节能”这类核心数字上，外推性存疑。

- **DVFS 档位为假设值**：三档 (V, f) 组合并非由真实芯片标定或实测得到，而是论文设定的工作点；实际芯片的频率—电压曲线、供电网络与散热约束会显著改变可用档位，进而影响 tile 划分的最优性与收益幅度。

- **基线对照不对等**：HALO 的加速在很大程度上源于“允许超频 + 多档 DVFS”，而 RTN/SmoothQuant/GPTQ/ZeroQuant 等基线多在固定标称频率下评估。缺少“相同量化方法 + 相同 DVFS 策略”的对照组，因此难以把收益拆分为“量化贡献”与“DVFS 贡献”，270% 这一数字的归因需谨慎。

- **基线数据部分为引用值**：GPTQ 与 ZQ-Local/Global 的结果标注取自他人工作，未在同一校准集、同一评估流程下复现，且只覆盖部分模型，横向可比性受限。

- **精度评估单一**：仅用 WikiText2/C4 的困惑度，缺少下游任务（问答、推理、代码、长文本生成）与指令微调模型的评测。PPL 退化 < 0.5 不等于下游能力无损，尤其对 3-bit 级有效位宽的配置。

- **有效位宽与稀疏开销的边界情形讨论不足**：稀疏（离群 + 显著）权重虽 < 0.5%，但其 SpMV 走的是独立引擎，若未来稀疏比例上升或 batch 很小，这部分开销占比可能不再是“< 1%”；论文未给出该结论的敏感性分析。

- **通用性受限**：方法强依赖“脉动阵列锁步 + 最慢单元决定时钟”这一前提，对采用异步电路、细粒度时钟门控或近存计算架构的加速器，其收益机理与 tile 划分策略需要重新推导。

- **缺少与硬件感知量化同类工作的直接对比**：论文主要与硬件无关的 PTQ 方法比较，未与已有考虑硬件代价的量化/调度方法在同条件下对照，新颖性的“相对增益”不够清晰。

## 九、可借鉴的思路与后续方向

- **把“物理代价”纳入量化目标函数**：以关键路径延迟、开关活动作为正则或约束项，与敏感度共同决定权重取值集合，这一范式可迁移到其他电路族（乘法器、加法树、SRAM 读写）与其它精度格式（FP8、MX 系列微缩放格式）。

- **tile 级异构精度 + 频率绑定**：以 tile 为粒度同时决定“位宽/取值集合”与“VF 档位”，既保持数据流同步性又获得细粒度收益，是一个结构上干净的 co-design 切入点。

- **动态阈值划分策略**：用累积敏感度比例（如 95%）自动确定高/低敏感分界，避免手工设定阈值，这类自适应划分在其它分层优化问题中也可复用。

- **可补齐的实验**：(1) 在真实 GPU/TPU 上做端到端实测并标定 VF 表；(2) 增加“同量化 + 同 DVFS”的消融以拆分贡献；(3) 补充下游任务评测与小 batch/大 batch 的敏感性分析；(4) 与硬件感知量化基线同条件对比。

## 十、总体评价

论文抓住了一个被现有量化研究普遍忽略的层面——权重取值对电路时序与功耗的影响，并据此提出“敏感度保护 + 延迟感知非均匀量化 + tile 级 DVFS”的三段式框架。问题定义清晰、动机有硬件测量支撑、方案在保持脉动阵列数据流的前提下具备可实现性，属于算法—架构协同设计中思路新颖且工程考量较完整的工作。

主要短板在于验证强度：核心收益数字全部来自仿真与假设的 VF 档位，且与基线的对照条件不对等，因此“270% 性能提升、51% 节能”应理解为在该论文特定建模假设下的上限估计，而非可直接复现的实测结论。若能在真实硬件上完成标定与端到端验证，并补齐下游任务评估与贡献拆解，该工作的说服力与影响力会显著提升。

总体而言，这是一篇**观点有价值、方法成体系、验证尚待加强**的工作，适合作为“量化 × 硬件时序”交叉方向的起点性参考，其结论宜作为方向性指引而非定量基准来引用。

（完）
