---
title: Segmented Operations using Matrix Multiplications
title_zh: 使用矩阵乘法的分段运算
authors: "Aleksandros Sobczyk, Giuseppe Sorrentino, Anastasios Zouzias"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf/df619d6c07af5d075bab7df5411b60ea46b5fc87.pdf"
tags: ["query:litert-ops"]
score: 4.0
evidence: 利用矩阵乘法单元在加速器上执行分段深度学习运算
tldr: 针对现代AI加速器中矩阵乘法单元除稠密矩阵乘外常被闲置、缺乏理论计算模型的问题，本文提出MMV-RAM模型，在向量RAM基础上加入矩阵乘法单元。方法通过分段运算将基本深度学习操作映射为小矩阵乘，并给出计算能力平衡的理论分析。分析表明该模型可高效执行更多算子。其意义在于为加速器上的算子执行与算法设计提供理论指导。
source: ICML-2026-Rejected-Public
selection_source: conference_retrieval
motivation: AI加速器的矩阵乘法单元在稠密矩阵乘之外常被闲置，且缺乏严谨计算模型。
method: 提出MMV-RAM模型，在向量RAM上增加矩阵乘法单元并以分段矩阵乘执行基本运算。
result: 给出矩阵与向量单元计算能力平衡的理论分析及分段运算的可行性证明。
conclusion: 为加速器上张量算子的高效执行与算法设计提供了理论模型基础。
---

## Abstract
Specialized computational units that perform small matrix multiplications as primitive operations are typically present in modern AI accelerators. However, these Matrix Multiplication Units (MMUs) are often underutilized for many fundamental deep learning operations besides dense matrix multiplications. Coincidentally, the lack of a rigorous theoretical model of computation for such architectures obstructs algorithmic design. In this work, we propose MMV-RAM, a computational model which judiciously extends the Vector-RAM model with an additional MMU. We provide a detailed theoretical analysis and carefully balance the computational power between the matrix and vector units, guided by the circuit complexity lower bound that parity is not in AC[0]. In MMV-RAM, we proceed to algorithm design, starting with two fundamental parallel operations: *segmented scan* and *sum*. By expressing them as compositions of elementary parallel primitives (e.g., seg. sum reduces to: scan, compress, and vector differentiation), we can  exploit MMUs to perform *speculative* blocked computations, ultimately leading to *provable theoretical speed-ups* against vector-only approaches. These results extend to other ubiquitous AI kernels, including dense matrix product, and sparse matrix-vector product. As a case study, we implemented the proposed algorithms on the Ascend 910B AI accelerator, which contains matrix and vector cores. We evaluate these implementations on synthetic and real-world datasets from various applications, including Large Language Models.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
利用矩阵乘法单元在加速器上执行分段深度学习运算。

### 2. 核心内容
针对现代AI加速器中矩阵乘法单元除稠密矩阵乘外常被闲置、缺乏理论计算模型的问题，本文提出MMV-RAM模型，在向量RAM基础上加入矩阵乘法单元。方法通过分段运算将基本深度学习操作映射为小矩阵乘，并给出计算能力平衡的理论分析。分析表明该模型可高效执行更多算子。其意义在于为加速器上的算子执行与算法设计提供理论指导。

### 3. 对应检索需求
hardware acceleration for tensor operator execution。

### 4. 来源与原文
- Source：ICML-2026-Rejected-Public
- OpenReview：[https://openreview.net/forum?id=PLQ7Yq2IzQ](https://openreview.net/forum?id=PLQ7Yq2IzQ)
