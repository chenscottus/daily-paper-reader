---
title: "Going Beyond the Edge: Distributed Inference of Transformer Models on Ultra-Low-Power Wireless Devices"
title_zh: 超越边缘：超低功耗无线设备上Transformer模型的分布式推理
authors: "Alexander Gräfe, Ding Huo, Vincent de Bakker, Johannes Berger, Marco Zimmerling, Sebastian Trimpe"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://www.ijcai.org/proceedings/2026/0014.pdf"
tags: ["query:coral-npu"]
score: 4.0
evidence: 在超低功耗无线嵌入式设备上分布式执行Transformer推理
tldr: Transformer模型在物联网中日益重要，但其计算与内存需求远超单个超低功耗设备的能力。作者提出CATS框架，让多台超低功耗无线设备协同执行远超单机容量的模型，核心是跨模型划分、无线通信与训练协同设计的通信感知分布式推理方案，并提出选择性广播激活列的剪枝通信原语SomeGather以降低通信开销。该工作为极低功耗嵌入式场景下的大模型推理提供了可行路径。
source: IJCAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/ijcai-2026-accepted/ijcai-2026-14/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 1285, \"height\": 195}, {\"url\": \"assets/figures/ijcai-2026-accepted/ijcai-2026-14/fig-002.webp\", \"caption\": \"\", \"page\": 2, \"index\": 2, \"width\": 570, \"height\": 439}, {\"url\": \"assets/figures/ijcai-2026-accepted/ijcai-2026-14/fig-003.webp\", \"caption\": \"\", \"page\": 2, \"index\": 3, \"width\": 677, \"height\": 369}, {\"url\": \"assets/figures/ijcai-2026-accepted/ijcai-2026-14/fig-004.webp\", \"caption\": \"\", \"page\": 2, \"index\": 4, \"width\": 661, \"height\": 378}, {\"url\": \"assets/figures/ijcai-2026-accepted/ijcai-2026-14/fig-005.webp\", \"caption\": \"\", \"page\": 2, \"index\": 5, \"width\": 1087, \"height\": 230}, {\"url\": \"assets/figures/ijcai-2026-accepted/ijcai-2026-14/fig-006.webp\", \"caption\": \"\", \"page\": 2, \"index\": 6, \"width\": 554, \"height\": 451}, {\"url\": \"assets/figures/ijcai-2026-accepted/ijcai-2026-14/fig-007.webp\", \"caption\": \"\", \"page\": 2, \"index\": 7, \"width\": 696, \"height\": 247}, {\"url\": \"assets/figures/ijcai-2026-accepted/ijcai-2026-14/fig-008.webp\", \"caption\": \"\", \"page\": 2, \"index\": 8, \"width\": 574, \"height\": 436}, {\"url\": \"assets/figures/ijcai-2026-accepted/ijcai-2026-14/fig-009.webp\", \"caption\": \"\", \"page\": 2, \"index\": 9, \"width\": 1483, \"height\": 834}, {\"url\": \"assets/figures/ijcai-2026-accepted/ijcai-2026-14/fig-010.webp\", \"caption\": \"\", \"page\": 2, \"index\": 10, \"width\": 631, \"height\": 1122}, {\"url\": \"assets/figures/ijcai-2026-accepted/ijcai-2026-14/fig-011.webp\", \"caption\": \"\", \"page\": 4, \"index\": 11, \"width\": 903, \"height\": 277}]"
motivation: Transformer的计算与内存需求远超单个超低功耗物联网设备的能力上限。
method: 提出CATS，跨模型划分、无线通信与训练协同设计分布式推理，并用剪枝通信原语SomeGather降低通信量。
result: 使多台超低功耗无线设备可协同运行远超单机容量的Transformer模型。
conclusion: 为极低功耗嵌入式设备上的大模型推理提供了分布式协同方案。
---

## Abstract
Transformer models are rapidly becoming a cornerstone of modern Internet of Things (IoT) applications, yet their computational and memory demands far exceed the capabilities of a single typical ultra-low-power IoT device. We present CATS, a framework for distributed transformer inference on ultra-low-power wireless devices, enabling multiple devices to collaboratively execute models far larger than what a single device can sustain. At its core, CATS is a communication-aware distributed transformer inference scheme co-designed across transformer partitioning, wireless communication and training. It employs SomeGather, a new pruned communication primitive that selectively broadcasts activation columns to reduce communication bandwidth and RAM usage without sacrificing model accuracy. Building on SomeGather, we design a partitioning method that exploits this primitive for efficient model parallelism. To cope with unreliable wireless communication, CATS employs message-dropout during training, which mimics packet losses and yields models that are robust to message loss during inference. In real-world experiments, we show that CATS brings distributed transformer inference to ultra-low-power wireless devices for the first time, with deployments on up to 16 devices that collaboratively execute transformer models up to 14 times larger than what a single device can run.

---

## 论文详细总结（自动生成）

# 论文总结：Going Beyond the Edge: Distributed Inference of Transformer Models on Ultra-Low-Power Wireless Devices

## 1. 核心问题与研究动机

- **背景**：Transformer 已成为 NLP、视觉、时序分析等 IoT 智能应用的核心模型，但其计算与内存需求远超单个典型超低功耗 IoT 设备的能力上限。
- **现有 DTI（分布式 Transformer 推理）的错位**：已有工作主要面向 Raspberry Pi、NVIDIA Jetson、笔记本/虚拟机，或带张量加速器的专用 MCU（如 Siracusa RISC-V 多核），且普遍假设**高速有线链路**。
- **真实部署场景**：无线传感器网络（环境监测、基础设施、农业）依赖小型电池/能量采集，设备是内存与算力极低的 MCU，通过 BLE 等低功耗无线接口组成多跳 mesh 网络。
- **三大未解挑战**：
  - **C1 算力受限**：Flash 存权重、RAM 存中间激活、计算吞吐三者同时紧张。
  - **C2 带宽受限**：低带宽、高延迟、多跳转发使通信成为主导瓶颈。
  - **C3 通信不可靠**：无线丢包会直接丢失中间激活，显著降低精度。
- **现有方法的缺口**：有的只降 RAM 不管 Flash（Voltage、Astra），有的只降 Flash（SiracusaDTI 依赖 AllReduce，在 mesh 中 RAM/通信反而随设备数线性增长）；通信约束多靠假设分组拓扑规避；**全部假设无损通信**。
- **结论性判断**：目前不存在面向超低功耗无线设备的 DTI 方法，本文提出 CATS 填补这一空白。

## 2. 方法论：CATS 框架

### 2.1 核心思想

- 将**模型划分、无线通信、训练**三者围绕一个剪枝式 all-to-all 通信原语 **SomeGather** 联合设计。
- 通过通信感知地剪掉整列激活，使部分特征子空间完全本地处理、仅共享精选子集，从而**同时**降低通信量、单设备激活 RAM、Flash 占用与计算负载。
- 推理被组织为严格同步的「计算—通信」轮次：每台设备先算本地分片，再用 SomeGather 交换部分激活。

### 2.2 SomeGather 通信原语

- 以列切分矩阵乘法 `B = AW` 为例：传统 AllGather 需交换 A 的全部列，RAM 为 `O(C_A + C_W/D)`、通信为 `O(C_A)`。
- SomeGather 只广播 A 的**部分列**，未传输列不产生流量、纯本地计算。
- 由于底层 Mixer 是「一发全收」的广播语义，剪枝粒度被约束为**按设备整体剪除其所有出边**，而非细粒度链路剪枝。形式化为：
  - 每台设备 d 维护二值向量 `p_d ∈ {0,1}^{S×1}`；
  - 剪枝矩阵 `P` 的对角块为全 1（保留设备内连接），非对角块由各设备的 `p_d` 填充；
  - 剪枝后乘法写作 `B = A(W ⊙ P)`（⊙ 为 Hadamard 积）。
- **剪枝流程**：仿照 Disco 的分阶段方式，每阶段按「所有出边权重绝对值之和」对列排序，剪掉和最小的固定比例列，然后重训练。

### 2.3 基于 SomeGather 的划分策略

- 沿**特征维**划分，每台设备分配不相交的注意力头子集，激活与权重均按列切分。
- **设备本地 LayerNorm**：因剪枝后单设备未必持有完整行，均值/方差仅由本地列计算。
- 各设备用自己分到的 `W_Q/W_K/W_V` 列本地计算 Q/K/V，注意力头计算**完全本地、零通信**。
- 头输出拼接后经一次 SomeGather 部分交换，再乘本地 `W_O` 列；`W_O` 的列索引与输入 `X_i` 对齐，使残差可本地相加。
- 残差块（式 5）沿用相同模式：每层一次 SomeGather + 本地列乘，输出保持列划分，可直接作为下一层输入。

### 2.4 Message Dropout（MD）

- 训练时模拟 all-to-all 协议下的三类丢包：(a) 某设备收不到另一设备的包；(b) 某设备收不到任何其他设备的包；(c) 其他所有设备都收不到某设备的包。
- 通过采样三个与 `P` 结构类似、但**每个训练步重采样**的权重掩码实现；并保证 Q/K/V 的掩码一致，且 LayerNorm 按设备独立执行。
- 使所有层都暴露于真实丢包模式，从而在推理时对丢包鲁棒。

### 2.5 底层通信：Mixer 协议

- 采用 Mixer：同步传输 + 随机线性网络编码，利用捕获效应与微秒级相位锁定同步，实现低延迟、高可靠的 all-to-all 广播。
- 该抽象屏蔽了 mesh 拓扑细节，避免拓扑变化时反复做任务分配优化（对比 DISNET 等基于图优化的方案）。

## 3. 实验设计

- **硬件 Testbed**：最多 16 台 nRF52840（64 MHz Cortex-M4、1 MB Flash、256 KB RAM），沿 35 m 走廊布置，网络直径 ≥ 2 跳；使用原始 Mixer 实现跑在 BLE 物理层；层计算用 CMSIS-NN 的 8-bit kernel。
- **模型**：时序 Transformer（6 层、特征维 128、残差块 1 个隐藏层）。
- **数据集**：ETT-h2、ICD、London-smart-meters、Traffic（均为时序预测）。
- **训练配置**：ADAM，batch size 2048，学习率 0.001 + cosine schedule，50 epochs，每个配置 **10 个随机种子**重复。
- **指标**：RAM 与 Flash（解析计算）、延迟（硬件实测）、精度（MSE，仿真）。
- **Baseline（按问题分别设定，因无端到端基线）**：
  - Q1 内存：Voltage、Astra、SiracusaDTI（假设集成进 Mixer 框架）。
  - Q2 延迟：单设备集中式执行。
  - Q3 精度：AllGather + 普通剪枝（剪到与 SomeGather 相同带宽）。
  - Q4 鲁棒性：有/无 MD 训练的模型对比。
- **四个研究问题**：Q1 内存扩展性、Q2 延迟扩展性、Q3 剪枝是否保精度、Q4 MD 是否抗丢包。

## 4. 资源与算力

- 论文**未明确报告** GPU 型号、数量或训练时长。
- 仅在致谢中提到使用了 RWTH Aachen 的 **NHR4CES 超算中心**计算时间（项目号 p0021919），但无具体硬件规格与机时。
- 硬件实验侧为 16 台 nRF52840 MCU；仿真侧训练规模（4 数据集 × 10 种子 × 多配置）隐含较大训练开销，但未量化。
- **结论：算力信息不透明，无法据此评估训练成本与可复现的资源门槛。**

## 5. 实验数量与充分性

- **实验组数（概览）**：
  - Q1：基线资源对比（图 4，设备数 5–15）+ 模型规模扩展（图 5，特征维 F × token 数 N × 设备数 2/4/8/16 × 剪枝率 0/30/60/90%）。
  - Q2：注意力块与残差块 × 256/512 特征 × 设备数 1/4/8/12/16 × 剪枝率 0/30/60/90% 的延迟矩阵（图 6）。
  - Q3：4 个数据集上的「精度—通信节省」曲线（图 7）。
  - Q4：4 个数据集 × MD 概率 0/1/5/10% × 消息丢失 0–10%（图 8）。
  - 另有 16 设备端到端执行 14M 参数 ViT 的实机演示（图 1）。
- **充分性评价**：
  - **较充分**：覆盖 4 个数据集、多设备数、多剪枝率、多丢包率的组合，且全部仿真跑 10 个种子；延迟在真实硬件上测量而非纯仿真；同时给出解析资源模型与实测结果。
  - **存在公平性隐患**：baseline 并非原生部署，而是「假设集成进 Mixer 框架」后比较，Voltage/Astra/SiracusaDTI 的真实表现可能被改变；作者也承认不存在端到端基线。
  - **覆盖偏差**：精度评估仅限时序预测任务，14M 参数 ViT 只是规模可行性演示，未报告视觉任务精度；MD 的鲁棒性仅在仿真中验证，未在丢包的真实无线链路上做端到端精度实验。

## 6. 主要结论与发现

- **首次实现**超低功耗无线设备上的分布式 Transformer 推理：16 台 MCU 协同执行 **1400 万参数 ViT**，规模约为单设备的 **14 倍**。
- **SomeGather 效果**：通信量最多降低 **90%**，单设备激活 RAM 降低 **67.5%**，且在 4 个时序预测 benchmark 上精度基本不降；对比普通剪枝，后者随通信压缩精度显著恶化。
- **资源维度**：CATS 是唯一**同时**降低 RAM 与 Flash 的方案；Voltage/Astra 只降 RAM（Flash 不变），SiracusaDTI 的 RAM 与通信随设备数线性增长。
- **延迟**：通信延迟占主导（69.42%–91.4%），且随设备数近似不变；剪枝 90% 可使通信延迟降低最多 80%，相比集中式执行加速最高 **10.96×**（注意力块 512 特征）与 **6.45×**（残差块 512 特征）。
- **MD 效果**：无 MD 时 10% 丢包可使相对预测误差上升最多 **200%**；有 MD 时降至 **23.6%**（ICD），其余数据集为 4.2%（ETT）、8.5%（Traffic）、2.5%（London）；训练与部署丢包率匹配时表现最佳。
- **残余损失**：MD 仍无法完全恢复无损精度，作者归因于最后层不可恢复的丢包。

## 7. 优点与亮点

- **联合设计而非拼接**：划分、通信原语、训练正则三者围绕同一原语协同，剪枝矩阵同时承担通信压缩、RAM 压缩与训练期丢包模拟三种角色，设计高度自洽。
- **SomeGather 的一石多鸟**：在单一机制下同时降低通信量、激活 RAM、Flash 与计算负载，突破了已有方法「降 RAM 不降 Flash / 降 Flash 不降 RAM」的取舍。
- **与 all-to-all 语义对齐的剪枝粒度**：显式指出 Mixer 广播语义下只能按设备整体剪边，并给出 `B = A(W⊙P)` 的严格形式化，而非直接套用点对点剪枝。
- **工程细节扎实**：设备本地 LayerNorm、Q/K/V 列对齐、残差列索引对齐、头顺序计算以省 RAM 等，均针对 MCU 现实约束。
- **MD 的泛化**：把「单切分点 + dropout」的已有思路推广到多层、多设备、多种丢包模式，并处理 Q/K/V 掩码一致性等 Transformer

…特有的结构约束，使 MD 不局限于单一模型结构，可直接迁移到其他采用多头注意力的 Transformer 变体。

## 8. 局限性与可改进之处

- **剪枝粒度受协议语义硬约束**：由于 Mixer 采用「一发全收」广播，剪枝只能按设备整体剪除其全部出边，无法做链路级细粒度剪枝。这意味着当某设备的部分列对下游仍有用、部分列无用時，只能二选一，可能在通信压缩与精度之间留下未被利用的优化空间。作者对此有明确说明，但未量化该约束带来的精度损失上界。
- **精度验证任务面偏窄**：全部精度结论（Q3、Q4）仅建立在 4 个时序预测数据集上，模型为 6 层、特征维 128 的小型 Transformer。14M 参数 ViT 仅作为「规模可行性」演示，未报告任何视觉任务精度，因此「SomeGather 保精度」这一结论在视觉/NLP 任务上的普适性尚未被证实。
- **Baseline 公平性存疑**：Voltage、Astra、SiracusaDTI 并非原生部署，而是「假设集成进 Mixer 框架」后比较，其原生设计假设（如有线链路、AllReduce、专用加速器）被改变后，对比结果可能高估或低估差距。作者亦承认领域内不存在端到端基线，这既是空白也是评估上的软肋。
- **MD 鲁棒性仅停留在仿真**：丢包实验通过仿真注入，未在真实无线链路上做端到端精度验证。真实 BLE/Mixer 的丢包分布具有时间相关性、突发性和距离依赖性，与 MD 训练时假设的独立随机丢包可能不匹配，实际鲁棒性可能低于报告值。
- **残余精度损失未根治**：MD 训练后 10% 丢包下相对误差仍上升 2.5%–23.6%，作者归因于最后一层丢包不可恢复，但未提出针对性机制（如末层重传、冗余编码或输出端纠错）。
- **规模与拓扑覆盖有限**：实验最多 16 台设备、网络直径 ≥ 2 跳，且为静态走廊布局。未考察更大规模、更深多跳、节点移动/失效、拓扑动态变化下的表现；而 Mixer 虽屏蔽拓扑细节，其在稠密 mesh 中的碰撞与捕获效应行为可能随规模变化。
- **能耗维度缺失**：目标平台是电池/能量采集供电的超低功耗设备，但论文未报告能耗、功率或电池寿命指标，也未分析通信与计算在能耗上的权衡。对这类设备而言，能耗往往比延迟更关键。
- **训练开销与超参敏感性不透明**：未报告训练时长、GPU 资源；剪枝率作为核心超参需针对带宽预算人工调优，论文未给出「给定带宽预算如何选剪枝率」的自动化方法或敏感性分析。
- **本地 LayerNorm 的统计偏差**：剪枝后各设备仅用本地列计算均值/方差，与全局 LayerNorm 存在分布差异，论文将其作为工程选择接受，但未分析该近似对精度的影响程度。
- **缺乏与 MCU 加速器方案的对照**：计算侧仅用 CMSIS-NN 8-bit kernel，未与带 NPU/DSP 扩展的新一代 MCU（如带 Ethos-U、CMSIS-NN 优化核）比较，因而「计算受限」这一挑战的缓解空间可能被低估。

## 9. 启示与未来方向

- **「通信原语—模型压缩—训练正则」三位一体**是可复用的设计范式：本文的价值不止于 CATS 本身，更在于示范了如何从一个受限的底层通信抽象（广播式 all-to-all）反向推导出模型侧应有的结构约束（列剪枝 + 设备级掩码），而非把已有的点对点 DTI 方案硬套到无线上。
- **剪枝矩阵的多重语义**（通信压缩 / RAM 压缩 / Flash 压缩 / 丢包模拟）提示：在资源极度受限场景下，应优先寻找能同时服务多个约束的「单一机制」，而不是为每个约束各加一个模块。
- **可直接延伸的研究点**：
  - 突破广播语义的细粒度剪枝（如借助网络编码或子载波/时隙分组实现部分广播）；
  - 面向真实链路统计特性的丢包建模与训练（信道感知 MD、时间相关丢包）；
  - 末层可靠性与端到端纠错机制；
  - 能耗感知的划分与调度，将能耗纳入划分目标函数；
  - 异构设备（不同 Flash/RAM/算力）下的非均匀划分；
  - 动态拓扑与节点失效下的重划分与鲁棒推理；
  - 将方法迁移到视觉、关键词唤醒、语音等 IoT 主流负载并报告精度。

## 10. 总体评价

本文针对「超低功耗无线 MCU 上的分布式 Transformer 推理」这一此前基本空白的场景，提出了结构自洽的 CATS 框架。其核心贡献在于识别出低带宽广播网络的语义约束，并据此设计 SomeGather 原语与配套剪枝，使通信量、激活 RAM、Flash 与计算负载在同一机制下同步下降，同时用 Message Dropout 把无线不可靠性纳入训练。实验在真实 nRF52840 硬件上验证了延迟与资源收益，并在 4 个时序数据集、10 个随机种子上给出了较扎实的精度与鲁棒性证据，16 设备协同运行 14M 参数 ViT 的演示具有标志性意义。

主要不足集中在评估边界：精度结论局限于小型时序 Transformer、baseline 非原生部署、丢包鲁棒性仅在仿真中验证、能耗与训练成本未报告。这些不足不否定方法本身的价值，但意味着「在真实无线链路上、在更广泛任务上是否依然成立」仍需后续工作确认。总体而言，这是一篇问题定义清晰、机制设计巧妙、工程落地扎实的系统性工作，为该方向建立了可参照的基线范式。

（完）
