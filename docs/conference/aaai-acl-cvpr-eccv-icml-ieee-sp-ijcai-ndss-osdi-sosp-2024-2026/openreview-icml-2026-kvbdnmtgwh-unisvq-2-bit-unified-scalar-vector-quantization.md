---
title: "UniSVQ: 2-bit Unified Scalar-Vector Quantization"
title_zh: UniSVQ：2比特统一标量-矢量量化
authors: "Haoyu Wang, Haiyan Zhao, Xingyu Yu, Zhangyang Yao, Xu Han, Zhiyuan Liu, Maosong Sun"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf/76f82538e8196ed3c2663323bcecf446f1dc413e.pdf"
tags: ["query:litert-ops"]
score: 4.0
evidence: 2比特量化保持与优化整型核的兼容
tldr: 2比特训练后量化利于大模型低成本部署，但标量量化精度下降明显，矢量量化又带来计算与存储开销。本文提出UniSVQ统一框架，将码字参数化为整数格点的仿射变换，从而在保持与优化整型核兼容的同时保留矢量量化的灵活性，并引入分块微调以最小化重构误差。实验表明该方法在高效硬件执行与精度之间取得良好平衡，为低比特推理提供硬件友好的量化方案。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 2比特量化利于大模型低成本部署，但标量量化精度差、矢量量化开销大。
method: 提出统一框架，将码字参数化为整数格点仿射变换，兼顾整型核兼容与矢量灵活性，并做分块微调。
result: 实验表明该方法在保持整型核兼容的同时取得优于标量量化的精度。
conclusion: 为2比特高效推理提供了兼顾精度与硬件友好的量化方案。
---

## Abstract
Post-training quantization at the 2-bit level enables low-cost deployment and inference acceleration for large language models (LLMs). Scalar quantization (SQ) and vector quantization (VQ) are two primary quantization methods, however, the former suffers from significant performance degradation, and the latter incurs computational and storage overhead. 
We propose UniSVQ, a unified 2-bit quantization framework that bridges scalar and vector quantization by parameterizing codewords as an affine transform of integer lattices.
This structure preserves compatibility with optimized integer kernels while retaining much of VQ's flexibility. 
We further introduce a data-driven block-wise fine-tuning strategy to directly minimize quantization reconstruction error.
Extensive experiments across multiple LLM families and zero-shot benchmarks demonstrate that UniSVQ consistently outperforms state-of-the-art SQ methods and achieves performance comparable to advanced VQ methods, while providing higher inference throughput.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
2比特量化保持与优化整型核的兼容。

### 2. 核心内容
2比特训练后量化利于大模型低成本部署，但标量量化精度下降明显，矢量量化又带来计算与存储开销。本文提出UniSVQ统一框架，将码字参数化为整数格点的仿射变换，从而在保持与优化整型核兼容的同时保留矢量量化的灵活性，并引入分块微调以最小化重构误差。实验表明该方法在高效硬件执行与精度之间取得良好平衡，为低比特推理提供硬件友好的量化方案。

### 3. 对应检索需求
quantized operator kernels for efficient hardware execution。

### 4. 来源与原文
- Source：ICML-2026-Accepted
- OpenReview：[https://openreview.net/forum?id=kVBDNMTGWh](https://openreview.net/forum?id=kVBDNMTGWh)
