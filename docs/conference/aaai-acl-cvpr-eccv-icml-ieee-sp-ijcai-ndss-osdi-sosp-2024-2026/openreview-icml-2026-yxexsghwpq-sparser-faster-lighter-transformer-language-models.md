---
title: "Sparser, Faster, Lighter Transformer Language Models"
title_zh: 更稀疏、更快、更轻的Transformer语言模型
authors: "Edoardo Cetin, Stefano Peluchetti, Emilio Castillo, Akira Naruse, Mana Murakami, Llion Jones"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf/b99d638d7972937a084b4e95c1b3804656590bb9.pdf"
tags: ["query:litert-ops"]
score: 4.0
evidence: 面向GPU高效稀疏推理的CUDA内核
tldr: "大型语言模型计算成本高昂，本文利用前馈层非结构化稀疏降低开销。方法提出新的稀疏打包格式与配套CUDA内核，无缝接入现代GPU优化执行管线，从而高效支持稀疏推理与训练。定量研究表明简单L1正则即可带来超过99%稀疏度且对下游性能影响甚微。该工作为算子级内核优化与硬件加速推理提供了可复用方法。"
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: LLM计算成本高，前馈层参数与FLOPs占比大。
method: 提出稀疏打包格式与CUDA内核并接入GPU执行管线。
result: "实现超99%稀疏度且下游性能几乎无损。"
conclusion: 为稀疏推理提供内核级硬件加速方案。
---

## Abstract
Scaling autoregressive large language models (LLMs) has driven unprecedented progress but comes with vast computational costs. In this work, we tackle these costs by leveraging unstructured sparsity within an LLM's feedforward layers, the components accounting for most of the model parameters and execution FLOPs. To achieve this, we introduce a new sparse packing format and a set of CUDA kernels designed to seamlessly integrate with the optimized execution pipelines of modern GPUs, enabling efficient sparse computation during LLM inference and training. To substantiate our gains, we provide a quantitative study of LLM sparsity, demonstrating that simple L1 regularization can induce over 99% sparsity with negligible impact on downstream performance. When paired with our kernels, we show that these sparsity levels translate into substantial throughput, energy efficiency, and memory usage benefits that increase with model scale. We will release all code and kernels under an open-source license to promote adoption and accelerate research toward establishing sparsity as a practical axis for improving the efficiency and scalability of modern foundation models.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
面向GPU高效稀疏推理的CUDA内核。

### 2. 核心内容
大型语言模型计算成本高昂，本文利用前馈层非结构化稀疏降低开销。方法提出新的稀疏打包格式与配套CUDA内核，无缝接入现代GPU优化执行管线，从而高效支持稀疏推理与训练。定量研究表明简单L1正则即可带来超过99%稀疏度且对下游性能影响甚微。该工作为算子级内核优化与硬件加速推理提供了可复用方法。

### 3. 对应检索需求
operator kernel optimization for accelerated inference。

### 4. 来源与原文
- Source：ICML-2026-Accepted
- OpenReview：[https://openreview.net/forum?id=YXEXsGHwpq](https://openreview.net/forum?id=YXEXsGHwpq)
