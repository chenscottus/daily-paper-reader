---
title: "KernelFoundry: Hardware-Aware Evolutionary GPU Kernel Optimization"
title_zh: KernelFoundry：硬件感知的进化式GPU核优化
authors: "Nina Wiedemann, Quentin Leboutet, Michael Paulitsch, Diana Wofk, Benjamin Ummenhofer"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf/1addc06656d11927aec61a4c027187eff6a10535.pdf"
tags: ["query:litert-ops"]
score: 4.0
evidence: 面向硬件的GPU算子核进化式优化
tldr: GPU核优化要求理解硬件架构与并行策略，而现有基于大模型的方法仅靠性能剖析反馈考虑硬件，探索能力有限。本文提出KernelFoundry进化框架，通过MAP-Elites质量多样性搜索保持探索，用元提示进化协同挖掘任务相关优化策略，并以模板化参数调优精化核实现。该框架能高效探索GPU核空间，生成更优的硬件感知核，为算子核自动优化提供了可迁移方法。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有基于大模型的GPU核生成仅通过性能剖析反馈考虑硬件，优化能力有限。
method: 提出进化框架，结合MAP-Elites质量多样性搜索、元提示进化与模板化参数调优。
result: 该框架高效探索GPU核空间，生成更优的硬件感知核实现。
conclusion: 为面向硬件的算子核自动优化提供了可迁移的进化式方法。
---

## Abstract
GPU kernel optimization challenges LLMs beyond standard coding tasks, as it requires an understanding of hardware architecture, parallel computing optimization strategies, and profiling outputs. However, most existing approaches leveraging LLMs for kernel generation apply standard prompting and feedback loops, considering hardware only through profiling feedback. We introduce KernelFoundry, an evolutionary framework that efficiently explores the space of GPU kernels through (1) MAP-Elites quality-diversity search with kernel-specific behavioral dimensions to sustain exploration; (2) meta-prompt evolution that co-evolves prompts with kernels to uncover task-specific optimization strategies, and (3) a template-based parameter optimization approach to tune kernels to inputs and hardware. We evaluate this framework on KernelBench, robust-kbench and custom tasks, generating SYCL kernels as a cross-platform GPU programming paradigm, and CUDA kernels for comparison to prior work. Our approach consistently outperforms the baseline methods and achieves an average speedup of 2.3 on KernelBench for SYCL. Moreover, KernelFoundry is implemented as a distributed framework with remote access to diverse hardware, allowing quick benchmarking and featuring a flexible user input layer to support kernel generation for a wide range of real use cases beyond benchmarking.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
面向硬件的GPU算子核进化式优化。

### 2. 核心内容
GPU核优化要求理解硬件架构与并行策略，而现有基于大模型的方法仅靠性能剖析反馈考虑硬件，探索能力有限。本文提出KernelFoundry进化框架，通过MAP-Elites质量多样性搜索保持探索，用元提示进化协同挖掘任务相关优化策略，并以模板化参数调优精化核实现。该框架能高效探索GPU核空间，生成更优的硬件感知核，为算子核自动优化提供了可迁移方法。

### 3. 对应检索需求
operator kernel optimization for accelerated inference。

### 4. 来源与原文
- Source：ICML-2026-Accepted
- OpenReview：[https://openreview.net/forum?id=4wy2YAHKRF](https://openreview.net/forum?id=4wy2YAHKRF)
