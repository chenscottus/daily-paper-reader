---
title: "FourTune: Towards Fully 4-Bit Efficient Post-Training for Diffusion Models"
title_zh: FourTune：面向扩散模型的全四位高效后训练
authors: "Bowen Xue, Zihan Min, Xingyang Li, Zhekai Zhang, Haocheng Xi, Lvmin Zhang, Maneesh Agrawala, Jun-Yan Zhu, Song Han, Yujun Lin, Muyang Li"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf/f779639f9535424e1d38cbb76dcddf5ea0ff992c.pdf"
tags: ["query:litert-ops"]
score: 4.0
evidence: 面向高效执行的端到端W4A4G4四位量化流程
tldr: 扩散模型的后训练受限于巨大内存开销与缓慢训练速度，现有参数高效微调方法只能部分缓解。本文提出FourTune，一种端到端W4A4G4的四位后训练框架，通过三分支混合流水线结合冻结的数值稳定器来隔离量化敏感离群值。该设计使模型能在原生四位精度下稳定训练，为低精度高效后训练提供可行路径。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 扩散模型后训练内存开销大、速度慢，现有参数高效微调方法难以充分缓解。
method: 提出端到端W4A4G4框架，采用三分支混合流水线加冻结数值稳定器隔离量化敏感离群值。
result: 使扩散模型可在原生四位精度下稳定训练，降低内存并提升后训练效率。
conclusion: 端到端低精度后训练框架可在保持稳定的同时显著降低扩散模型后训练成本。
---

## Abstract
Diffusion models have become a dominant paradigm for high-quality generative modeling, while post-training is essential for adapting them to diverse downstream applications.
However, post-training of large diffusion models is still challenging due to the prohibitive memory footprints and slow training speed, which existing parameter-efficient fine-tuning methods only partially address.
To overcome these limitations, we propose FourTune, an efficient post-training framework for diffusion models based on an end-to-end W4A4G4 paradigm. FourTune introduces a triple-branch hybrid pipeline that augments the standard LoRA architecture with a frozen numerical stabilizer to isolate quantization-sensitive outliers, enabling stable training under native 4-bit computation. In addition, FourTune employs hardware-efficient block-wise quantization and customized fused kernels to support efficient quantized backpropagation and reduce memory bandwidth overhead.
Across customization, reinforcement learning, and distillation tasks, FourTune matches the quality of full-precision fine-tuning. On FLUX.1-dev (12B), FourTune reduces memory overhead by $2.25\times$ and increases end-to-end training throughput by $2.27\times$ compared to BF16 LoRA.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
面向高效执行的端到端W4A4G4四位量化流程。

### 2. 核心内容
扩散模型的后训练受限于巨大内存开销与缓慢训练速度，现有参数高效微调方法只能部分缓解。本文提出FourTune，一种端到端W4A4G4的四位后训练框架，通过三分支混合流水线结合冻结的数值稳定器来隔离量化敏感离群值。该设计使模型能在原生四位精度下稳定训练，为低精度高效后训练提供可行路径。

### 3. 对应检索需求
quantized operator kernels for efficient hardware execution。

### 4. 来源与原文
- Source：ICML-2026-Accepted
- OpenReview：[https://openreview.net/forum?id=RpwnrBkht2](https://openreview.net/forum?id=RpwnrBkht2)
