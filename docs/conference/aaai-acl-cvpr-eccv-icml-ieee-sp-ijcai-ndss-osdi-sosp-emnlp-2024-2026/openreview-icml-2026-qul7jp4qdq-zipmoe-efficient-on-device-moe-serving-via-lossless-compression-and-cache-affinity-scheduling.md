---
title: "ZipMoE: Efficient On-Device MoE Serving via Lossless Compression and Cache-Affinity Scheduling"
title_zh: ZipMoE：基于无损压缩与缓存亲和调度的设备端高效MoE服务
authors: "Yuchen Yang, Yaru Zhao, Pu Yang, Shaowei Wang, Zhi-Hua Zhou"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf/49bf4e4e63ca3db10c69e66da0ec207b033b7226.pdf"
tags: ["query:coral-npu"]
score: 4.0
evidence: 利用边缘硬件特性的无损设备端MoE推理服务系统
tldr: 大模型中的混合专家架构虽提升表达能力，但其巨大内存占用严重阻碍在资源受限边缘设备上的部署，且在不使用有损量化时难以保持模型行为。作者提出ZipMoE，一种语义无损的设备端MoE服务系统，利用边缘设备硬件特性与专家参数统计冗余进行缓存与调度协同设计，并给出可证明的性能保证。该方法将设备端MoE推理从I/O瓶颈转变为计算瓶颈，为低功耗边缘端高效部署大模型提供了新思路。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: MoE大模型内存占用过高，难以在资源受限边缘设备上无损部署。
method: 提出ZipMoE，结合无损压缩与缓存亲和调度，利用边缘硬件特性与参数冗余协同优化。
result: 将设备端MoE推理从I/O受限转为计算受限，并获得可证明的性能保证。
conclusion: 为资源受限边缘设备上的大模型高效推理提供了无损部署路径。
---

## Abstract
While Mixture-of-Experts (MoE) architectures substantially bolster the expressive power of large-language models, their prohibitive memory footprint severely impedes the practical deployment on resource-constrained edge devices, especially when model behavior must be preserved without relying on lossy quantization. In this paper, we present ZipMoE, an efficient and semantically lossless on-device MoE serving system. ZipMoE exploits the synergy between the hardware properties of edge devices and the statistical redundancy inherent to MoE parameters via a caching-scheduling co-design with provable performance guarantee. Fundamentally, our design shifts the paradigm of on-device MoE inference from an I/O-bound bottleneck to a compute-centric workflow that enables efficient parallelization. We implement a prototype of ZipMoE and conduct extensive experiments on representative edge computing platforms using popular open-source MoE models and real-world workloads. Our evaluation reveals that ZipMoE achieves up to 72.77% inference latency reduction and up to $6.76\times$ higher throughput than the state-of-the-art systems. Our code is available at:  https://github.com/npnothard/ZipMoE-ICML26.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
利用边缘硬件特性的无损设备端MoE推理服务系统。

### 2. 核心内容
大模型中的混合专家架构虽提升表达能力，但其巨大内存占用严重阻碍在资源受限边缘设备上的部署，且在不使用有损量化时难以保持模型行为。作者提出ZipMoE，一种语义无损的设备端MoE服务系统，利用边缘设备硬件特性与专家参数统计冗余进行缓存与调度协同设计，并给出可证明的性能保证。该方法将设备端MoE推理从I/O瓶颈转变为计算瓶颈，为低功耗边缘端高效部署大模型提供了新思路。

### 3. 对应检索需求
low power machine learning inference on embedded edge devices。

### 4. 来源与原文
- Source：ICML-2026-Accepted
- OpenReview：[https://openreview.net/forum?id=QUL7jp4QDQ](https://openreview.net/forum?id=QUL7jp4QDQ)
