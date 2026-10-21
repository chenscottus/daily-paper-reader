---
title: Characterizing Mobile SoC for Accelerating Heterogeneous LLM Inference
title_zh: 面向异构大模型推理加速的移动SoC特性刻画
authors: "Le Chen, Dahu Feng, Erhu Feng, Yingrui Wang, Rong Zhao, Yubin Xia, Pinjie Xu, Haibo Chen"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://dl.acm.org/doi/pdf/10.1145/3731569.3764808"
tags: ["query:coral-npu"]
score: 6.0
evidence: 刻画移动SoC中NPU与GPU以加速设备端低延迟大模型推理
tldr: 移动系统开始本地集成AI能力以提升隐私并降低响应延迟，当前SoC配备GPU与神经处理单元等多样加速器，但缺乏对异构处理器的系统刻画，且现有设计通常只用单一加速器做推理，导致算力与内存带宽利用不足。论文系统总结了异构处理器的关键性能特征，并据此提出协同利用多种加速器的推理方案。该工作为边缘NPU等加速器上的低延迟设备端推理提供了实测依据与优化方向。
source: SOSP-2025-ACM
selection_source: conference_retrieval
motivation: 移动SoC虽集成NPU与GPU等加速器，但缺乏系统刻画，单加速器推理导致资源利用不足。
method: 对移动SoC中GPU与NPU等异构加速器进行系统性能刻画，并据此设计协同推理方案。
result: 揭示单一加速器方案在算力与内存带宽上的利用不足，验证异构协同的收益。
conclusion: 为边缘设备上利用NPU等加速器实现低延迟推理提供了实测指导。
---

## Abstract
With the rapid advancement of artificial intelligence technologies such as ChatGPT, AI agents, and video generation, contemporary mobile systems have begun integrating these AI capabilities on local devices to enhance privacy and reduce response latency. To meet the computational demands of AI tasks, current mobile SoCs are equipped with diverse AI accelerators, including GPUs and Neural Processing Units (NPUs). However, there has not been a comprehensive characterization of these heterogeneous processors, and existing designs typically only leverage a single AI accelerator for LLM inference, leading to suboptimal use of computational resources and memory bandwidth. In this paper, we first summarize key performance characteristics of heterogeneous processors, SoC memory bandwidth, etc. Drawing on these observations, we propose different heterogeneous parallel mechanisms to fully exploit both GPU and NPU computational power and memory bandwidth. We further design a fast synchronization mechanism between heterogeneous processors that leverages the unified memory architecture. By employing these techniques, we present HeteroInfer, the fastest LLM inference engine in mobile devices which supports GPU-NPU heterogeneous execution. Evaluation shows that HeteroInfer delivers a 1.34× to 6.02× end-to-end speedup over state-of-the-art GPU-only and NPU-only LLM engines, while maintaining negligible interference with other applications.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
刻画移动SoC中NPU与GPU以加速设备端低延迟大模型推理。

### 2. 核心内容
移动系统开始本地集成AI能力以提升隐私并降低响应延迟，当前SoC配备GPU与神经处理单元等多样加速器，但缺乏对异构处理器的系统刻画，且现有设计通常只用单一加速器做推理，导致算力与内存带宽利用不足。论文系统总结了异构处理器的关键性能特征，并据此提出协同利用多种加速器的推理方案。该工作为边缘NPU等加速器上的低延迟设备端推理提供了实测依据与优化方向。

### 3. 对应检索需求
edge AI accelerator hardware for low-latency on-device inference。

### 4. 来源与原文
- Source：SOSP-2025-ACM
- OpenReview：[https://dl.acm.org/doi/10.1145/3731569.3764808](https://dl.acm.org/doi/10.1145/3731569.3764808)
