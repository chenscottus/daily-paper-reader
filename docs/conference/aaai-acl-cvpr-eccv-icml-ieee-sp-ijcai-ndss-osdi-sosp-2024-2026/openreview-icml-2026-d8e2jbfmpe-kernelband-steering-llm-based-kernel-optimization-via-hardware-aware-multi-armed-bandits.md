---
title: "KernelBand: Steering LLM-based Kernel Optimization via Hardware-Aware Multi-Armed Bandits"
title_zh: KernelBand：以硬件感知多臂老虎机引导基于LLM的内核优化
authors: "Dezhi Ran, Shuxiao Xie, Mingfang Ji, Anmin Liu, Mengzhou Wu, Yuan Cao, Yuzhe Guo, Hao Yu, Linyi Li, Yitao Hu, Wei Yang, Tao Xie"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf/792dc9e4fc94b365e5eaba384679c3306f2c4e02.pdf"
tags: ["query:litert-ops"]
score: 5.0
evidence: 面向加速推理的硬件感知内核优化搜索
tldr: 高性能GPU内核优化依赖深厚系统经验，而代码大模型虽能生成正确代码却难以高效搜索庞大优化空间。KernelBand将内核优化建模为多臂老虎机问题，显式平衡探索与利用以引导代码大模型。该框架针对不同硬件与计算模式搜索优化策略，提升了内核优化的效率。其硬件感知搜索思路可迁移至算子内核的性能优化。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: GPU内核优化依赖专家经验，现有代码大模型难以高效探索庞大的优化空间。
method: 提出KernelBand，将内核优化建模为多臂老虎机问题，平衡探索与利用引导LLM搜索。
result: 在多样硬件与计算模式下更高效地搜索内核优化策略。
conclusion: 为算子内核的自动化性能优化提供了硬件感知的可迁移搜索框架。
---

## Abstract
High-performance GPU kernels are critical for efficient LLM serving, yet their optimization remains a bottleneck requiring deep system expertise. While code LLMs show promise in generating functionally correct code, kernel optimization is intrinsically a search problem over a vast optimization space. This fundamental mismatch prevents existing LLM agents from efficiently exploring the optimization space for diverse hardware and compute patterns. To bridge the gap, we present KernelBand, a framework that formulates kernel optimization as a Multi-Armed Bandit (MAB) problem, explicitly balancing exploration and exploitation to unlock the potential of code LLMs. To navigate the infinite arm space of optimization strategies applied to candidate kernels, we design two key mechanisms: a hardware-aware pruning strategy via profiling bounds and a runtime-behavior clustering algorithm that leverages Lipschitz continuity. Extensive experiments on TritonBench-G with three GPU architectures and four code LLMs show that KernelBand consistently outperforms the strongest available agent baseline, achieving up to **1.91×** geometric mean speedup over correctly optimized kernels with **39–140%** relative improvement in Fast@1 success rate. Our code is available at https://github.com/TongmingLAIC/KernelBand.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
面向加速推理的硬件感知内核优化搜索。

### 2. 核心内容
高性能GPU内核优化依赖深厚系统经验，而代码大模型虽能生成正确代码却难以高效搜索庞大优化空间。KernelBand将内核优化建模为多臂老虎机问题，显式平衡探索与利用以引导代码大模型。该框架针对不同硬件与计算模式搜索优化策略，提升了内核优化的效率。其硬件感知搜索思路可迁移至算子内核的性能优化。

### 3. 对应检索需求
operator kernel optimization for accelerated inference。

### 4. 来源与原文
- Source：ICML-2026-Accepted
- OpenReview：[https://openreview.net/forum?id=d8e2jBfMpE](https://openreview.net/forum?id=d8e2jBfMpE)
