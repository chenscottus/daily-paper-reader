---
title: "Ave: Guiding Agentic GPU Optimization Using Data-Flow Invariants"
title_zh: Ave：利用数据流不变量指导智能体GPU优化
authors: "Haohui Mai, Xiaoyan Guo, Xiangyun Ding, Daifeng Li, Chenzhun Guo, Cong Wang, Jiacheng Zhao, Christos Kozyrakis, Binhang Yuan"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://dl.acm.org/doi/pdf/10.1145/3830418.3843902"
tags: ["query:litert-ops"]
score: 6.0
evidence: 以数据流不变量作为GPU内核优化的编译期护栏
tldr: 针对LLM编码智能体生成的GPU内核虽正确但性能落后于专家库、且单元测试和性能剖析反馈稀疏的问题，Ave提出以数据流不变量作为编译期护栏的智能体优化框架。它通过基于Tile的Python DSL暴露硬件指令和编译策略，帮助智能体定位并保持全局约束。该工作为GPU内核自动优化提供了可迁移方法，可启发算子内核性能调优。
source: SOSP-2026-ACM
selection_source: conference_retrieval
motivation: LLM智能体生成的GPU内核虽正确，但性能仍落后于专家库，且现有反馈稀疏。
method: Ave以数据流不变量作为编译期护栏，结合Tile级Python DSL指导智能体进行GPU内核优化。
result: 该框架帮助智能体协调共享内存、流水线和指令调度等底层变换，从而提升GPU吞吐。
conclusion: Ave为算子内核的智能体化性能优化提供了可迁移的编译期约束方法。
---

## Abstract
LLM coding agents can generate correct GPU kernels, yet their performance still lags behind expert libraries. Achieving peak GPU throughput requires coordinating low-level transformations such as shared-memory staging, software pipelining, and instruction scheduling. Unit tests and performance profiles provide only sparse end-to-end feedback, leaving agents unable to localize violations of the global constraints that these transformations must preserve. We present Ave, an agentic framework that uses data-flow invariants as compile-time guardrails for kernel optimization. These invariants specify relationships among data values that must hold throughout execution. Ave's tile-based Pythonic DSL exposes hardware instructions and compiler policies while abstracting complex memory layout encodings as tiles. Tag functions assign symbolic labels derived from selected logical coordinates and expressions. The compiler propagates these labels through data and control flow, and tag assertions enforce required relationships among them at use sites. The compiler checks these assertions with a tractable, flow-sensitive, path-insensitive analysis backed by an SMT solver, returning concrete assignments that witness abstract violations and guide repairs with no runtime overhead. An in-context reinforcement learning (ICRL) planner proposes optimizations from a curated knowledge base. A lowering agent implements them and instantiates their invariants. We evaluate Ave on AMD MI300X across GEMM, flash attention, and MoE, which together account for up to 90% of GPU time in LLM inference. With GPT-5.6 Sol, its optimized kernels achieve 89-99% of the effective throughput of state-of-the-art hand-optimized libraries and improve geometric-mean throughput by 1.62-1176× over uncontaminated agentic baselines. On 200 KernelBench tasks, with GPT-5.6 Sol and in-context DSL examples, Ave produces valid kernels within three attempts for 100% of Level 1 and 88% of Level 2 problems. Ave is open source at https://github.com/causalflowai/avelang.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
以数据流不变量作为GPU内核优化的编译期护栏。

### 2. 核心内容
针对LLM编码智能体生成的GPU内核虽正确但性能落后于专家库、且单元测试和性能剖析反馈稀疏的问题，Ave提出以数据流不变量作为编译期护栏的智能体优化框架。它通过基于Tile的Python DSL暴露硬件指令和编译策略，帮助智能体定位并保持全局约束。该工作为GPU内核自动优化提供了可迁移方法，可启发算子内核性能调优。

### 3. 对应检索需求
operator kernel optimization for accelerated inference。

### 4. 来源与原文
- Source：SOSP-2026-ACM
- OpenReview：[https://dl.acm.org/doi/10.1145/3830418.3843902](https://dl.acm.org/doi/10.1145/3830418.3843902)
