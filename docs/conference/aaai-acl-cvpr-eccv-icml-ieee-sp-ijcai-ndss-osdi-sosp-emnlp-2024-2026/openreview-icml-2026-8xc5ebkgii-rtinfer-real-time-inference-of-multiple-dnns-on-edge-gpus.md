---
title: "RTInfer: Real-Time Inference of Multiple DNNs on Edge GPUs"
title_zh: RTInfer：边缘GPU上多DNN的实时推理
authors: "Renjie Li, Tong Sun, Yi Gao, Wei Dong"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf/2615fa9a88e81a5951dafef1908976df7b230fe6.pdf"
tags: ["query:coral-npu"]
score: 6.0
evidence: 边缘GPU加速器上的实时并发DNN推理
tldr: 边缘GPU日益用于延迟敏感的DNN任务，但资源有限，并发负载下难以满足严格实时要求，现有抢占与早退机制利用率低且精度损失大。作者提出RTInfer系统，结合精度校准的轻量变体协同优化、内存布局感知调度与按需加载策略。实验表明其能在并发场景下兼顾吞吐与精度，直接支撑边缘加速器上的低延迟推理部署。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 边缘GPU资源有限，并发负载下现有机制难以满足严格实时要求且精度损失大。
method: 提出RTInfer，融合轻量变体协同优化、内存布局感知调度与按需加载。
result: 在并发实时任务下实现吞吐与精度的更优平衡并降低主机到GPU延迟。
conclusion: 为边缘加速器上的低延迟多任务DNN推理提供了系统级方案。
---

## Abstract
While edge GPUs are increasingly used for latency-critical DNN tasks, limited resources often fail to meet strict real-time (RT) requirements under concurrent workloads. 
Existing preemption and early-exit mechanisms often underutilize GPU resources through single-task queuing and sacrifice excessive accuracy during task bursts.
To address this, we propose RTInfer, a novel system that enables concurrent RT task execution while balancing throughput and accuracy.
RTInfer integrates an accuracy-calibrated lightweight variant co-optimization to generate efficient models, a memory-layout-aware scheduler to mitigate fragmentation during preemption, and an on-demand loading strategy to minimize host-to-GPU latency.
Extensive evaluations demonstrate that RTInfer outperforms state-of-the-art methods by reducing average deadline miss rate (DMR) from 32.8\% to 0\% and improving accuracy by up to 56.5\%.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
边缘GPU加速器上的实时并发DNN推理。

### 2. 核心内容
边缘GPU日益用于延迟敏感的DNN任务，但资源有限，并发负载下难以满足严格实时要求，现有抢占与早退机制利用率低且精度损失大。作者提出RTInfer系统，结合精度校准的轻量变体协同优化、内存布局感知调度与按需加载策略。实验表明其能在并发场景下兼顾吞吐与精度，直接支撑边缘加速器上的低延迟推理部署。

### 3. 对应检索需求
edge AI accelerator hardware for low-latency on-device inference。

### 4. 来源与原文
- Source：ICML-2026-Accepted
- OpenReview：[https://openreview.net/forum?id=8XC5ebkGII](https://openreview.net/forum?id=8XC5ebkGII)
