---
title: "MACKO: Sparse matrix-vector multiplication for low sparsity"
title_zh: MACKO：面向低稀疏度的稀疏矩阵向量乘
authors: "Vladimír Macko, Vladimír Boža"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf/6eb5a9e7314cc8f61ae3dc151f3e4ba53ea097a8.pdf"
tags: ["query:litert-ops"]
score: 5.0
evidence: 面向稀疏推理加速的GPU优化SpMV内核协同设计
tldr: "稀疏大模型推理依赖稀疏矩阵向量乘，但现有方法在30-90%的低非结构化稀疏度下性能不佳，内存收益和加速有限。本文提出MACKO，将存储格式与GPU内核协同设计，使其契合GPU执行模型并降低存储开销。作者指出内存带宽是主要瓶颈，实验显示在50%稀疏度下首次取得显著加速，为稀疏推理提供高效内核方案。"
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 剪枝大模型的低非结构化稀疏度下，现有SpMV方法内存收益与加速有限。
method: 提出GPU优化的存储格式与内核协同设计的MACKO-SpMV，并分析存储开销与带宽瓶颈。
result: "在50%稀疏度下首次实现显著加速，无需专用硬件单元或预计算。"
conclusion: 格式与内核协同设计可在低稀疏度下有效提升SpMV性能，支撑稀疏大模型推理。
---

## Abstract
Sparse Matrix-Vector Multiplication (SpMV) is a fundamental operation in the inference of sparse Large Language Models (LLMs).
Because existing SpMV methods perform poorly under the low, unstructured sparsity ($30-90\\%$) commonly observed in pruned LLMs, unstructured pruning provides only limited memory reduction and speedup.
We propose **MACKO-SpMV**, a GPU-optimized format and kernel co-designed to reduce storage overhead while remaining compatible with the GPU’s execution model.
This enables efficient SpMV for unstructured sparsity without specialized hardware units or precomputation.
We identify memory bandwidth as the primary limiting factor of SpMV and analyze the storage overhead of MACKO.
At $50\\%$ sparsity, MACKO is the first approach to achieve $1.5\times$ memory reduction and $1.2-1.5\times$ speedup over the dense baseline as well as substantial improvements over other SpMV methods: cuSPARSE ($2.8-13.0\times$), Sputnik ($1.9-2.6\times$), and DASP ($2.2-2.5\times$).
An LLM pruned with Wanda to sparsity $50\\%$ requires $1.5\times$ less memory and achieves $1.5\times$ faster inference at fp16 precision.
As a result, **unstructured pruning at $50\\%$ sparsity becomes practical** for real-world LLM workloads and **bridges the efficiency gap with structured 2:4 sparsity**.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
面向稀疏推理加速的GPU优化SpMV内核协同设计。

### 2. 核心内容
稀疏大模型推理依赖稀疏矩阵向量乘，但现有方法在30-90%的低非结构化稀疏度下性能不佳，内存收益和加速有限。本文提出MACKO，将存储格式与GPU内核协同设计，使其契合GPU执行模型并降低存储开销。作者指出内存带宽是主要瓶颈，实验显示在50%稀疏度下首次取得显著加速，为稀疏推理提供高效内核方案。

### 3. 对应检索需求
operator kernel optimization for accelerated inference。

### 4. 来源与原文
- Source：ICML-2026-Accepted
- OpenReview：[https://openreview.net/forum?id=ah9xkFXCV6](https://openreview.net/forum?id=ah9xkFXCV6)
