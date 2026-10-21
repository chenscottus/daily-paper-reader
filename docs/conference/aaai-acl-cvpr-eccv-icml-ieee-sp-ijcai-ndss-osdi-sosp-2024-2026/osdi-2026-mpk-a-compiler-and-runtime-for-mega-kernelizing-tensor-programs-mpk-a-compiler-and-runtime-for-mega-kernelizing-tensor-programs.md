---
title: "{MPK}: A Compiler and Runtime for {Mega-Kernelizing} Tensor Programs"
title_zh: MPK：面向张量程序巨型内核化的编译器和运行时
authors: "Xinhao Cheng, Zhihao Zhang, Yu Zhou, Jianan Ji, Jinchen Jiang, Zepeng Zhao, Ziruo Xiao, Zihao Ye, Yingyi Huang, Ruihang Lai, Hongyi Jin, Bohan Hou, Mengdi Wu, Yixin Dong, Anthony Yip, Zihao Ye, Songting Wang, Wenqin Yang, Xupeng Miao, Tianqi Chen, Zhihao Jia"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://www.usenix.org/system/files/osdi26-cheng.pdf"
tags: ["query:litert-ops"]
score: 6.0
evidence: 将张量程序巨型内核化并实现跨算子流水线
tldr: 针对传统逐算子执行模型难以实现跨算子流水线和通信计算重叠的问题，MPK提出首个自动将多GPU模型推理转换为单一高性能巨型内核的编译器和运行时。它构建SM级任务图，将张量程序编译为优化任务并生成CUDA实现，由持久化巨型内核运行时执行。该设计使跨算子软件流水线和细粒度通信计算重叠成为可能，从而提升多GPU推理性能。
source: OSDI-2026-USENIX
selection_source: conference_retrieval
motivation: 传统逐算子执行模型难以实现跨算子流水线和通信计算重叠，限制多GPU推理性能。
method: MPK构建SM级任务图，将张量程序编译为优化任务并生成CUDA实现，由持久化巨型内核运行时执行。
result: MPK能把多GPU模型推理自动转换为单一高性能巨型内核，并支持细粒度流水与重叠。
conclusion: 该工作为张量程序提供了超越逐算子执行的新型编译与运行时范式。
---

## Abstract
We introduce Mirage Persistent Kernel (MPK), the first compiler and runtime system that automatically transforms multi-GPU model inference into a single high-performance mega-kernel. MPK introduces an SM-level graph representation that captures data dependencies at the granularity of individual streaming multiprocessors (SMs), enabling cross-operator software pipelining, fine-grained overlap of computation and communication, and other optimizations that are infeasible under the conventional kernel-per-operator execution model. The MPK compiler lowers tensor programs into optimized SM-level task graphs and generates fast CUDA implementations for each task, while the MPK in-kernel parallel runtime executes these tasks within a single persistent mega-kernel using decentralized scheduling across SMs. Together, these components provide end-to-end kernel fusion with minimal developer effort, while preserving the flexibility of existing programming models. Our evaluation shows that MPK significantly outperforms existing kernel-per-operator LLM serving systems, achieving up to 1.7× lower end-to-end inference latency and pushing LLM inference performance close to the limits of the underlying hardware. MPK is publicly available at https://github.com/mirage-project/mirage .

---

## 论文详细总结（自动生成）

# MPK：面向张量程序巨型内核化的编译器与运行时（OSDI '26）论文总结

## 1. 核心问题与整体含义

- **背景**：主流 ML 系统把模型表示为张量
