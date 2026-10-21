---
title: Diversity-aware Weight Perturbation Promotes Robust Adaptation
title_zh: 多样性感知权重扰动促进鲁棒自适应
authors: "Zibo Chen, Ruxin Li, Zilu Wang"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf/2de006a9cdd8145cc96ea782be2554c11d0744d0.pdf"
tags: ["query:coral-npu"]
score: 4.0
evidence: 面向存内计算加速器的低功耗边缘推理
tldr: "存内计算加速器被视为低功耗边缘推理的重要途径，但器件噪声与漂移带来的权重扰动会显著削弱DNN推理精度。作者提出免疫系统启发的多样性感知权重扰动训练方法，利用多种噪声实现下样本级预测分歧自适应地为样本加权。实验表明，该方法训练的模型鲁棒性明显提升，相比标准训练精度提高超过15%。该工作为在存内计算等边缘加速硬件上可靠部署深度模型提供了训练侧的鲁棒性方案。"
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 存内计算加速器用于低功耗边缘推理时，器件噪声与漂移造成的权重扰动会降低DNN精度并阻碍可靠部署。
method: 提出多样性感知权重扰动训练方法，借鉴免疫系统亲和选择机制，依据多样噪声下的样本级预测分歧进行自适应样本加权。
result: "实验显示该方法训练的模型鲁棒性持续优于标准训练，精度提升超过15%。"
conclusion: 为存内计算等边缘推理硬件的可靠模型部署提供了有效的鲁棒训练范式。
---

## Abstract
Compute-In-Memory (CIM) accelerators are promising for energy-efficient edge inference, yet they faces fundamental challenges when deploying Deep Neural Networks (DNNs), as hardware-induced weight perturbations from intrinsic noise and device drift degrade accuracy and impede reliable inference. To tackle this challenge, we propose Diversity-aware Weight 
Perturbation (DWP), an immune-system-inspired training method that 
emulates affinity-based selection by exploiting sample-level 
prediction disagreement under diverse noise realizations to guide 
adaptive sample weighting, building robustness to weight perturbation. Experiments show that DWP-trained models consistently yield superior robustness, achieving over 15\% accuracy improvements compared to standard-trained models under severe weight perturbations (mismatch level up to 70\%) and maintaining 
inference accuracy at 90\% over a simulated one-year CIM operation 
with only 2\%–4\% variation in accuracy. Moreover, under matched model and inference configurations, deployment on low-precision CIM hardware reduces inference energy by 38\% compared to a GPU baseline. These results demonstrate that DWP enables robust and energy-efficient neural network deployment on resource-constrained edge devices with inherent hardware uncertainties.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
面向存内计算加速器的低功耗边缘推理。

### 2. 核心内容
存内计算加速器被视为低功耗边缘推理的重要途径，但器件噪声与漂移带来的权重扰动会显著削弱DNN推理精度。作者提出免疫系统启发的多样性感知权重扰动训练方法，利用多种噪声实现下样本级预测分歧自适应地为样本加权。实验表明，该方法训练的模型鲁棒性明显提升，相比标准训练精度提高超过15%。该工作为在存内计算等边缘加速硬件上可靠部署深度模型提供了训练侧的鲁棒性方案。

### 3. 对应检索需求
low power machine learning inference on embedded edge devices。

### 4. 来源与原文
- Source：ICML-2026-Accepted
- OpenReview：[https://openreview.net/forum?id=5VoK9nUVSD](https://openreview.net/forum?id=5VoK9nUVSD)
