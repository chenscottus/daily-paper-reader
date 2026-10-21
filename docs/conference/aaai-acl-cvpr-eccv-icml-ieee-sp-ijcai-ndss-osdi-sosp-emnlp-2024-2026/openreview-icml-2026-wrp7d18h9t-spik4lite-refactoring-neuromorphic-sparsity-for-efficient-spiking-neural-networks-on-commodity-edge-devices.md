---
title: "Spik4lite: Refactoring Neuromorphic Sparsity for Efficient Spiking Neural Networks on Commodity Edge Devices"
title_zh: Spik4lite：为通用边缘设备上的高效脉冲神经网络重构神经形态稀疏性
authors: "Yongzhi She, Qihua Zhou, Yuhao Wang, Yaodong Huang, Laizhong Cui, Jingcai Guo"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf/b2695c2d9782d84af09e7d5865b5f20b8a2660c2.pdf"
tags: ["query:coral-npu"]
score: 5.0
evidence: 在通用边缘设备上高效运行脉冲神经网络
tldr: 脉冲神经网络依赖FPGA或神经形态芯片才能高效运行，而通用边缘设备难以获得此类硬件，导致大量无效零脉冲计算浪费能耗。本文提出Spik4lite，通过重构神经形态稀疏性，使脉冲驱动计算在Jetson等通用边缘设备上高效执行。该工作降低了SNN对专用硬件的依赖，提升了边缘部署的能效与可用性，扩展了SNN的实际应用边界。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 脉冲神经网络需专用加速硬件，通用边缘设备上因无效零脉冲计算导致能效下降。
method: 提出Spik4lite，重构神经形态稀疏性以在通用边缘设备上高效执行脉冲驱动计算。
result: 方法使SNN在Jetson等通用边缘设备上高效运行，减少无效计算带来的能耗浪费。
conclusion: 该工作降低SNN对专用硬件依赖，拓展了其在边缘设备上的应用边界。
---

## Abstract
Recently, the spiking neural networks (SNNs) have shown great promise in enhancing AI task performance by utilizing the brain-inspired and energy-efficient computational paradigm via the binary (0/1) spikes. Modern SNNs, especially those based on transformers, often require FPGA accelerators or neuromorphic chips to enable spike-driven computations. However, this domain-specific hardware is not always accessible on commodity edge devices like NVIDIA Jetsons, which may degrade SNNs' energy efficiency due to massive computational waste on inactive "0" spikes and finally undermine the usage boundary. This limitation raises an interesting question: is it possible to make SNNs edge-friendly and tame the computations mostly on active "1" spikes? We present the answer yes and propose Spik4lite, which serves as a lightweight plug-and-play module to significantly improve SNN's performance between model accuracy and computational efficiency. The key is to refactor SNN's channel-wise neuromorphic sparsity by zeroing out low-efficiency channels while proactively compensating for the eliminated spikes. Different from prior methods mainly focusing on optimizing the theoretical synaptic operations, our design philosophy can evolve the SNNs into a physically compact manner, thus inherently saving more computational and energy costs. Extensive experiments based on real edge devices show that Spik4lite can be integrated into existing SNN baselines to further improve their accuracy-and-efficiency performance, guaranteeing the model accuracy while saving the computational and energy costs.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
在通用边缘设备上高效运行脉冲神经网络。

### 2. 核心内容
脉冲神经网络依赖FPGA或神经形态芯片才能高效运行，而通用边缘设备难以获得此类硬件，导致大量无效零脉冲计算浪费能耗。本文提出Spik4lite，通过重构神经形态稀疏性，使脉冲驱动计算在Jetson等通用边缘设备上高效执行。该工作降低了SNN对专用硬件的依赖，提升了边缘部署的能效与可用性，扩展了SNN的实际应用边界。

### 3. 对应检索需求
low power machine learning inference on embedded edge devices。

### 4. 来源与原文
- Source：ICML-2026-Accepted
- OpenReview：[https://openreview.net/forum?id=WRP7d18h9t](https://openreview.net/forum?id=WRP7d18h9t)
