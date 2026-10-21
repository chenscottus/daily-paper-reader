---
title: "KernelCraft: Benchmarking for Agentic Close-to-Metal Kernel Generation on Emerging Hardware"
title_zh: KernelCraft：面向新兴硬件的智能体近金属内核生成基准
authors: "Jiayi Nie, Haoran Wu, Yao Lai, Zeyu Cao, Cheng Zhang, Binglei Lou, Erwei Wang, Jianyi Cheng, Timothy M. Jones, Robert D. Mullins, Rika Antonova, Yiren Zhao"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf/864f5c7aad9255cb584145d582d990217bf68f7f.pdf"
tags: ["query:litert-ops"]
score: 5.0
evidence: 面向新兴加速器的智能体底层内核生成基准
tldr: 针对新兴AI加速器ISA需要手工编写底层内核、耗时且难以跨硬件扩展的问题，本文提出KernelCraft基准。它通过函数调用与反馈驱动的流程，评估LLM智能体为定制加速器生成并优化底层内核的能力。实验在多类新兴硬件上刻画了智能体生成有效高效内核的表现。其贡献在于为算子内核的自动化生成与硬件适配提供了首个系统性评测平台。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 新兴加速器需手工编写底层内核，耗时易错且难以跨硬件目标扩展。
method: 构建KernelCraft基准，以函数调用与反馈驱动流程评测智能体生成底层内核的能力。
result: 在多类新兴ISA硬件上刻画了智能体生成有效且高效内核的表现。
conclusion: 为算子内核自动化生成与硬件适配提供了首个系统性评测平台。
---

## Abstract
New AI accelerators with novel instruction set architectures (ISAs) often require developers to manually craft low-level kernels — a time-consuming and error-prone process that does not scale across hardware targets. This delays emerging hardware platforms from reaching the market. While prior LLM-based code generation has shown promise in mature GPU ecosystems, it remains unclear whether agentic LLM systems can quickly produce valid and efficient kernels for emerging hardware with new ISAs. We present KernelCraft: the first benchmark for evaluating an LLM agent's ability to generate and optimize low-level kernels for customized accelerators through a function-calling, feedback-driven workflow. We evaluate agent performance across three emerging accelerators on more than 20 machine-learning tasks, each with five diverse task configurations. Across four leading reasoning models, the strongest agents generate functionally correct kernels for unseen ISAs within a few refinement steps, and produce optimized kernels that match or outperform compiler baselines. These results demonstrate KernelCraft's potential to accelerate the accelerator chip development cycle. KernelCraft is available at https://kernelcraft-cam.github.io/.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
面向新兴加速器的智能体底层内核生成基准。

### 2. 核心内容
针对新兴AI加速器ISA需要手工编写底层内核、耗时且难以跨硬件扩展的问题，本文提出KernelCraft基准。它通过函数调用与反馈驱动的流程，评估LLM智能体为定制加速器生成并优化底层内核的能力。实验在多类新兴硬件上刻画了智能体生成有效高效内核的表现。其贡献在于为算子内核的自动化生成与硬件适配提供了首个系统性评测平台。

### 3. 对应检索需求
implementing operator kernels for the LiteRT runtime。

### 4. 来源与原文
- Source：ICML-2026-Accepted
- OpenReview：[https://openreview.net/forum?id=CG3TwM48M8](https://openreview.net/forum?id=CG3TwM48M8)
