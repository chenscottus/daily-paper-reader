---
title: "No Retraining at Edge: Efficient Resource-Aware Mixed-Precision Quantization via Federated Supernet Learning"
title_zh: 边缘免重训练：基于联邦超网学习的资源感知混合精度量化
authors: "Lianbo Ma, Yonghui Su, Nan Li, Xingwei Wang"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf/e7fd4baac965eeb8d48dcf88c6b552de23ab5c20.pdf"
tags: ["query:coral-npu"]
score: 5.0
evidence: 面向边缘部署的免重训练混合精度量化
tldr: 联邦学习可在分布式边缘设备上协同训练，但设备资源动态变化时现有方法需反复重训练，开销巨大。作者提出DFMPQ动态联邦混合精度量化框架，通过联邦学习训练权重共享的混合精度超网，训练后可按需导出满足资源约束的量化子网。该方法无需重训练即可适配异构且时变的边缘资源，对端侧量化模型部署具有参考价值。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 边缘资源动态变化时，现有量化方法需反复重训练，计算开销过大。
method: 通过联邦学习训练权重共享的混合精度超网，按需导出资源感知量化子网。
result: 无需额外重训练即可满足异构且时变的边缘资源约束。
conclusion: 为动态边缘环境下的轻量模型部署提供了免重训练的量化方案。
---

## Abstract
Federated learning (FL) enables collaborative training across distributed edge devices, but deploying lightweight models in dynamic edge environments remains challenging. Existing methods typically require retraining whenever device resource constraints change, resulting in excessive computational overhead. We propose DFMPQ, a dynamic federated mixed-precision quantization framework that enables retraining-free deployment at the edge. DFMPQ trains a weight-sharing mixed-precision supernet via FL, which jointly represents diverse bit-width configurations. After training, resource-aware quantized subnets can be derived on demand to satisfy heterogeneous and time-varying resource constraints without additional optimization. Optimizing such a supernet in federated settings is difficult due to optimization interference among heterogeneous bit-widths and the coupling of quantization noise with non-IID data. DFMPQ addresses these issues through semantic-aware training and aggregation mechanisms that stabilize supernet optimization. In addition, a sensitivity-guided greedy search strategy is adopted to efficiently identify suitable quantization configurations under given resource budgets. Extensive experiments on multiple datasets and network architectures demonstrate that DFMPQ achieves competitive accuracy with significantly reduced computational cost, enabling efficient deployment for dynamic edge computing environments.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
面向边缘部署的免重训练混合精度量化。

### 2. 核心内容
联邦学习可在分布式边缘设备上协同训练，但设备资源动态变化时现有方法需反复重训练，开销巨大。作者提出DFMPQ动态联邦混合精度量化框架，通过联邦学习训练权重共享的混合精度超网，训练后可按需导出满足资源约束的量化子网。该方法无需重训练即可适配异构且时变的边缘资源，对端侧量化模型部署具有参考价值。

### 3. 对应检索需求
low power machine learning inference on embedded edge devices。

### 4. 来源与原文
- Source：ICML-2026-Accepted
- OpenReview：[https://openreview.net/forum?id=nHbvQF35ch](https://openreview.net/forum?id=nHbvQF35ch)
