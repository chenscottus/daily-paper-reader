---
title: Faster Activation Functions at the Edge for Post-Training Speedups
title_zh: 边缘端更快的激活函数：面向训练后加速
authors: "Anton Lydike, Jun Bi, Jackson Woodruff"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf/c6e7359c8ec05ad034329553bc37bb6b26299da4.pdf"
tags: ["query:coral-npu"]
score: 6.0
evidence: 为边缘推理生成快速激活函数的编译器
tldr: 边缘设备资源紧张，Swish、GELU等高级激活函数因缺少指数与除法硬件快路径而推理开销高，迫使端侧只能用ReLU类简单函数、牺牲精度。作者提出FFCC编译器，通过浮点重解释自动生成高效近似激活函数，无需硬件快路径即可在端侧保持快速。该方法以编译手段直接支撑边缘低延迟推理，与边缘加速器工具链需求高度相关。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 高级激活函数在边缘设备缺少硬件快路径导致开销高，限制模型精度。
method: 提出FFCC编译器，通过浮点重解释自动生成高效的激活函数近似。
result: 无需硬件快路径即可在边缘设备上获得快速且准确的激活函数。
conclusion: 以编译优化方式提升边缘推理速度并释放高精度模型潜力。
---

## Abstract
On-device AI has gained significant attention for enabling efficient, low-latency inference on edge devices.
However, tight resource constraints on these platforms make the deployment of accurate and lightweight deep
learning models challenging.
In particular,
advanced activation functions (AFs) like Swish and GELU often incur
high inference overhead due to the lack of hardware fast-paths for exponentiation and division,
restricting edge-ML applications to simple AFs like ReLU, limiting model accuracy.
To address this, we propose FFCC, a compiler that automatically
generates efficient approximations of AFs through floating-point reinterpretation.
These functions do not require hardware fast-paths, meaning they remain fast
on edge devices, but are accurate enough to be used as post-training drop-ins.
FFCC takes a specification of AFs using basic floating-point operators
and applies derivation rules to lower these expressions into
efficient instruction sequences.
Our experiments show that FFCC provides fast approximations of AFs, achieving order-of-magnitude
speed-ups over accurate baselines on Arm M7, Aarch64 and Intel platforms.
Using ConvNeXt as an example, we demonstrate how these activation-level gains translate to end-to-end speed-ups,
and do not result in significant loss of model accuracy.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
为边缘推理生成快速激活函数的编译器。

### 2. 核心内容
边缘设备资源紧张，Swish、GELU等高级激活函数因缺少指数与除法硬件快路径而推理开销高，迫使端侧只能用ReLU类简单函数、牺牲精度。作者提出FFCC编译器，通过浮点重解释自动生成高效近似激活函数，无需硬件快路径即可在端侧保持快速。该方法以编译手段直接支撑边缘低延迟推理，与边缘加速器工具链需求高度相关。

### 3. 对应检索需求
compiler and toolchain support for deploying models on Coral accelerators。

### 4. 来源与原文
- Source：ICML-2026-Accepted
- OpenReview：[https://openreview.net/forum?id=u2o6hPE8ik](https://openreview.net/forum?id=u2o6hPE8ik)
