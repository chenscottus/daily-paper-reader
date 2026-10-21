---
title: "Unleash All Cores: {Asymmetry-Aware} Scalable {DNN} Inference on Mobile {CPUs}"
title_zh: 释放全部核心：移动CPU上非对称感知的可扩展DNN推理
authors: "Qianlong Sang, Puyi He, Huanghuang Liang, Yili Gong, Chuang Hu, Xiaobo Zhou, Dazhao Cheng"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://www.usenix.org/system/files/osdi26-sang.pdf"
tags: ["query:coral-npu"]
score: 5.0
evidence: 面向移动异构嵌入式CPU的可扩展低延迟DNN推理
tldr: 针对移动设备上非对称多核CPU难以高效运行DNN推理、朴素调度会因负载不均导致增加小核反而降低吞吐的问题，本文提出SANI框架。该框架引入核-内核亲和感知的调度、动态负载均衡与运行时干扰自适应三项机制，突破静态划分的局限。实验表明其能充分释放异构移动CPU的算力，实现可扩展的低延迟推理，为嵌入式边缘设备上的高效机器学习推理提供了通用调度思路。
source: OSDI-2026-USENIX
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-001.webp\", \"caption\": \"\", \"page\": 4, \"index\": 1, \"width\": 1253, \"height\": 577}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-002.webp\", \"caption\": \"\", \"page\": 4, \"index\": 2, \"width\": 2326, \"height\": 1426}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-003.webp\", \"caption\": \"\", \"page\": 5, \"index\": 3, \"width\": 6000, \"height\": 1200}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-004.webp\", \"caption\": \"\", \"page\": 5, \"index\": 4, \"width\": 1047, \"height\": 340}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-005.webp\", \"caption\": \"\", \"page\": 5, \"index\": 5, \"width\": 390, \"height\": 366}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-006.webp\", \"caption\": \"\", \"page\": 5, \"index\": 6, \"width\": 4800, \"height\": 3000}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-007.webp\", \"caption\": \"\", \"page\": 5, \"index\": 7, \"width\": 942, \"height\": 746}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-008.webp\", \"caption\": \"\", \"page\": 7, \"index\": 8, \"width\": 1329, \"height\": 625}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-009.webp\", \"caption\": \"\", \"page\": 7, \"index\": 9, \"width\": 717, \"height\": 366}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-010.webp\", \"caption\": \"\", \"page\": 9, \"index\": 10, \"width\": 1384, \"height\": 460}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-011.webp\", \"caption\": \"\", \"page\": 10, \"index\": 11, \"width\": 6654, \"height\": 2434}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-012.webp\", \"caption\": \"\", \"page\": 11, \"index\": 12, \"width\": 3221, \"height\": 1731}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-013.webp\", \"caption\": \"\", \"page\": 11, \"index\": 13, \"width\": 1124, \"height\": 441}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-014.webp\", \"caption\": \"\", \"page\": 12, \"index\": 14, \"width\": 2349, \"height\": 913}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-015.webp\", \"caption\": \"\", \"page\": 12, \"index\": 15, \"width\": 2349, \"height\": 801}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-016.webp\", \"caption\": \"\", \"page\": 12, \"index\": 16, \"width\": 2348, \"height\": 851}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-017.webp\", \"caption\": \"\", \"page\": 12, \"index\": 17, \"width\": 3584, \"height\": 1127}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-018.webp\", \"caption\": \"\", \"page\": 13, \"index\": 18, \"width\": 3174, \"height\": 893}]"
motivation: 移动设备广泛采用非对称多核CPU，但朴素跨核调度会因负载不均导致增加小核反而降低吞吐。
method: 提出SANI框架，包含核与内核亲和感知调度、动态负载均衡和运行时干扰自适应机制。
result: 在移动CPU上实现可扩展的DNN推理加速，充分释放异构核心算力。
conclusion: 为嵌入式边缘设备上的低延迟DNN推理提供了可迁移的异构调度方法。
---

## Abstract
Asymmetric Multiprocessing (AMP) CPUs are now central to mobile devices, but exploiting them for efficient Deep Neural Network (DNN) inference remains challenging. Naive scheduling across heterogeneous cores often triggers a performance-collapse paradox: adding LITTLE cores degrades throughput due to workload imbalance. Existing approaches rely on static partitioning, which partially mitigates imbalance but fails to adapt to runtime interference, incurs extra task acquisition overhead, and ignores core–kernel affinities—leaving substantial performance untapped. We present SANI, a scalable, asymmetry-aware inference framework that unleashes the full potential of AMP architectures. SANI introduces three key mechanisms: (1) an affinity-aware kernel issuer that selects cluster-optimal kernels to exploit core–kernel efficiency from the outset; (2) an adaptive granularity scheduler that dynamically merges or splits tasks, balancing load under runtime interference by mapping smaller tasks to slower cores and larger ones to faster cores; and (3) an on-demand kernel switcher that efficiently transforms kernels during workload migration, preserving affinity across clusters. We implement SANI atop Arm-CL and evaluate it on five mobile SoCs. SANI reduces DNN inference latency by 17.6%–23.7% on average (up to 29.5% on some models) while lowering energy consumption by up to 39% compared to state-of-the-art baselines, scaling efficiently across both symmetric and asymmetric CPU configurations.

---

## 论文详细总结（自动生成）

# 论文结构化总结：Unleash All Cores: Asymmetry-aware Scalable DNN Inference on Mobile CPUs（SANI, OSDI '26）

## 1. 核心问题与研究动机

- **背景**：移动 SoC 普遍采用非对称多核（AMP, big.LITTLE）架构，大小核共享物理地址空间、缓存一致，但各自拥有私有 L1/L2，共享 L3 与主存。CPU 是唯一"普适可用"的推理后端：论文在 Pixel 9 / OnePlus 12 上实测，轻量模型（MobileNetV3）上优化良好的 CPU 推理比移动 GPU 快 6–11 倍（GPU 受 kernel launch 与数据传输开销拖累，且 FIFO 调度会与渲染竞争）；NPU 则存在算子兼容性缺口（Pixel 9 上 SqueezeNet 回退 CPU 慢 10 倍，OnePlus 12 上多数模型直接失败）。
- **核心问题——"性能坍塌悖论"（performance-collapse paradox）**：直觉上把算子输出张量切块分给所有核应提升吞吐，但实测**加入 LITTLE 核反而使端到端时延最多增加 37%**。根因是负载不均：大核上的快线程在同步屏障处空等小核慢线程，等待时延主导了执行时间。
- **现有方案的三类局限**：
  - **仅对称执行**（Arm-CL 默认）：只用大核，小核闲置，浪费能效资源；
  - **按比例静态划分**（MNN）：按固定大小核算力比切分，理想条件下可缓解不均，但在移动端普遍存在的运行时干扰下十分脆弱；
  - **细粒度动态派发**（AsyMo）：任务过细导致频繁出队，任务获取开销高。
- **被忽视的第二个维度——核–内核亲和性（core–kernel affinity）**：大小核微架构不同（SIMD 宽度、缓存层次），偏好不同的算子实现。实测 GEMM 算子在大核上最优 kernel 比次优快 9.1%，DepthWise 卷积用错 kernel 性能下降超 34%；而对齐亲和性可带来 **30%+ 的性能提升**。现有框架在初始化时为每个算子只选一个 kernel 并全局统一使用，工作负载跨簇迁移时性能进一步劣化。
- **整体含义**：已有方法缺少两种能力——(1) 运行时对工作负载粒度的控制以在干扰下维持均衡；(2) 利用核–内核亲和性的机制。SANI 定位为**首个同时系统性处理"固有算力非对称"与"运行时干扰"的推理框架**，目标是算子级可并行负载，明确不面向大规模端侧 LLM 推理（该场景 NPU 更合适）。

## 2. 方法论：SANI 框架

### 2.1 总体架构
三个协同组件构成流水线：**亲和感知 kernel 下发器**（准备双 kernel、统一 block 抽象、按簇建队）→ **自适应粒度调度器**（运行时 Merge/Split 调粒度）→ **按需 kernel 切换器**（迁移时做索引重映射与 kernel 变换）。

### 2.2 亲和感知 Kernel 下发器（Affinity-Aware Kernel Issuer）
- **Kernel 选择**：对算子 $O_j$、候选实现 $K_i$、目标簇 $H$ 计算亲和分数
  $$A(O_j,K_i,H)=\alpha\cdot C(O_j,K_i)+\beta\cdot M(O_j,K_i)+\gamma\cdot I(K_i,H)$$
  其中 $C$ 为按张量形状与 kernel 逻辑估算的算术运算量，$M$ 为按访存模式估算的数据移动量，$I$ 为指令兼容性（如专用 SIMD 指令与簇微架构的匹配度）。分数与执行时延成反比；$\alpha,\beta,\gamma$ 通过离线剖析的线性回归拟合得到。最终 $K^*_H(O_j)=\arg\max_{K_i} A(O_j,K_i,H)$。若大小核最优 kernel 不同，则触发**双 kernel 下发**。
- **Block 统一（Block Unification）**：为消除不同 kernel 输出形状/计算顺序的异构性，定义 kernel 无关的"block"分区单元：
  - 沿输出张量的高、宽维度切分，并强制**高度优先（height-first）迭代序**以统一计算序列；
  - block 尺寸取候选 kernel 原生输出 tile 尺寸的**最小公倍数（LCM）**（如大核 tile 2×4、小核 tile 2×2 → 统一 block 2×4）；
  - 由此建立"单个 block 索引 ↔ 两种 kernel 各自 workload 索引"的映射，为调度与迁移奠基。
- **按簇队列**：初始化时按簇相对算力比例分发 block 到 big-queue / LITTLE-queue，而非单一全局队列。目的有二：线程取到的 block 已预绑定亲和 kernel 且数据在该簇私有缓存中已热；避免全局队列每次出队的争用。
- **异步准备**：双 kernel 的准备（每算子内存分配、权重按各 kernel 原生布局预转置、索引映射表构建）在模型加载时一次性完成，提交到线程池并发执行以掩盖时延，最后以屏障同步后再进入在线推理。

### 2.3 自适应粒度调度器（Adaptive Granularity Scheduler）
- **设计哲学**："能力越大责任越大"——执行更快的线程承担更大负载（不易成为瓶颈），慢线程承担更小负载以免拖累全局。
- **数据结构**：`struct W { int split_dim; int start[MAX_DIM]; int end[MAX_DIM]; void (*kernel)(); }`。
- **两个基本操作**：
  - `Merge`：把一组连续 workload 沿指定维合并为一个更大 workload（新块从首块起点跨到末块终点）；
  - `Split`：把一个大 workload 沿某维切成 n 份，其余维边界不变；两者均保持 workload 携带的 kernel 指针。
- **调度算法（Algorithm 1，按轮次执行）**：
  1. 每线程维护计数器 $C_i$，初值 1；轮次 $r$ 递增；
  2. 若 $C_i\ge 1$：从队列取 $C_i$ 个 workload 并 Merge 后执行；否则取 1 个 workload，找到满足 $w[d]>T$ 的最小维度 $d$，Split 成 $T$ 份；
  3. 记录各线程本轮完成时间 $t_i$ 并排序；
  4. **最快的 k 个线程** $C_i \mathrel{+}= 1$（下轮合并更多块），**最慢的 k 个线程** $C_i \mathrel{-}= 1$（下轮拆分）；
  5. 队列空则结束。
- **复杂度**：N 个 workload、T 线程下，每 workload 出队一次为 O(N)，跨轮 merge 总和 Θ(N)，split 每次至多产生 T 个子任务为常数因子开销，每轮排序 T 个时间为 O(1)，故总体 **O(N)**。
- 该机制天然同时吸收"处理器非对称"与"系统干扰"两种速度差异。

### 2.4 按需 Kernel 切换器（On-Demand Kernel Switcher）
当某簇队列耗尽、调度器从另一簇迁移 workload 时，目标核可能拿到为异构微架构准备的 kernel。切换器做**同计算区域的索引重映射**：
- **三项优化**：(1) 预建**直接映射查找表**实现常数时间查表；(2) **阈值过滤**，小于预设尺寸的 workload 免变换（收益不抵开销）；(3) 对 Merge 产生的大块采用**列式变换**，用起止索引一次性取出目标索引区间，避免逐子块查表，并借高度优先布局进一步优化。
- **示例**：大核亲和 kernel tile 1×2、小核 2×1 → 统一 block 2×2；4×4 输出张量被划为 4 个逻辑 block；block 索引 2 对应大核 workload 索引 {3,4}，经映射表查得同一计算区域对应小核 workload 索引 {2,4}；通过尺寸阈值检查后常数时间取回目标索引，替换 kernel 指针与 workload 索引，交由目标 kernel 执行，保证计算结果区域完全一致（正确性）同时在新宿主核上性能最优。

### 2.5 实现与可扩展性
- 约 **11.6k LoC** 实现在 Arm-CL (v52.3) 之上，计划开源。
- 以自定义 scheduler **重写 `schedule_common()`** 拦截执行流，无需修改用户代码；每簇独立队列 + 通过 pthread API 做线程绑核；重写 `estimate_cycles()` 以注入大小核各自的硬件画像；迁移时用预计算索引映射生成新 workload。
- **Scalability by design**：kernel 下发器使每簇总跑最优 kernel，把算子效率与核数量/类型解耦；调度器随并发数或干扰波动持续再平衡；切换器在迁移中保持亲和性——三者共同使系统无需针对特定硬件重新调参即可跨 SoC 扩展。

## 3. 实验设计

- **硬件平台（5 款，跨厂商）**：Google Pixel 9（Tensor G4，1+3+4，Android 14）、OPPO Find X3 Pro（SM8350，1+3+4，Android 13，改装以接 Monsoon 功率计）、Redmi K60（SM8475，1+3+4）、OnePlus Ace 5 Ultra（MediaTek 9400，1+3+4，Android 15）、Odroid XU4 开发板（
