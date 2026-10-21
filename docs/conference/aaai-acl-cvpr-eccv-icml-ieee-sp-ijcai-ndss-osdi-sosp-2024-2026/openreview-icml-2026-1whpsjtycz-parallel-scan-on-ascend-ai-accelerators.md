---
title: Parallel Scan on Ascend AI Accelerators
title_zh: 昇腾AI加速器上的并行扫描
authors: "Bartłomiej Wróblewski, Gioele Gottardo, Anastasios Zouzias"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf/ff4bc478b553e4a39b30c60773e07d3b801e7da8.pdf"
tags: ["query:litert-ops"]
score: 5.0
evidence: 面向昇腾NPU加速器的高性能前缀和算子内核
tldr: "大模型部署需针对硬件优化少量通用并行原语，而前缀和（scan）在昇腾NPU上缺乏高效实现。本文设计面向昇腾架构的高性能scan算法，充分利用向量与矩阵单元，最高达到内存拷贝带宽的74.9%。作者将其作为快速原语应用于多个AI与LLM工作负载，验证了实际加速效果。该工作展示了算子内核针对特定硬件加速器优化的一般方法。"
source: ICML-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 大模型需要针对特定硬件优化通用并行原语，而前缀和在昇腾NPU上缺乏高效算子实现。
method: 设计面向昇腾架构的高性能scan算法，充分利用向量与矩阵单元提升带宽利用率。
result: "达到内存拷贝带宽的74.9%，并在多个AI与LLM工作负载中实现加速。"
conclusion: 表明针对硬件加速器定制算子内核可有效加速通用计算原语。
---

## Abstract
Large Language Models (LLMs) depend on model- and architecture-specific optimizations to be efficiently
executed at a large scale. The number of new LLM variants grows rapidly, making it necessary to distill and
only optimize a few common parallel computational primitives, including prefix sum (scan). In this work, we
design high-performance prefix-sum algorithms for the Ascend NPU architecture and explore their applicability
in AI workloads and LLMs. The key feature of our algorithms is the efficient use of vector and matrix units in
the Ascend architecture, which allows us to reach up to 74.9% of the memory bandwidth achieved by memory
copy. To showcase the effectiveness of matrix-multiplication-based scans as a fast primitive in AI workloads, we
implemented several essential scan-based operators like radix sort and top-p, achieving respectively up to 3.3×
and 2.3× speedup compared to the vector-only kernels. Finally, we show how these optimized kernels can impact
real world models on the Ascend NPU obtaining 2.02× speed-up on state-space neural networks like Mamba, and
up to 1.47× on LLMs with large vocabulary sizes thanks to more efficient top-p token sampling.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
面向昇腾NPU加速器的高性能前缀和算子内核。

### 2. 核心内容
大模型部署需针对硬件优化少量通用并行原语，而前缀和（scan）在昇腾NPU上缺乏高效实现。本文设计面向昇腾架构的高性能scan算法，充分利用向量与矩阵单元，最高达到内存拷贝带宽的74.9%。作者将其作为快速原语应用于多个AI与LLM工作负载，验证了实际加速效果。该工作展示了算子内核针对特定硬件加速器优化的一般方法。

### 3. 对应检索需求
hardware acceleration for tensor operator execution。

### 4. 来源与原文
- Source：ICML-2026-Rejected-Public
- OpenReview：[https://openreview.net/forum?id=1WhPSJtycZ](https://openreview.net/forum?id=1WhPSJtycZ)
