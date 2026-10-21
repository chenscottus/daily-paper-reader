---
title: "EGG: An Expert-Guided Agent Framework for Kernel Generation"
title_zh: EGG：面向内核生成的专家引导智能体框架
authors: "Yaochen Han, Ke Fan, Hongxu Jiang, Wanqi Xu, Weiyu Xie, Runhua Zhang, Chenhui Zhu, Yixiang Zhang"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf/5d6c40298bc2f6af6ca7e305ff66653da10cb850.pdf"
tags: ["query:litert-ops"]
score: 4.0
evidence: 通过专家引导的LLM智能体自动生成高性能GPU内核
tldr: 针对LLM计算开销激增而高性能GPU内核依赖专家手工调优、LLM自动生成难以兼顾正确性与性能的问题，本文提出EGG专家引导智能体框架，将专家优化原则注入LLM的决策过程。方法上仿照专家工作流把内核生成分解为可引导的阶段，约束优化空间探索。实验表明该框架能生成更正确且高效的内核。其意义在于为算子内核自动优化提供了可迁移的智能体范式。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 高性能GPU内核开发依赖专家手工调优，现有LLM方法难以同时保证正确性与高性能。
method: 提出专家引导的智能体框架，将专家优化原则注入LLM决策并分解内核生成流程。
result: 实验显示该框架生成的内核在正确性与性能上均优于缺乏领域引导的基线。
conclusion: 为算子内核的自动化生成与优化提供了可复用的智能体范式。
---

## Abstract
High-performance GPU kernels are critical for reducing the exponentially growing computational costs of large language models (LLMs), but their development heavily relies on manual tuning by domain experts. While recent advances in LLM-based approaches show promise for automating kernel generation, they still struggle to achieve both correctness and high performance. This limitation primarily arises from the lack of domain-specific optimization guidance, hindering effective exploration of the optimization space. We propose $\textbf{EGG}$, an $\underline{E}$xpert-$\underline{G}$uided Agent Framework for Kernel $\underline{G}$eneration, which incorporates expert optimization principles to guide LLMs’ decisions. Inspired by expert workflows, we decompose kernel generation into two hierarchical stages: 1) algorithmic structure design, which establishes a high-quality computational structure foundation; 2) hardware-specific tuning, which performs targeted adjustments through parallel mapping, tensor tiling, and memory optimization. This staged decomposition defines explicit optimization objectives, structuring the design space to achieve progressive refinements. To this end, a stage-aware multi-agent collaboration mechanism is designed for inter and intra-stage context management, ensuring stable optimization trajectories. Experiments on KernelBench and real-world workloads show that EGG achieves a $2.13\times$ average speedup over PyTorch, outperforming existing agent-based and RL-based approaches.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
通过专家引导的LLM智能体自动生成高性能GPU内核。

### 2. 核心内容
针对LLM计算开销激增而高性能GPU内核依赖专家手工调优、LLM自动生成难以兼顾正确性与性能的问题，本文提出EGG专家引导智能体框架，将专家优化原则注入LLM的决策过程。方法上仿照专家工作流把内核生成分解为可引导的阶段，约束优化空间探索。实验表明该框架能生成更正确且高效的内核。其意义在于为算子内核自动优化提供了可迁移的智能体范式。

### 3. 对应检索需求
operator kernel optimization for accelerated inference。

### 4. 来源与原文
- Source：ICML-2026-Accepted
- OpenReview：[https://openreview.net/forum?id=8nti23Zqkt](https://openreview.net/forum?id=8nti23Zqkt)
