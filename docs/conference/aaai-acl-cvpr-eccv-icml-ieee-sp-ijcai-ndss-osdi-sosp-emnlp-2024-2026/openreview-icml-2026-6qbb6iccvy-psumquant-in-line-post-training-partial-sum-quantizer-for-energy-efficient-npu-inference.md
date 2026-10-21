---
title: "PsumQuant: In-line Post-training Partial Sum Quantizer for Energy Efficient NPU Inference"
title_zh: PsumQuant：面向高能效NPU推理的在线后训练部分和量化器
authors: "Sangwoo Hwang, Yeeun Hong, Jaeha Kung"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf/1f8197be0a88018a5d9092450db9ded9f256a7cd.pdf"
tags: ["query:coral-npu"]
score: 6.0
evidence: 面向NPU推理的能效量化
tldr: 深度神经网络加速中，累加产生的部分和往往保留高精度，带来显著能耗开销。本文分析分块架构中部分和的分布，发现块内离群值依赖输入，进而提出PsumQuant，一种后训练、输入感知的量化方法，利用激活的峰值因子在线预测部分和尺度以约束位宽。实验表明该方法在抑制离群值的同时降低了NPU推理能耗，为高能效硬件加速提供了实用方案。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 累加过程中部分和保留高精度导致显著能耗开销，成为NPU高效推理的瓶颈。
method: 提出PsumQuant，利用输入激活的峰值因子在线预测部分和尺度，动态约束位宽。
result: 实验表明该方法能有效处理离群值并降低NPU推理的累加能耗。
conclusion: 该工作为量化下的高能效NPU推理提供了可落地的后训练量化方案。
---

## Abstract
The rapid growth of deep neural networks (DNNs) has intensified the demand for efficient hardware acceleration under quantization. 
While prior research has successfully reduced weight and activation precision, partial sums generated during accumulation often retain high precision, resulting in significant energy overhead.
In this work, we analyze psum distributions in tiled architectures and reveal that within-tile outliers are input-dependent.
We propose PsumQuant, a post-training, input-aware quantization that predicts psum scales on-the-fly.
By leveraging the crest factor of input activations, our learnable scale predictor effectively bounds the psum bit-width while handling the extreme outliers in DNNs.
Experimental results on a $128 \times 128$ systolic array demonstrate that PsumQuant compresses psum precision down to 8-bit within only a 1\% accuracy drop on ResNet-18 and a marginal 0.04 perplexity increase on Llama-3.1. 
Furthermore, bit-width reduction with PsumQuant results in a 45\% reduction in total energy with minimal accuracy loss, demonstrating that PsumQuant provides a highly efficient solution for actual NPU architectures.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
面向NPU推理的能效量化。

### 2. 核心内容
深度神经网络加速中，累加产生的部分和往往保留高精度，带来显著能耗开销。本文分析分块架构中部分和的分布，发现块内离群值依赖输入，进而提出PsumQuant，一种后训练、输入感知的量化方法，利用激活的峰值因子在线预测部分和尺度以约束位宽。实验表明该方法在抑制离群值的同时降低了NPU推理能耗，为高能效硬件加速提供了实用方案。

### 3. 对应检索需求
low power machine learning inference on embedded edge devices。

### 4. 来源与原文
- Source：ICML-2026-Accepted
- OpenReview：[https://openreview.net/forum?id=6Qbb6IcCVy](https://openreview.net/forum?id=6Qbb6IcCVy)
