---
title: "UQ-ViT: Harmonizing Extreme Activations with Hardware-Friendly Uniform Quantization in Vision Transformers"
title_zh: UQ-ViT：以硬件友好均匀量化协调视觉Transformer中的极端激活
authors: "Tao Jiang, Yucheng Jiang, Xiwen Yao, Gong Cheng, Junwei Han"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39393/43354"
tags: ["query:litert-ops"]
score: 4.0
evidence: 面向加速器推理的硬件友好均匀量化
tldr: 视觉Transformer中后Softmax注意力分数呈幂律分布、后GELU激活通道间方差大，导致均匀量化难以兼顾高幅值精度与整体误差。UQ-ViT提出硬件友好的均匀量化方案，通过激活整形与校准缓解极端非均匀性。实验表明其在保持精度的同时可充分利用加速器矩阵核心实现高效推理。该工作为量化算子内核在硬件上的高效执行提供了可迁移思路。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39393/fig-001.webp\", \"caption\": \"\", \"page\": 1, \"index\": 1, \"width\": 751, \"height\": 597}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39393/fig-002.webp\", \"caption\": \"\", \"page\": 1, \"index\": 2, \"width\": 750, \"height\": 574}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39393/fig-003.webp\", \"caption\": \"\", \"page\": 2, \"index\": 3, \"width\": 612, \"height\": 247}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39393/fig-004.webp\", \"caption\": \"\", \"page\": 2, \"index\": 4, \"width\": 630, \"height\": 250}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39393/fig-005.webp\", \"caption\": \"\", \"page\": 2, \"index\": 5, \"width\": 631, \"height\": 231}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39393/fig-006.webp\", \"caption\": \"\", \"page\": 2, \"index\": 6, \"width\": 629, \"height\": 242}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39393/fig-007.webp\", \"caption\": \"\", \"page\": 5, \"index\": 7, \"width\": 895, \"height\": 618}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39393/fig-008.webp\", \"caption\": \"\", \"page\": 5, \"index\": 8, \"width\": 1177, \"height\": 784}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39393/fig-009.webp\", \"caption\": \"\", \"page\": 5, \"index\": 9, \"width\": 948, \"height\": 622}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39393/fig-010.webp\", \"caption\": \"\", \"page\": 5, \"index\": 10, \"width\": 1174, \"height\": 781}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39393/fig-011.webp\", \"caption\": \"\", \"page\": 5, \"index\": 11, \"width\": 1162, \"height\": 778}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39393/fig-012.webp\", \"caption\": \"\", \"page\": 5, \"index\": 12, \"width\": 905, \"height\": 598}]"
motivation: 视觉Transformer激活分布极端非均匀，使均匀量化在保精度与适配加速器之间陷入两难。
method: 提出UQ-ViT，用校准与激活整形将极端激活映射到硬件友好的均匀量化范围。
result: 在视觉Transformer量化基准上兼顾精度，同时支持加速器矩阵核心高效推理。
conclusion: 为量化算子内核在硬件加速器上的高效部署提供了硬件友好的量化方案。
---

## Abstract
Post-Training Quantization enables efficient Vision Transformer (ViTs) deployment with a small calibration data, and its prevalent use of uniform quantization harnesses AI accelerator matrix cores for high-speed inference. However, the application of uniform quantization is fundamentally challenged by the extreme non-uniformity of activation distributions.Specifically, the power-law nature of post-Softmax attention scores and the significant inter-channel variance in post-GELU activations create a dilemma for conventional quantization, as it struggles to preserve critical high-magnitude values without sacrificing overall precision. To resolve this core conflict, we introduce UQ-ViT (Uniform Quantization for Vision Transformers), a novel uniform quantization framework designed to reconcile high precision with hardware efficiency. Central to UQ-ViT are two operators: Dynamic Elimination of Maximum (DeMax) and Normalization Quantization (NormQuant). DeMax is a quantization operator for post-Softmax attention scores that utilizes uniform quantization. It dynamically eliminates and preserves dominant values, effectively mitigating quantization loss from the extreme values in the power-law distribution. NormQuant utilizes a per-channel quantization strategy during quantization and reverts to a per-tensor format for dequantization, achieving both high accuracy and computational efficiency. Crucially, it is applicable to any linear layer, enabling effective quantization of post-GELU activations in ViTs. Through extensive experiments on various ViTs and vision tasks, including image classification, object detection, and instance segmentation, we demonstrate that our proposed approach outperforms existing methods, achieving superior accuracy while ensuring hardware friendliness.

---

## 论文详细总结（自动生成）

## 一、核心问题与整体含义

- **背景**：Vision Transformer（ViT）在分类、检测、分割等任务上表现优异，但计算与存储开销大，难以部署到资源受限设备。Post-Training Quantization（PTQ）只需少量校准数据即可完成低比特量化，部署效率高。
- **硬件约束**：均匀量化（uniform quantization）能直接复用 AI 加速器的整数矩阵乘核心，是实际部署中最通用的方案；而非均匀量化（如 log√2、AdaLog）虽然精度更好，却因 Hadamard 积与矩阵乘法不可交换，移位操作无法预执行，导致实现复杂、硬件效率低。
- **核心矛盾**：ViT 中存在两类“量化不友好”的极端激活分布——
  - **post-Softmax 注意力分数**：呈幂律分布，每行通常有一个远大于其余值的支配值（如 [0.8, 0.1, 0.05, 0.05]）。均匀量化下大值压缩小值造成严重信息损失，而直接裁剪又会破坏关键语义 token。
  - **post-GELU 激活**：通道间动态范围差异显著，per-tensor 统一缩放因子无法刻画通道差异；已有的重参数化方法（如 RepQuant）依赖前置 LayerNorm，无法推广到 post-GELU 位置。
- **整体含义**：论文试图在“保持均匀量化的硬件友好性”这一硬约束下，尽量逼近非均匀量化的精度，从而调和精度与硬件效率之间的根本冲突。

## 二、方法论

### 核心思想
以 RepQ-ViT 为基础、把其 log√2 非均匀量化替换为均匀量化作为 baseline，再针对两个极端激活位置分别设计专用算子：**DeMax**（post-Softmax）与 **NormQuant**（pre-Linear / post-GELU），二者均为均匀量化算子。

### DeMax：动态剔除最大值
- **观察**：Softmax 输出每行仅有一个主导值，其余值很小，极端值主导了量化区间。
- **流程**：
  1. 对输入矩阵 A（m×n）逐行提取最大值 v_i 及其列索引 idx_i，得到值向量 v 与索引向量 idx；
  2. 构造稀疏矩阵 A_max（每行仅保留最大值，其余置零），并将 A 中对应位置置零得到 Ã；
  3. 将 O = AV 分解为两部分：Õ = ÃV 用整数矩阵乘（加速器核心）计算；O_max = A_max·V 因每行只有一个非零元，可化简为 O_max,i = v_i · V_{idx_i,:}，即按索引取 V 的行再加权；
  4. 反量化阶段用稀疏矩阵乘法恢复被剔除的主导值。
- **开销分析**：注意力矩阵乘复杂度为 O(N²C)，DeMax 仅增加 O(NC)；且行最大值搜索与索引记录可复用 Softmax 计算中为防数值溢出而做的行最大值遍历，**无额外统计开销**。

### NormQuant：归一化量化
- **核心思想**：量化时采用 per-channel 缩放/零点以匹配各通道分布，反量化时退回 per-tensor 标量格式，从而兼顾精度与整数矩阵运算兼容性。
- **公式流程**（文字描述）：
  - 量化：x(Z) = clip(⌊x/s′ + z′⌉, 0, 2^b−1)，其中 s′、z′ 为逐通道缩放与零点，可视为归一化的缩放/平移系数；
  - 反量化：x̂ = s·x(Z)，s 为 per-tensor 标量。
- **重参数化**：将 s′、z′ 吸收进权重与偏置——W′ = s′·W/s，b′ = b − s′z′·W，使推理阶段**零额外计算开销**。
- **通用性**：不依赖 LayerNorm，可插入任意线性层或卷积层之前，因而适用于 post-GELU 激活。

### 实现细节
- 权重使用 channel-wise 量化；pre-Linear 激活用 NormQuant；所有 post-Softmax 激活用 DeMax。
- NormQuant 的缩放因子参照 SmoothQuant 策略，通过简单搜索确定。

## 三、实验设计

- **数据集与场景**：
  - **ImageNet** 图像分类：ViT-S/B、DeiT-T/S/B、Swin-S/B 共 7 个骨干。
  - **COCO** 目标检测与实例分割：Mask R-CNN（Swin-T/S）与 Cascade Mask R-CNN（Swin-T/S）。
- **量化配置**：W4/A4 与 W6/A6 两档（权重/激活比特）。
- **Benchmark 与对比方法**：全精度作为上界；对比 PTQ4ViT、APQ-ViT、RepQ-ViT、ERQ，并给出自建均匀量化 baseline；表中额外标注各方法是否硬件友好（HF 列）。
- **评价指标**：ImageNet Top-1 准确率；COCO 的 AP^box 与 AP^mask。
- **校准数据**：ImageNet 随机 512 张，COCO 随机 2 张。

## 四、资源与算力

- 文中明确提到：全部实验在 **单张 NVIDIA 2080Ti GPU** 上使用 PyTorch 完成。
- **未提及**：具体训练/校准耗时、总 GPU 小时数、量化推理的实际延迟或吞吐测量。
- 作为 PTQ 方法本身无需重训练，但论文也没有报告校准阶段的时间成本与端到端加速比，硬件效率的论证主要停留在“算子结构兼容矩阵核心”的定性/复杂度层面。

## 五、实验数量与充分性

- **实验规模**：
  - 表 1：ImageNet，7 个模型 × 2 种比特配置 × 约 6 个对比项；
  - 表 2：COCO，4 个模型设置 × 2 种比特配置 × 5 个对比项；
  - 表 3：DeMax / NormQuant 组合消融，3 个模型 × 4 种配置；
  - 表 4：NormQuant 层配置消融，3 个模型 × 6 种层组合；
  - 另有图 5 的激活分布可视化对比。
- **充分性**：覆盖分类、检测、分割三类任务与 ViT/DeiT/Swin 三类架构，消融同时覆盖“算子级”和“层级”两个粒度，整体较为完整。
- **客观与公平性**：
  - 优点是对比表中显式区分硬件友好性，避免“以非均匀方法精度压均匀方法”的不公平比较；对 RepQ-ViT 在 COCO Swin-S 上的结果标注了 *（用官方代码复现）。
  - 局限：baseline 为作者自行将 RepQViT 替换为均匀量化的版本，其调优程度可能影响增益幅度；未与 QAT 方法对比；未报告多次运行的方差或置信区间。

## 六、主要结论与发现

- **W4/A4（最具挑战设置）**：UQ-ViT 平均较 baseline 提升约 **3.2 个百分点**，ViT-B 提升 4.46%、Swin-B 提升 5.61%；DeiT-S 达 72.13%（baseline 68.98%）。在完全均匀量化的前提下，精度已接近当前最优的非均匀 PTQ 方法。
- **W6/A6**：与现有非均匀方法性能相当，多数模型略优于 baseline（如 ViT-S 80.71%）。
- **COCO**：4-bit 下 Mask R-CNN + Swin-T 达 **39.0 AP^box / 37.3 AP^mask**，明显优于 RepQ-ViT（36.1/36.0）与 ERQ（36.8/36.6）；6-bit 下与 SOTA 基本持平。
- **消融**：NormQuant 单独使用即带来明显增益（DeiT-T 58.65、DeiT-S 71.45），叠加 DeMax 后进一步提升（59.71、72.13）。层级消融显示增益主要来自 **fc1（+6.99%）与 qkv（+5.61%）**，fc2 为 +1.92%，proj 仅 +0.30%，与 ViT 激活分布不均衡的观察一致。
- **总体结论**：在保持均匀量化硬件友好性的同时，可取得与非均匀量化相当的精度，为 ViT 低比特部署提供了可落地的折中方案。

## 七、优点

- **问题定位精准**：把 ViT 量化难点拆解为 post-Softmax 与 post-GELU 两个具体位置，并分别给出针对性算子，而非笼统的“激活不均衡”处理。
- **DeMax 设计巧妙**：利用 Softmax 每行“单峰”这一结构先验，把极端值从量化区间中“摘出”再稀疏恢复，既避免裁剪损失又保持整数矩阵乘；且复用 Softmax 已有的行最大值遍历，几乎零额外统计成本，复杂度增量 O(NC) 相对 O(N²C) 可忽略。
- **NormQuant 兼顾精度与效率**：per-channel 量化 + per-tensor 反量化 + 权重/偏置重参数化，推理零额外开销；不依赖 LayerNorm，适用范围比 RepQuant 更广（可用于任意线性/卷积层）。
- **硬件意识贯穿全文**：明确以“能否复用矩阵计算核心”作为方法取舍标准，并在对比表中标注硬件友好性，避免了纯精度竞赛的误导。
- **实验覆盖较广**：三大任务、多类骨干、两种比特宽度，加上算子级与层级消融，并附激活分布可视化佐证机制；代码已开源。

## 八、不足与局限

- **缺乏真实硬件验证**：仅在 2080Ti 上做精度实验，未测实际延迟、吞吐或能耗；DeMax 依赖的“每行单非零稀疏矩阵乘”在真实加速器上的支持度与效率未做验证，端到端加速比缺失，硬件友好性主要是结构性论证。
- **精度仍未完全追平非均匀方法**：W4/A4 下 ViT-S（68.34 vs ERQ 68.91）、Swin-B（81.96 vs 82.44）、COCO 部分设置（Swin-S 43.3 vs 43.4，Cascade 49.6 vs 50.0）略低于 ERQ。
- **对比维度有限**：未与 QAT 方法比较；未涉及更大模型（ViT-L、Swin-L）或更激进比特（3-bit/2-bit）；未讨论 LLM 或多模态 Transformer 的迁移性。
- **超参与校准敏感性缺失**：校准集固定为 512 张（COCO 仅 2 张），未做校准数据量/分布的敏感性分析；NormQuant 的缩放因子仅用“简单搜索”，未说明搜索空间与最优性，也未与 SmoothQuant 的迁移强度超参做消融。
- **潜在偏差风险**：baseline 为作者改造的均匀量化版 RepQViT，其调优程度直接影响增益幅度；部分对比数值为复现结果，跨方法比较的公平性存在一定不确定性。
- **应用限制**：DeMax 的有效性依赖 Softmax 每行单一主导值这一先验，对线性注意力、稀疏注意力或分布更平坦的注意力（如某些检测/分割头）可能收益下降；NormQuant 的 per-channel 零点处理细节（对称/非对称）在文中未充分展开。

（完）
