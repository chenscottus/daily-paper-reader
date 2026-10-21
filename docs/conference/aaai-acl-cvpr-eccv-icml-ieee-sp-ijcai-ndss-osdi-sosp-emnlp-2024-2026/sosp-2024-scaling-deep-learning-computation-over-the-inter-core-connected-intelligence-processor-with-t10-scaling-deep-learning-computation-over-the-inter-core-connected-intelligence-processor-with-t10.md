---
title: Scaling Deep Learning Computation over the Inter-Core Connected Intelligence Processor with T10
title_zh: 用T10在核间互联智能处理器上扩展深度学习计算
authors: "Yiqi Liu, Yuqi Xue, Yu Cheng, Lingxiao Ma, Ziming Miao, Jilong Xue, Jian Huang"
date: 2024
publication_date: 2024
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://dl.acm.org/doi/pdf/10.1145/3694715.3695955"
tags: ["query:coral-npu"]
score: 4.0
evidence: 利用片上互联的深度学习编译器支撑模型部署
tldr: 新型AI芯片通过核间高带宽低延迟链路与分布式片上内存扩展算力，但现有深度学习编译器难以利用这些核间通信能力，开发者无法发挥新架构优势。作者提出T10，首个面向核间互联智能处理器的深度学习编译器，对张量算子的计算与通信模式进行统一建模与调度。该工作释放了核间通信带宽与分布式片上内存的潜力，提升了深度学习计算的可扩展性。其编译器层面的加速器适配思路对Coral等边缘NPU的工具链支持具有方法借鉴价值。
source: SOSP-2024-ACM
selection_source: conference_retrieval
motivation: AI芯片采用众多并行核与核间高速链路，但现有深度学习编译器缺乏对核间通信与分布式片上内存的支持。
method: 提出深度学习编译器T10，对张量算子的计算与通信模式进行统一建模，以利用核间通信带宽和分布式片上内存。
result: 使开发者能够利用核间互联架构的带宽与片上内存资源，提升深度学习计算的扩展性。
conclusion: 为新型AI芯片架构提供了编译器层面的有效支撑。
---

## Abstract
As AI chips incorporate numerous parallelized cores to scale deep learning (DL) computing, inter-core communication is enabled recently by employing high-bandwidth and low-latency interconnect links on the chip (e.g., Graphcore IPU). It allows each core to directly access the fast scratchpad memory in other cores, which enables new parallel computing paradigms. However, without proper support for the scalable inter-core connections in current DL compilers, it is hard for developers to exploit the benefits of this new architecture. We present T10, the first DL compiler to exploit the inter-core communication bandwidth and distributed on-chip memory on AI chips. To formulate the computation and communication patterns of tensor operators in this new architecture, T10 introduces a distributed tensor abstraction rTensor. T10 maps a DNN model to execution plans with a generalized compute-shift pattern, by partitioning DNN computation into sub-operators and mapping them to cores, so that the cores can exchange data following predictable patterns. T10 makes globally optimized trade-offs between on-chip memory consumption and inter-core communication overhead, selects the best execution plan from a vast optimization space, and alleviates unnecessary inter-core communications. Our evaluation with a real inter-core connected AI chip, the Graphcore IPU, shows up to 3.3× performance improvement, and scalability support for larger models, compared to state-of-the-art DL compilers and vendor libraries.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
利用片上互联的深度学习编译器支撑模型部署。

### 2. 核心内容
新型AI芯片通过核间高带宽低延迟链路与分布式片上内存扩展算力，但现有深度学习编译器难以利用这些核间通信能力，开发者无法发挥新架构优势。作者提出T10，首个面向核间互联智能处理器的深度学习编译器，对张量算子的计算与通信模式进行统一建模与调度。该工作释放了核间通信带宽与分布式片上内存的潜力，提升了深度学习计算的可扩展性。其编译器层面的加速器适配思路对Coral等边缘NPU的工具链支持具有方法借鉴价值。

### 3. 对应检索需求
compiler and toolchain support for deploying models on Coral accelerators。

### 4. 来源与原文
- Source：SOSP-2024-ACM
- OpenReview：[https://dl.acm.org/doi/10.1145/3694715.3695955](https://dl.acm.org/doi/10.1145/3694715.3695955)
