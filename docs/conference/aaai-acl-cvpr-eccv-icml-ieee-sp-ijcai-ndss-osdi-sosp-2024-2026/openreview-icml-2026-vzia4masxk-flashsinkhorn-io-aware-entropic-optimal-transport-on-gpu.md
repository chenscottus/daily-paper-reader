---
title: "FlashSinkhorn: IO-Aware Entropic Optimal Transport on GPU"
title_zh: FlashSinkhorn：GPU上IO感知的熵正则最优传输
authors: "Felix X.-F. Ye, Xingjie Li, An Yu, Ming-Ching Chang, LINSONG CHU, Davis Wertheimer"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf/4b680647193df0ef1d87c02833c9495a32a72d37.pdf"
tags: ["query:litert-ops"]
score: 5.0
evidence: 采用分块与片上内存流式的融合Triton内核实现算子融合
tldr: 基于Sinkhorn迭代的熵正则最优传输在GPU上规模扩展时效率低下，稠密实现带来二次HBM流量，在线后端又缺乏内核融合。本文提出FlashSinkhorn，将对数域更新改写为带偏置点积的行级LogSumExp归约，从而支持FlashAttention式的分块与融合。融合Triton内核将数据块流经片上SRAM，显著降低IO开销并提升大规模求解效率。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: GPU上Sinkhorn求解器规模扩展时存在二次HBM流量与内核融合不足的效率问题。
method: 将对数域更新改写为行级LogSumExp归约，采用FlashAttention式分块与融合Triton内核。
result: 融合内核通过片上SRAM流式处理数据块，降低IO开销并提升大规模求解效率。
conclusion: IO感知的算子融合与分块策略可显著提升GPU上熵正则最优传输的可扩展性。
---

## Abstract
Entropic optimal transport (EOT) via Sinkhorn iterations is widely used in modern machine learning, yet GPU solvers remain inefficient at scale. Tensorized implementations suffer quadratic HBM traffic from dense $n\times m$ interactions, while existing online backends avoid storing dense matrices but still rely on generic tiled map-reduce reduction kernels with limited fusion. We present **FlashSinkhorn**, an IO-aware EOT solver for squared Euclidean cost that rewrites stabilized log-domain Sinkhorn updates as row-wise LogSumExp reductions of biased dot-product scores, the same normalization as transformer attention. This enables FlashAttention-style fusion and tiling: fused Triton kernels stream tiles through on-chip SRAM and update dual potentials in a single pass, substantially reducing HBM IO per iteration while retaining linear-memory operations. We further provide streaming kernels for transport application, enabling scalable first- and second-order optimization. On A100 GPUs, FlashSinkhorn achieves up to $32\times$ forward-pass and $161\times$ end-to-end speedups over state-of-the-art online baselines on point-cloud OT, improves scalability on OT-based downstream tasks. For reproducibility, we release an open-source implementation at https://github.com/ot-triton-lab/flash-sinkhorn.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
采用分块与片上内存流式的融合Triton内核实现算子融合。

### 2. 核心内容
基于Sinkhorn迭代的熵正则最优传输在GPU上规模扩展时效率低下，稠密实现带来二次HBM流量，在线后端又缺乏内核融合。本文提出FlashSinkhorn，将对数域更新改写为带偏置点积的行级LogSumExp归约，从而支持FlashAttention式的分块与融合。融合Triton内核将数据块流经片上SRAM，显著降低IO开销并提升大规模求解效率。

### 3. 对应检索需求
How are LiteRT operators optimized through kernel fusion, quantization, and memory layout。

### 4. 来源与原文
- Source：ICML-2026-Accepted
- OpenReview：[https://openreview.net/forum?id=VzIA4MASxK](https://openreview.net/forum?id=VzIA4MASxK)
