---
title: "NuWa: Deriving Lightweight Class-Specific Vision Transformers for Edge Devices"
title_zh: NuWa：为边缘设备导出轻量级类别专用视觉Transformer
authors: "Wei, Ziteng, He, Qiang, Li, Bing, Chen, Feifei, Jin, Hai, Yang, Yun"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openaccess.thecvf.com/content/CVPR2026/papers/Wei_NuWa_Deriving_Lightweight_Class-Specific_Vision_Transformers_for_Edge_Devices_CVPR_2026_paper.pdf"
tags: ["query:coral-npu"]
score: 5.0
evidence: 为资源受限边缘设备压缩视觉Transformer
tldr: 现有视觉Transformer压缩方法忽视了边缘设备往往只需识别特定类别这一事实，导致压缩模型保留冗余知识、在目标类别上表现欠佳。本文提出NuWa，通过识别并剔除对类别有害的权重、并保持类别相关特征的多样性，直接导出面向特定类别的轻量模型。实验表明该方法在目标类别精度上优于常规压缩方案，为资源受限边缘设备上部署专用视觉模型提供了有效途径。
source: CVPR-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2026-accepted/cvpr-2026-wei-nuwa-deriving-lightweight-class-specific-vision-transformers-for-edge-devices-cvpr-2026-paper/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 1024, \"height\": 1024}, {\"url\": \"assets/figures/cvpr-2026-accepted/cvpr-2026-wei-nuwa-deriving-lightweight-class-specific-vision-transformers-for-edge-devices-cvpr-2026-paper/fig-002.webp\", \"caption\": \"\", \"page\": 2, \"index\": 2, \"width\": 1024, \"height\": 1024}, {\"url\": \"assets/figures/cvpr-2026-accepted/cvpr-2026-wei-nuwa-deriving-lightweight-class-specific-vision-transformers-for-edge-devices-cvpr-2026-paper/fig-003.webp\", \"caption\": \"\", \"page\": 2, \"index\": 3, \"width\": 1024, \"height\": 1024}, {\"url\": \"assets/figures/cvpr-2026-accepted/cvpr-2026-wei-nuwa-deriving-lightweight-class-specific-vision-transformers-for-edge-devices-cvpr-2026-paper/fig-004.webp\", \"caption\": \"\", \"page\": 2, \"index\": 4, \"width\": 1024, \"height\": 1024}, {\"url\": \"assets/figures/cvpr-2026-accepted/cvpr-2026-wei-nuwa-deriving-lightweight-class-specific-vision-transformers-for-edge-devices-cvpr-2026-paper/fig-005.webp\", \"caption\": \"\", \"page\": 2, \"index\": 5, \"width\": 1024, \"height\": 1024}, {\"url\": \"assets/figures/cvpr-2026-accepted/cvpr-2026-wei-nuwa-deriving-lightweight-class-specific-vision-transformers-for-edge-devices-cvpr-2026-paper/fig-006.webp\", \"caption\": \"\", \"page\": 2, \"index\": 6, \"width\": 1024, \"height\": 1024}, {\"url\": \"assets/figures/cvpr-2026-accepted/cvpr-2026-wei-nuwa-deriving-lightweight-class-specific-vision-transformers-for-edge-devices-cvpr-2026-paper/fig-007.webp\", \"caption\": \"\", \"page\": 2, \"index\": 7, \"width\": 1024, \"height\": 1024}, {\"url\": \"assets/figures/cvpr-2026-accepted/cvpr-2026-wei-nuwa-deriving-lightweight-class-specific-vision-transformers-for-edge-devices-cvpr-2026-paper/fig-008.webp\", \"caption\": \"\", \"page\": 2, \"index\": 8, \"width\": 1024, \"height\": 1024}, {\"url\": \"assets/figures/cvpr-2026-accepted/cvpr-2026-wei-nuwa-deriving-lightweight-class-specific-vision-transformers-for-edge-devices-cvpr-2026-paper/fig-009.webp\", \"caption\": \"\", \"page\": 2, \"index\": 9, \"width\": 1024, \"height\": 1024}, {\"url\": \"assets/figures/cvpr-2026-accepted/cvpr-2026-wei-nuwa-deriving-lightweight-class-specific-vision-transformers-for-edge-devices-cvpr-2026-paper/fig-010.webp\", \"caption\": \"\", \"page\": 2, \"index\": 10, \"width\": 1024, \"height\": 1024}, {\"url\": \"assets/figures/cvpr-2026-accepted/cvpr-2026-wei-nuwa-deriving-lightweight-class-specific-vision-transformers-for-edge-devices-cvpr-2026-paper/fig-011.webp\", \"caption\": \"\", \"page\": 2, \"index\": 11, \"width\": 1024, \"height\": 1024}, {\"url\": \"assets/figures/cvpr-2026-accepted/cvpr-2026-wei-nuwa-deriving-lightweight-class-specific-vision-transformers-for-edge-devices-cvpr-2026-paper/fig-012.webp\", \"caption\": \"\", \"page\": 2, \"index\": 12, \"width\": 1024, \"height\": 1024}, {\"url\": \"assets/figures/cvpr-2026-accepted/cvpr-2026-wei-nuwa-deriving-lightweight-class-specific-vision-transformers-for-edge-devices-cvpr-2026-paper/fig-013.webp\", \"caption\": \"\", \"page\": 2, \"index\": 13, \"width\": 1024, \"height\": 1024}, {\"url\": \"assets/figures/cvpr-2026-accepted/cvpr-2026-wei-nuwa-deriving-lightweight-class-specific-vision-transformers-for-edge-devices-cvpr-2026-paper/fig-014.webp\", \"caption\": \"\", \"page\": 4, \"index\": 14, \"width\": 1271, \"height\": 868}, {\"url\": \"assets/figures/cvpr-2026-accepted/cvpr-2026-wei-nuwa-deriving-lightweight-class-specific-vision-transformers-for-edge-devices-cvpr-2026-paper/fig-015.webp\", \"caption\": \"\", \"page\": 4, \"index\": 15, \"width\": 544, \"height\": 642}, {\"url\": \"assets/figures/cvpr-2026-accepted/cvpr-2026-wei-nuwa-deriving-lightweight-class-specific-vision-transformers-for-edge-devices-cvpr-2026-paper/fig-016.webp\", \"caption\": \"\", \"page\": 4, \"index\": 16, \"width\": 1024, \"height\": 1536}, {\"url\": \"assets/figures/cvpr-2026-accepted/cvpr-2026-wei-nuwa-deriving-lightweight-class-specific-vision-transformers-for-edge-devices-cvpr-2026-paper/fig-017.webp\", \"caption\": \"\", \"page\": 4, \"index\": 17, \"width\": 1268, \"height\": 898}, {\"url\": \"assets/figures/cvpr-2026-accepted/cvpr-2026-wei-nuwa-deriving-lightweight-class-specific-vision-transformers-for-edge-devices-cvpr-2026-paper/fig-018.webp\", \"caption\": \"\", \"page\": 4, \"index\": 18, \"width\": 538, \"height\": 660}, {\"url\": \"assets/figures/cvpr-2026-accepted/cvpr-2026-wei-nuwa-deriving-lightweight-class-specific-vision-transformers-for-edge-devices-cvpr-2026-paper/fig-019.webp\", \"caption\": \"\", \"page\": 4, \"index\": 19, \"width\": 1270, \"height\": 898}, {\"url\": \"assets/figures/cvpr-2026-accepted/cvpr-2026-wei-nuwa-deriving-lightweight-class-specific-vision-transformers-for-edge-devices-cvpr-2026-paper/fig-020.webp\", \"caption\": \"\", \"page\": 4, \"index\": 20, \"width\": 597, \"height\": 1053}, {\"url\": \"assets/figures/cvpr-2026-accepted/cvpr-2026-wei-nuwa-deriving-lightweight-class-specific-vision-transformers-for-edge-devices-cvpr-2026-paper/fig-021.webp\", \"caption\": \"\", \"page\": 4, \"index\": 21, \"width\": 2625, \"height\": 928}, {\"url\": \"assets/figures/cvpr-2026-accepted/cvpr-2026-wei-nuwa-deriving-lightweight-class-specific-vision-transformers-for-edge-devices-cvpr-2026-paper/fig-022.webp\", \"caption\": \"\", \"page\": 4, \"index\": 22, \"width\": 2613, \"height\": 952}, {\"url\": \"assets/figures/cvpr-2026-accepted/cvpr-2026-wei-nuwa-deriving-lightweight-class-specific-vision-transformers-for-edge-devices-cvpr-2026-paper/fig-023.webp\", \"caption\": \"\", \"page\": 4, \"index\": 23, \"width\": 1024, \"height\": 1024}, {\"url\": \"assets/figures/cvpr-2026-accepted/cvpr-2026-wei-nuwa-deriving-lightweight-class-specific-vision-transformers-for-edge-devices-cvpr-2026-paper/fig-024.webp\", \"caption\": \"\", \"page\": 4, \"index\": 24, \"width\": 1285, \"height\": 868}]"
motivation: 边缘设备通常只需识别特定类别，而现有压缩方法保留冗余知识导致目标类别性能不佳。
method: 提出NuWa，剔除干扰类别专精的有害权重并保持类别相关数据多样性来导出轻量ViT。
result: 实验显示导出的类别专用轻量模型在目标类别上取得优于常规压缩方法的性能。
conclusion: 该工作为资源受限边缘设备上部署高效专用视觉模型提供了新思路。
---

## Abstract
Vision Transformers (ViTs) often need to be compressed for deployment on resource-constrained edge devices like drones and smart vehicles. However, existing model compression methods ignore that many edge devices only require the knowledge of specific classes for their applications. As a result, the derived all-class ViTs retain redundant knowledge and perform suboptimally on these classes. We discovered that simply replacing the calibration dataset with class-specific data does not suffice to address this issue, as these methods face two fundamental limitations. First, they overlook the existence of class-detrimental weights, which interfere with specialization, while removing them can improve class-specific performance. Second, the diversity of target classes and resource constraints on edge devices demand numerous customized models. Existing methods are time-consuming and computationally expensive, thus unscalable. In this work, we present NuWa, a cost-efficient method that addresses these challenges by deriving small ViTs from base ViTs for edge devices with specific class requirements. NuWa performs self-knowledge purification to prune class-detrimental weights and efficiently derives compact ViTs through closed-form optimization. Without post-pruning retraining, the derived edge ViTs surpass the base ViT in class-specific accuracy and accelerate inference. Comprehensive experiments demonstrate that NuWa outperforms state-of-the-art training-free pruning methods on class-specific tasks by up to 29.00% in accuracy. Compared with the best-performing training-dependent pruning method, NuWa achieves a 33.69x pruning speedup and reduces pruning cost by up to 99.83%, with only a 0.61% average accuracy loss.

---

## 论文详细总结（自动生成）

# NuWa 论文结构化总结

## 1. 核心问题与研究动机

- **背景**：ViT 已广泛用于图像识别、检测、分割等视觉服务，但参数量和计算量巨大，难以直接部署在无人机、智能车等资源受限的边缘设备上，因此需要模型压缩（量化、蒸馏、剪枝等）。
- **关键观察（被已有工作忽视）**：许多边缘应用只关心**特定类别子集**（例如智能车只识别行人、车辆、交通标志，不需要花鸟知识）。而现有结构化剪枝方法都是"类别无关"的，保留大量与目标类别无关的冗余知识，浪费模型容量，导致目标类别精度次优。
- **直接替换校准数据并不够**，作者指出两个根本局限：
  1. **忽视"类别有害权重"（class-detrimental weights）**：实验发现，从 DeiT-Base 的 MLP 中随机删除部分神经元，反而能提升其在 25 个 ImageNet 类上的精度（Fig.1）。但传统的幅值/激活/梯度/Taylor 重要性度量**无法识别**这类权重（Fig.4），因此剪枝模型永远无法超过基模型在目标类上的表现。
  2. **时间与算力成本高、不可扩展**：不同场景目标类不同、不同设备资源约束不同，需要大量定制模型；现有方法需搜索剪枝配置并进行昂贵重训练（单模型 30–90 分钟级，Fig.3），且中间结果不可复用，新类别/新剪枝率需从头再来。
- **整体含义**：论文提出"类别专用 ViT 导出"这一新设定，目标是**从基 ViT 直接、低成本地导出面向指定类别子集的小型 ViT**，且无需剪枝后重训练。

## 2. 方法论：NuWa

**核心思想**：先用"自知识提纯"剔除类别有害权重得到锚模型，再用闭式优化一次性算出剪枝后权重，全程免重训练。

### 2.1 记号与任务设定
- 基模型 \(V_B\)，类别集 \(Y\)；目标子任务类别子集 \(S \subset Y\)，对应类别专用数据 \(D_S\)；目标是导出 \(V_E\)。
- ViT 由 patch embedding、若干 Transformer 块（MHA + MLP）与分类器组成。

### 2.2 步骤一：自知识提纯（SKP）
- **构造剪枝空间**：冻结 \(V_B\)，在每个 MLP 的下采样权重 \(W_2^{(l)}\) 前插入可学习掩码向量 \(M^{(l)}\in\mathbb{R}^{e_l}\) 与控制因子 \(\beta^{(l)}\)。
- **二值化**：以 \(k=\lfloor e_l\cdot\sigma(\beta^{(l)})\rfloor\) 为阈值，取 \(M^{(l)}\) 中第 \(k\) 大的元素作为门限，得到 \(M^{(l)}_{bin}\)（式 3）；掩码参与前向（式 4），置 0 即等价于剪掉 \(W_1\) 的第 \(i\) 行与 \(W_2\) 的第 \(i\) 列。
- **函数保持初始化**：\(\beta^{(l)}\) 初始化为 5.0（\(\sigma(5.0)\approx1\)），保证插入掩码前后模型输出一致。
- **训练**：使用直通估计器（STE）解决二值化不可导，\(M^{(l)}_{bin}\) 的梯度 \(G^{(l)}\) 作为 \(M^{(l)}\) 的替代梯度，内积 \(\langle M^{(l)}_{bin},G^{(l)}\rangle\) 作为 \(\beta^{(l)}\) 的替代梯度；**不加额外正则项**，仅用原始任务损失 \(L_T\) 与 \(D_S\) 监督，让模型自主发现有害权重。
- **产出**：按掩码索引裁剪得到锚模型 \(V_A\)（式 5、6）。SKP 仅更新不超过总参数 0.05% 的参数量，且**只作用于 MLP**（经验上比同时作用于 MHA 更好）。
- 效果：相比随机剪枝，锚模型能达到更高剪枝率与更大精度提升（平均 22.61% 剪枝率、+15.84% 目标类精度）。

### 2.3 步骤二：基于优化的快速剪枝（OFP）
- **剪枝 MHA（QK 与 VO 维度）**：
  - 动机：细粒度维度剪枝比整头剪枝压缩潜力更大；QK/VO 是两次权重乘法的中间维度、与输入 \(X\) 无关，因此**剪枝结果可跨子任务共享**。
  - 建模为低秩逼近：\(\min \|W_Q^{\top}W_K - W_Q'^{\top}W_K'\|_F^2\)，由 Eckart–Young 定理用 **SVD** 得到闭式解（式 8）。
  - 用超参 \(\rho\)（保留奇异值能量比）按式 9 自适应决定每层剪后的 \(q_l'\)、\(v_l'\)，从而平衡 MHA 与 MLP 的剪枝强度。
- **剪枝 MLP（中间维度）**：
  - 为到达总剪枝率 \(\alpha\)，并尽量让各块 MLP 尺寸均衡（式 10：\(e_l'=\min(e_l,\lfloor(\sum e_i-e_{prune})/L\rfloor)\)）。
  - 在 \(D_S\) 上前向得到各神经元平均激活 \(a_i^{(l)}\)，并采样 \(K=128\) 张图提取块级特征 \(H^{(l)}\)；保留激活最高的 \(e_l'\) 个神经元（索引 \(I_r^{(l)}\)）。
  - 求解 \(\min\|H^{(l)}W_2^{(l)\top}-H^{(l)}[I_r]W_2'^{\top}\|_F^2\)，得到**最小二乘闭式解**（式 12）：\(W_2'=W_2H^{\top}H_r(H_r^{\top}H_r)^{\dagger}\)，\(W_1'=W_1[I_r]\)。
- **附加技巧**：在 \(V_A\) 分类器中给非目标类加大负偏置（如 −100），从根本上抑制误判到 \(S^c\)。
- **整体流程**：\(V_B \xrightarrow{\text{SKP}} V_A \xrightarrow{\text{SVD 剪 MHA} + \text{闭式解剪 MLP}} V_E\)，无重训练。

## 3. 实验设计

- **模型与数据集**：
  - DeiT-Base / DeiT-Small / DeiT-Tiny、ViT-L/16，在 ImageNet-1K、CIFAR-100、CIFAR-10 上做分类；
  - Fast R-CNN / Mask R-CNN（Swin-T 骨干）在 COCO2017 上做检测与实例分割。
- **子任务设定**：从数据集类别中随机抽取 \(N\) 类构成 \(S_i/N\)，ImageNet 上用 \(N=10,25,50\) 共 9 个子任务（S1–S9）；CIFAR-10 用 \(|S|=2\)、CIFAR-100 用 \(|S|=10\)、COCO 用 \(|S|=1\)。
- **剪枝率**：主表用 0.40 与 0.60，另做 0–0.60 的曲线扫描；以 GFLOPs 反映设备资源约束并据此计算剪枝率。
- **Benchmark 与对比方法**：
  - 免训练结构化剪枝：Magnitude Pruning、Wanda-sp、Numerical Pruning、Random（下界）；
  - 依赖训练的结构化剪枝（SOTA）：X-Pruner、DC-ViT、RECAP、MDP；
  - 参照：DeiT-Base、DeiT-Base(FT)、Anchor Model、NuWa(FT)（10 epoch 轻微微调）。
- **效率评测**：Jetson Orin NX（batch=1，模拟实时顺序请求）与 RTX 4090（batch=256，模拟服务端批处理）上的时延/吞吐/显存；成本按 AWS EC2 g5.48xlarge 计价；设定 \(N\) 个子任务 × \(M\) 台异构设备的 \(MN\) 模型部署场景。

## 4. 资源与算力

- 文中明确说明**所有实验在一张 NVIDIA RTX 4090 GPU 上完成**，未提及多卡或集群规模。
- SKP 训练配置：mask 学习率 0.001、控制因子学习率 0.1、总步数 \(10^4\)、batch size = 1、AdamW 优化器；OFP 校准样本数 \(K=128\)。
- 导出开销：单模型总导出约 **0.07–0.09 GPU 小时**（约 5 分钟级），其中 SVD（P1）仅 0.01 GPU 小时，SKP+前向（P2）约 0.06–0.08 GPU 小时，MLP 闭式解（P3）约 \(2.5\times10^{-4}\) GPU 小时。
- 未报告总训练/实验累计 GPU 时长，也未说明基线方法重训练的具体 epoch 总量（仅称 RECAP 用 10 epoch 微调作对照）。

## 5. 实验数量与充分性

- **规模**：主实验覆盖 9 个 ImageNet 子任务 × 2 个剪枝率，加 4 类其他模型/数据集配置（DeiT-T/S、ViT-L 于 CIFAR-10/100，Swin-T 于 COCO 检测与分割）、2 张硬件平台的速度/显存评测、1 组大规模部署成本模拟（\(N=50,M=10\)）。
- **消融**：5 组（去 SKP、去 MHA 剪枝、去 MLP 剪枝、随机选神经元代替激活排序、用直接删行代替闭式解），另有
