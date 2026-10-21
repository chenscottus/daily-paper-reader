---
title: "Unleash All Cores: {Asymmetry-Aware} Scalable {DNN} Inference on Mobile {CPUs}"
title_zh: 释放所有核心：移动CPU上非对称感知的可扩展DNN推理
authors: "Qianlong Sang, Puyi He, Huanghuang Liang, Yili Gong, Chuang Hu, Xiaobo Zhou, Dazhao Cheng"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://www.usenix.org/system/files/osdi26-sang.pdf"
tags: ["query:litert-ops"]
score: 6.0
evidence: 核亲和感知调度与异构CPU加速移动端DNN推理
tldr: 移动设备上的非对称多核CPU运行DNN推理时，简单地把任务分摊到小核会因负载不均造成吞吐骤降，而静态划分又难以适应运行时干扰。作者提出SANI框架，通过核亲和感知的算子调度、动态负载均衡与干扰自适应机制充分利用异构核心。实验表明该框架可避免性能崩塌并显著提升移动端推理吞吐。该工作为本地推理在CPU加速器上的算子级调度优化提供了可借鉴的思路。
source: OSDI-2026-USENIX
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-001.webp\", \"caption\": \"\", \"page\": 4, \"index\": 1, \"width\": 1253, \"height\": 577}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-002.webp\", \"caption\": \"\", \"page\": 4, \"index\": 2, \"width\": 2326, \"height\": 1426}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-003.webp\", \"caption\": \"\", \"page\": 5, \"index\": 3, \"width\": 6000, \"height\": 1200}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-004.webp\", \"caption\": \"\", \"page\": 5, \"index\": 4, \"width\": 1047, \"height\": 340}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-005.webp\", \"caption\": \"\", \"page\": 5, \"index\": 5, \"width\": 390, \"height\": 366}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-006.webp\", \"caption\": \"\", \"page\": 5, \"index\": 6, \"width\": 4800, \"height\": 3000}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-007.webp\", \"caption\": \"\", \"page\": 5, \"index\": 7, \"width\": 942, \"height\": 746}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-008.webp\", \"caption\": \"\", \"page\": 7, \"index\": 8, \"width\": 1329, \"height\": 625}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-009.webp\", \"caption\": \"\", \"page\": 7, \"index\": 9, \"width\": 717, \"height\": 366}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-010.webp\", \"caption\": \"\", \"page\": 9, \"index\": 10, \"width\": 1384, \"height\": 460}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-011.webp\", \"caption\": \"\", \"page\": 10, \"index\": 11, \"width\": 6654, \"height\": 2434}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-012.webp\", \"caption\": \"\", \"page\": 11, \"index\": 12, \"width\": 3221, \"height\": 1731}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-013.webp\", \"caption\": \"\", \"page\": 11, \"index\": 13, \"width\": 1124, \"height\": 441}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-014.webp\", \"caption\": \"\", \"page\": 12, \"index\": 14, \"width\": 2349, \"height\": 913}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-015.webp\", \"caption\": \"\", \"page\": 12, \"index\": 15, \"width\": 2349, \"height\": 801}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-016.webp\", \"caption\": \"\", \"page\": 12, \"index\": 16, \"width\": 2348, \"height\": 851}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-017.webp\", \"caption\": \"\", \"page\": 12, \"index\": 17, \"width\": 3584, \"height\": 1127}, {\"url\": \"assets/figures/osdi-2026-usenix/osdi-2026-unleash-all-cores-asymmetry-aware-scalable-dnn-inference-on-mobile-cpus/fig-018.webp\", \"caption\": \"\", \"page\": 13, \"index\": 18, \"width\": 3174, \"height\": 893}]"
motivation: 移动端非对称多核CPU上简单跨核调度会导致负载失衡与吞吐崩塌，静态划分也无法适应运行时干扰。
method: 提出SANI框架，采用核亲和感知的算子调度、动态负载均衡与运行时干扰自适应三项机制。
result: 实验显示SANI避免小核加入带来的性能崩塌，充分利用异构核心并显著提升DNN推理吞吐。
conclusion: 该工作表明亲和感知调度可释放移动CPU多核潜力，为端侧推理加速提供通用调度范式。
---

## Abstract
Asymmetric Multiprocessing (AMP) CPUs are now central to mobile devices, but exploiting them for efficient Deep Neural Network (DNN) inference remains challenging. Naive scheduling across heterogeneous cores often triggers a performance-collapse paradox: adding LITTLE cores degrades throughput due to workload imbalance. Existing approaches rely on static partitioning, which partially mitigates imbalance but fails to adapt to runtime interference, incurs extra task acquisition overhead, and ignores core–kernel affinities—leaving substantial performance untapped. We present SANI, a scalable, asymmetry-aware inference framework that unleashes the full potential of AMP architectures. SANI introduces three key mechanisms: (1) an affinity-aware kernel issuer that selects cluster-optimal kernels to exploit core–kernel efficiency from the outset; (2) an adaptive granularity scheduler that dynamically merges or splits tasks, balancing load under runtime interference by mapping smaller tasks to slower cores and larger ones to faster cores; and (3) an on-demand kernel switcher that efficiently transforms kernels during workload migration, preserving affinity across clusters. We implement SANI atop Arm-CL and evaluate it on five mobile SoCs. SANI reduces DNN inference latency by 17.6%–23.7% on average (up to 29.5% on some models) while lowering energy consumption by up to 39% compared to state-of-the-art baselines, scaling efficiently across both symmetric and asymmetric CPU configurations.

---

## 论文详细总结（自动生成）

# 论文总结：Unleash All Cores — 移动 CPU 上非对称感知的可扩展 DNN 推理（SANI，OSDI '26）

## 1. 核心问题与整体含义

- **背景**：移动 SoC 普遍采用 AMP（Asymmetric Multiprocessing）架构，如 big.LITTLE（1+3+4）。CPU 是端侧推理最普遍可用的后端——论文实测在 Pixel 9 上，优化良好的 CPU 推理在 MobileNetV3 上比移动 GPU 快 6–11×（GPU 受 kernel launch 与数据传输开销拖累，且 FIFO 调度与渲染争抢），而 NPU 受算子兼容性限制（Pixel 9 上 SqueezeNet 回退 CPU 慢 10×，OnePlus 12 上 ResNet-50、Inception-V3 直接失败）。
- **核心悖论（performance-collapse paradox）**：直觉上把算子输出张量切块分给所有核心应提升吞吐，但实测**加入 LITTLE 核反而使端到端延迟增加最多 37%**。根因是负载不均：大核线程在同步屏障处空等慢速小核线程。
- **现有方案的三类局限**：
  - *仅对称执行*（Arm-CL 默认）：只用大核，小核闲置，浪费算力与能效；
  - *比例静态划分*（MNN）：按固定大/小核性能比切分，理想条件下缓解失衡，但在移动端普遍存在的运行时干扰下十分脆弱；
  - *细粒度动态派发*（AsyMo）：任务过细，出队频繁，任务获取开销高。
- **被忽视的维度**：**core–kernel affinity（核–内核亲和性）**。大核与小核微架构不同，偏好不同的算子实现；现有框架对每个算子只选一个默认 kernel 并跨簇统一使用。实测对齐亲和性可带来 **超过 30%** 的性能提升。
- **总体含义**：论文主张 AMP 上缺失两项关键能力——(1) 运行时可控的工作负载粒度以在干扰下维持均衡；(2) 利用核–内核亲和性的机制。SANI 即针对这两点提出，目标是让"大核的延迟优势"与"小核的能效优势"叠加而非相互抵消。

## 2. 方法论

### 2.1 总体架构（三组件协同）
工作流为：**亲和感知 kernel 派发器**（初始化）→ 统一 block 抽象与分簇队列 → **自适应粒度调度器**（运行时）→ **按需 kernel 切换器**（迁移时）。三者分别覆盖"主动选择""运行时均衡""反应式转换"。

### 2.2 亲和感知 Kernel 派发器（Affinity-Aware Kernel Issuer）
- **Kernel 选择**：为算子 $O_j$ 在簇 $H$ 上对每个候选 kernel $K_i$ 计算亲和性得分（与执行延迟成反比）：
  $$A(O_j, K_i, H) = \alpha \cdot C(O_j,K_i) + \beta \cdot M(O_j,K_i) + \gamma \cdot I(K_i,H)$$
  其中 $C$ 为计算成本（依张量形状与 kernel 逻辑估算算术量），$M$ 为访存成本（依 kernel 数据访问模式估算数据搬运量），$I$ 为指令兼容性得分（SIMD 指令组合与目标簇微架构的匹配度）。权重 $\alpha,\beta,\gamma$ 由离线 profiling 的线性回归拟合确定。最终：
  $$K^*_H(O_j) = \arg\max_{K_i} A(O_j, K_i, H)$$
  若大核与小核的最优 kernel 不同，则触发 **dual-kernel issue**，为两簇分别准备 kernel。
- **Block 统一**：以 kernel 无关的 "block" 作为统一调度单元。分区沿输出张量的高/宽维，强制 **height-first 迭代序** 以保证计算顺序一致；block 尺寸取两种 kernel 原生输出 tile 尺寸的 **最小公倍数（LCM）**（例：大核 2×4、小核 2×2 → 统一 block 2×4）。由此建立 block 索引 ↔ 两种 kernel workload 索引的明确映射，为迁移奠定基础。
- **分簇队列**：每个 cluster 独立队列（而非全局队列），块数按簇相对算力分配；好处是保留私有 cache 局部性，并避免全局队列每次出队的争用。
- **异步准备**：双 kernel 的内存分配、权重预转置、索引表构建等单线程准备任务并发提交至线程池，在模型加载阶段执行一次，屏障同步后进入在线推理，从而隐藏额外 kernel 的准备延迟。

### 2.3 自适应粒度调度器（Adaptive Granularity Scheduler）
- **设计原则**："能力越大责任越大"——执行快的线程承担更大的合并负载（不易成为瓶颈），慢线程处理更小的负载（避免拖累全局）。
- **数据结构与算子**：`struct W { split_dim; start[MAX_DIM]; end[MAX_DIM]; kernel; }`。`merge()` 将若干连续 workload 沿指定维合并为一个大 workload；`split()` 沿某维把 workload 均分为多份，其余维边界不变。两者均在簇内队列中调整大小，并保留 kernel 指针。
- **算法流程（Algorithm 1）**：轮次执行。每线程维护计数器 $C_i$（初值 1）。每轮：若 $C_i \ge 1$，取出 $C_i$ 个 workload 并 `Merge`；否则取出 1 个并 `Split`——先找**最小的维度 d 使该维迭代数 > 线程数 T**，再拆成 T 份（避免产生过细任务）。执行后记录各线程完成时间并排序；最快的 $k$ 个线程 $C_i{+}{+}$，最慢的 $k$ 个线程 $C_i{-}{-}$。该机制自然吸收了系统干扰与处理器非对称性，逐步把任务粒度对齐到各线程的真实运行能力。
- **复杂度**：每个 workload 出队一次为 $O(N)$；跨轮合并总量 $\Theta(N)$；split 每次至多产生 $T$ 份子任务，为常数因子开销；每轮对 T 个完成时间排序为 $O(1)$。总体 **$O(N)$**。

### 2.4 按需 Kernel 切换器（On-Demand Kernel Switcher）
当某簇队列耗尽、调度器从另一簇迁移负载时，目标核可能拿到为另一架构准备的 workload。切换器做**在线 kernel 变换**：
- **预建直接映射查找表**：常数时间完成索引映射；
- **阈值过滤**：小于预设尺寸的 workload 免于转换，避免收益微小的额外开销；
- **列式转换策略**：针对调度器合并出的大 workload，利用合并块的 start/end 索引一次性取出整段目标索引，避免逐子块查找（并借 height-first 布局进一步优化）。
- **示例**：大核 kernel tile 1×2、小核 2×1，LCM 得统一 2×2 block，4×4 输出被划为 4 个 block；block 索引 2 对应大核 workload 索引 {3,4}、小核索引 {2,4}。切换器经阈值检查后常数时间查表，替换 kernel 指针并设置新 workload 索引，保证计算**同一输出区域**，正确性不受影响。

### 2.5 实现
- 约 **11.6k LoC**，构建于 **Arm-CL（tag v52.3）** 之上；以自定义 scheduler 覆写 `schedule_common()`，无需修改用户代码；用 pthread 做线程亲和绑定；重新实现 Arm-CL 的 `estimate_cycles()` 以纳入大/小核不同硬件画像。作者计划开源代码。

## 3. 实验设计

- **测试平台（5 个，跨厂商）**：Google Pixel 9（Tensor G4, 1+3+4, Android 14）、OPPO Find X3 Pro（SM8350, 1+3+4，改装以接 Monsoon 功率计）、Redmi K60（SM8475, 1+3+4）、OnePlus Ace 5 Ultra（MediaTek 9400, 1+3+4）、Odroid XU4 开发板（Exynos 5422, 4+4, Android 7）。
- **模型 / 负载**：MobileNetV2、ResNet-50、SqueezeNet、ShuffleNet、Inception-V3、GPT-2（transformer，因 Arm-CL 不支持故经 Arm NN + TFLite parser 部署）。全部 FP32、启用 NEON；视觉模型用 224×224 ImageNet 分辨率输入；GPT-2 用固定 20-token 随机 prompt。
- **基线**：`Native`（Arm-CL 默认）、`AsyMo`（latency-first 模式，核心算法移植进 Arm-CL）、`MNN`（调度策略在 Arm-CL 内重实现）。作者说明 TensorFlow Lite / ONNX Runtime / PyTorch Mobile 在 AMP 上表现出类似的静态对称假设缺陷，故未逐一对比，而选 AsyMo 作为主要非对称感知基线。
- **指标**：端到端推理延迟；能耗（功率测量）；扩展效率（相对"仅 4 大核"执行的加速比）。
- **实验组**：① 5 设备 × 6 模型的端到端延迟；② 模型差异分析（dual-issue/并行潜力比例、merge-split 频率、Inception-V3 的 Permute kernel 深入剖析）；③ 硬件差异分析（单线程 neon_sgemm
