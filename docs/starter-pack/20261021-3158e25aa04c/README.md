<div class="dpr-topic-result-actions"><button type="button" data-topic-copy="docs/starter-pack/20261021-3158e25aa04c/papers.md">复制论文清单</button> <a href="docs/starter-pack/20261021-3158e25aa04c/papers.md" download data-no-router>下载 Markdown</a> <a href="docs/starter-pack/20261021-3158e25aa04c/papers.json" download data-no-router>下载 JSON</a> <button type="button" data-topic-continue="20261021-3158e25aa04c">继续生成阅读内容</button></div>

# mlir-iree · 入门导读

> 本导读基于所列论文的标题、摘要和已有速览，不代表已阅读全部全文。近期窗口不是完整领域史；检索不保证覆盖全部相关论文。模型导读需结合原文核验。

检索窗口：arXiv &#91;2025-10-21, 2026-10-21&#41;；会议 &#91;2024-10-21, 2026-10-21&#41;（UTC，结束日不含）

## 方向概览

本导读聚焦一个具体问题：如何把机器学习模型经由 MLIR 方言逐层下降，并借助 IREE 一类编译与运行时栈，最终在 FPGA SoC 等异构加速器上执行；同时覆盖相邻的编译栈与硬件协同设计实践，以便理解可复用的集成模式。输入论文中，直接涉及 IREE 的只有一篇（可伸缩打包布局，在 MLIR/IREE 中扩展分块、融合与向量化），没有任何一篇同时讨论 LiteRT、IREE 与 FPGA SoC 三者的端到端集成，也没有论文给出 LiteRT 到 MLIR/IREE 再到 FPGA 的完整流水线。因此，关于“LiteRT 模型如何接入 MLIR 与 IREE”以及“在 FPGA SoC 上的运行时集成模式”，现有证据不足，只能从相邻工作（MLIR 上的 FPGA 卸载、HAL 类运行时抽象、加速器后端生成、硬件-编译器协同设计）推断可能的切入点，不能视为已证实方案。导读仅覆盖本次检索窗口内的候选，不代表该领域完整历史。

依据：[Scalable Packed Layouts for Vector-Length-Agnostic ML Code Generation](/20251020-20261019/2605.12445v2)；[An MLIR pipeline for offloading Fortran to FPGAs via OpenMP](/20251020-20261019/2511.08713v1)；[Compiler and Hardware Co-Design for Accelerator Architectures](/20251020-20261019/2609.30099v1)；[TensorLift: Automatic Extraction of Tensor-Level ISA Semantics from Accelerator RTL via MLIR Semantic Lifting](/20251020-20261019/2604.13523v3)；[PolyBlocks: A Compiler Infrastructure for AI Chips and Programming Frameworks](/20251020-20261019/2603.06731v2)；[Hexagon-MLIR: An AI Compilation Stack For Qualcomm&&#35;x27;s Neural Processing Units &#40;NPUs&#41;](/20251020-20261019/2602.19762v1)；[An MLIR Lowering Pipeline for Stencils at Wafer-Scale](/20251020-20261019/2601.17754v1)；[Analyzing Latency Hiding and Parallelism in an MLIR-based AI Kernel Compiler](/20251020-20261019/2602.20204v1)；[Evaluating Cross-Architecture Performance Modeling of Distributed ML Workloads Using StableHLO](/20251020-20261019/2604.12090v1)；[An MLIR-Based Compilation Method for Large Language Models](/20251020-20261019/2607.15865v2)

## MLIR 到 IREE 的端到端编译与运行时衔接

这一子方向关注在 MLIR/IREE 生产编译栈内部做扩展，而不是另起炉灶。可伸缩打包布局工作把向量长度无关的打包数据布局与相应编译器扩展集成进 MLIR/IREE，并扩展分块、融合与向量化以支持可伸缩向量长度，在 Arm CPU 上生成 SVE 代码，报告相对 IREE 内既有 NEON 代码生成最高 1.45 倍加速，并优于 ExecuTorch、TorchInductor 与 eager 执行。该文是输入中唯一明确以 IREE 为集成目标的论文，可作为理解“在 IREE 流水线中新增布局与 lowering 能力”的样板，但它面向 CPU 向量化，不涉及 FPGA 或 LiteRT。关于 IREE HAL 运行时如何对接自定义设备，输入论文未提供直接证据。

依据：[Scalable Packed Layouts for Vector-Length-Agnostic ML Code Generation](/20251020-20261019/2605.12445v2)

## MLIR 上的 FPGA 卸载与 HLS 方言

面向 FPGA 的编译路径在输入中有一篇直接证据：该工作给出据作者称为首个在 MLIR 内通过 OpenMP target 指令实现选择性代码卸载到 FPGA 的实现，把 MLIR OpenMP 方言与高层次综合（HLS）方言结合，形成可移植的 FPGA 编译流，并以 Flang 作为前端示例，支持通过标准 OpenMP 指令手工优化被卸载内核。其要点是复用 MLIR 生态构件、保持前端无关。该文未涉及 IREE 运行时、LiteRT 或 SoC 级集成，也未给出与 IREE HAL 的对接方式，因此只能作为“MLIR 方言 + HLS 后端”这一集成形态的参考。

依据：[An MLIR pipeline for offloading Fortran to FPGAs via OpenMP](/20251020-20261019/2511.08713v1)

## 加速器后端生成与硬件-编译器协同设计

若要把编译栈落到自研或第三方加速器（包括 FPGA 上的定制单元），需要后端生成与协同设计方法。EAAC 提出可扩展加速器架构，用 MLIR 支持多前端接入，并提供最小编译器功能完成数据编排，原型为脉动阵列加嵌入式 RISC-V 核的 GEMM 加速器，在合成全连接层负载上相对纯 RISC-V 基线报告 28 倍执行时间加速，GEMM 本身仅占总执行时间 1.64%，并分析了硬件信号量分配随指令数线性增长的最坏情况。TensorLift 则从加速器 RTL 出发，用 8 遍 MLIR 流水线把位级 IR 逐步提升为张量级 ISA 规范，在 Gemmini 与 VTA 上覆盖 156 个 MLIR 文件中的全部硬件指令，位级 MLIR 总体分别减少 24.8% 与 41.2%，核心计算与数据搬移语义经 Z3 SMT 证明与 RTL 提取模型等价，接入 ACT 后生成的编译器后端与手写 Gemmini 内核性能相当（几何平均 1.014 倍）。这两篇说明“从硬件描述或架构约束自动得到编译器后端”的可行路径，但都未与 IREE 运行时或 LiteRT 前端打通。

依据：[Compiler and Hardware Co-Design for Accelerator Architectures](/20251020-20261019/2609.30099v1)；[TensorLift: Automatic Extraction of Tensor-Level ISA Semantics from Accelerator RTL via MLIR Semantic Lifting](/20251020-20261019/2604.13523v3)

## 面向 AI 芯片与框架的 MLIR 编译基础设施

多篇工作展示了以 MLIR 为基础、面向特定芯片或框架的完整编译栈。PolyBlocks 以 pass 流水线组合循环嵌套与 SSA 变换，依赖轻量仿射访问分析，配合解析代价模型与启发式实现多级分块、融合、片上暂存、矩阵单元映射与注意力融合，面向 PyTorch 与 JAX 的即时编译在 NVIDIA GPU 上与 Torch Inductor、XLA 相当或更优。Hexagon-MLIR 面向高通 Hexagon NPU，统一支持 Triton 内核与 PyTorch 模型下降，生成利用紧耦合内存的大内核，作者自述为进行中的工作。面向大语言模型的 MLIR 编译方法用 TopOp 高层图方言与 TpuOp 目标硬件方言分层下降，并把每个 Transformer 层拆为 prefill、prefill&#95;kv、decode 三阶段静态编译，已在 TPU-MLIR 与 LLM-TPU 项目中实现。这些论文共同给出“高层图方言—目标硬件方言—可部署二进制”的分层模式，可作为设计 LiteRT 模型下降路径时的结构参考，但它们均未讨论 IREE 或 FPGA SoC。

依据：[PolyBlocks: A Compiler Infrastructure for AI Chips and Programming Frameworks](/20251020-20261019/2603.06731v2)；[Hexagon-MLIR: An AI Compilation Stack For Qualcomm&&#35;x27;s Neural Processing Units &#40;NPUs&#41;](/20251020-20261019/2602.19762v1)；[An MLIR-Based Compilation Method for Large Language Models](/20251020-20261019/2607.15865v2)

## 专用目标上的 lowering 与运行时机制分析

两篇论文分别从专用硬件目标与运行时机制角度补充细节。面向 Cerebras 晶圆级引擎的 MLIR lowering 流水线把模板计算内核转换为该平台异步执行模型的 CSL 代码，在五个基准、三种 HPC 编程技术上于 WSE2 与 WSE3 运行，性能与手工优化代码相当或略优，且无需应用层改动；作者报告在 WSE3 上比 128 块 NVIDIA A100 快约 14 倍、比 128 节点 CPU 集群快约 20 倍。另一篇针对基于 MLIR 的 AI 内核编译器，用 Triton/Inductor 生成内核做消融实验，分离向量化、多线程与双缓冲三项编译器控制机制的贡献，发现向量化对带宽敏感内核贡献最大，多线程在调度开销摊薄后收益显著，双缓冲在传输与计算可重叠时带来额外收益。后者对理解“显式数据搬移 + 暂存缓冲”类运行时机制有直接帮助，这与 FPGA SoC 上的 DMA 与片上存储模式相近，但论文本身未涉及 FPGA。

依据：[An MLIR Lowering Pipeline for Stencils at Wafer-Scale](/20251020-20261019/2601.17754v1)；[Analyzing Latency Hiding and Parallelism in an MLIR-based AI Kernel Compiler](/20251020-20261019/2602.20204v1)

## 评测方法与跨架构建模

评测方面，输入提供两类可复用方法。其一是在 MLIR 流水线内做受控消融，逐项量化向量化、多线程与双缓冲的贡献，并考察多线程加速比随问题规模的缩放，适合评估新增 lowering 或运行时机制的实际收益。其二是以 StableHLO 作为统一负载表示做跨架构、跨保真度性能建模，把同一负载表示映射到解析模型、基于剖析的预测器与模拟器，在 GPU 与 TPU 上评估分布式 GEMM、ResNet 与 LLM 训练负载，结果显示 StableHLO 能保持跨架构的相对性能趋势，预测误差处于早期设计探索可接受范围，同时暴露了现有 GPU 模拟器随保真度变化的局限。另有工作以 ODS 派生的约束解码在多个 MLIR 方言上评测自然语言到 MLIR 的生成，报告在 linalg 上小模型达到 80.0% 通过验证（三随机种子均值，n=125），并明确把属性值主导验证语义的场景列为非胜出情形。这些方法可迁移到 FPGA 目标，但输入中没有针对 FPGA SoC 的实测评测。

依据：[Analyzing Latency Hiding and Parallelism in an MLIR-based AI Kernel Compiler](/20251020-20261019/2602.20204v1)；[Evaluating Cross-Architecture Performance Modeling of Distributed ML Workloads Using StableHLO](/20251020-20261019/2604.12090v1)；[Cross-Dialect Generalization Without Retraining: Benchmarks and Evaluation of Schema-Derived Constrained Decoding for MLIR](/20251020-20261019/2607.18254v1)

## 进展与局限

综合来看，进展集中在三点：MLIR 已成为连接多前端与多后端的公共基础设施，并已在 IREE 内部被扩展以支持新的数据布局与向量化；面向 FPGA 的 MLIR 卸载路径与 HLS 方言已有首个实现；从 RTL 或架构约束自动生成编译器后端并接入运行时生态的链条正在形成。局限同样明确：输入中没有论文同时覆盖 LiteRT、MLIR/IREE 与 FPGA SoC，LiteRT 在全部候选摘要中均未出现；IREE HAL 的设备抽象与 FPGA 后端对接方式缺乏直接证据；FPGA 相关工作多停留在编译流与原型验证，缺少与生产运行时（如 IREE）的集成报告与 SoC 级端到端评测。因此本导读只能给出结构参考与可迁移的评测方法，不能给出已验证的集成方案。

依据：[Scalable Packed Layouts for Vector-Length-Agnostic ML Code Generation](/20251020-20261019/2605.12445v2)；[An MLIR pipeline for offloading Fortran to FPGAs via OpenMP](/20251020-20261019/2511.08713v1)；[TensorLift: Automatic Extraction of Tensor-Level ISA Semantics from Accelerator RTL via MLIR Semantic Lifting](/20251020-20261019/2604.13523v3)；[Compiler and Hardware Co-Design for Accelerator Architectures](/20251020-20261019/2609.30099v1)；[Evaluating Cross-Architecture Performance Modeling of Distributed ML Workloads Using StableHLO](/20251020-20261019/2604.12090v1)

## 建议阅读顺序

以下为建议学习顺序，不是公布时间排序。

1. [Scalable Packed Layouts for Vector-Length-Agnostic ML Code Generation](/20251020-20261019/2605.12445v2) · 入门
   - 阅读理由：输入中唯一明确在 MLIR/IREE 内做扩展的论文，先建立对 IREE 编译流水线与布局、分块、向量化扩展点的具体认识。
   - 公布时间：2026-05-12（日精度）
   - arxiv · 2026-05-12（日精度）：[原文](https://arxiv.org/pdf/2605.12445v2) / [PDF](https://arxiv.org/pdf/2605.12445v2)

2. [Analyzing Latency Hiding and Parallelism in an MLIR-based AI Kernel Compiler](/20251020-20261019/2602.20204v1) · 入门
   - 阅读理由：用受控消融拆解向量化、多线程与双缓冲的贡献，帮助理解运行时机制与数据搬移在编译流水线中的位置，方法可迁移到其他目标。
   - 公布时间：2026-02-22（日精度）
   - arxiv · 2026-02-22（日精度）：[原文](https://arxiv.org/pdf/2602.20204v1) / [PDF](https://arxiv.org/pdf/2602.20204v1)

3. [An MLIR pipeline for offloading Fortran to FPGAs via OpenMP](/20251020-20261019/2511.08713v1) · 入门
   - 阅读理由：直接给出 MLIR 上经 OpenMP 指令卸载到 FPGA 并结合 HLS 方言的编译流，是理解 FPGA 后端接入形态的最直接入口。
   - 公布时间：2025-11-11（日精度）
   - arxiv · 2025-11-11（日精度）：[原文](https://arxiv.org/pdf/2511.08713v1) / [PDF](https://arxiv.org/pdf/2511.08713v1)

4. [An MLIR-Based Compilation Method for Large Language Models](/20251020-20261019/2607.15865v2) · 进阶
   - 阅读理由：展示高层图方言到目标硬件方言的分层下降与推理阶段拆分，可作为设计模型下降路径的结构模板。
   - 公布时间：2026-07-17（日精度）
   - arxiv · 2026-07-17（日精度）：[原文](https://arxiv.org/pdf/2607.15865v2) / [PDF](https://arxiv.org/pdf/2607.15865v2)

5. [PolyBlocks: A Compiler Infrastructure for AI Chips and Programming Frameworks](/20251020-20261019/2603.06731v2) · 进阶
   - 阅读理由：系统呈现基于 MLIR 的模块化 pass 流水线与代价模型驱动的自动代码生成，理解后端可复用基础设施的组织方式。
   - 公布时间：2026-03-06（日精度）
   - arxiv · 2026-03-06（日精度）：[原文](https://arxiv.org/pdf/2603.06731v2) / [PDF](https://arxiv.org/pdf/2603.06731v2)

6. [Compiler and Hardware Co-Design for Accelerator Architectures](/20251020-20261019/2609.30099v1) · 进阶
   - 阅读理由：给出编译器与硬件协同设计的最小闭环及量化结果，理解自研加速单元接入编译栈所需的最小功能集。
   - 公布时间：2026-09-24（日精度）
   - arxiv · 2026-09-24（日精度）：[原文](https://arxiv.org/pdf/2609.30099v1)

7. [TensorLift: Automatic Extraction of Tensor-Level ISA Semantics from Accelerator RTL via MLIR Semantic Lifting](/20251020-20261019/2604.13523v3) · 专题
   - 阅读理由：从 RTL 自动提取张量级 ISA 并生成编译器后端，理解后端生成自动化与形式化验证手段。
   - 公布时间：2026-04-15（日精度）
   - arxiv · 2026-04-15（日精度）：[原文](https://arxiv.org/pdf/2604.13523v3) / [PDF](https://arxiv.org/pdf/2604.13523v3)

8. [Hexagon-MLIR: An AI Compilation Stack For Qualcomm&&#35;x27;s Neural Processing Units &#40;NPUs&#41;](/20251020-20261019/2602.19762v1) · 专题
   - 阅读理由：面向具体 NPU 的开放编译栈案例，观察内核级下降与大内核、紧耦合内存利用的工程取舍。
   - 公布时间：2026-02-23（日精度）
   - arxiv · 2026-02-23（日精度）：[原文](https://arxiv.org/pdf/2602.19762v1) / [PDF](https://arxiv.org/pdf/2602.19762v1)

9. [An MLIR Lowering Pipeline for Stencils at Wafer-Scale](/20251020-20261019/2601.17754v1) · 专题
   - 阅读理由：面向异步分布式执行模型的 lowering 案例，对理解非传统目标上的语义桥接有参考价值。
   - 公布时间：2026-01-25（日精度）
   - arxiv · 2026-01-25（日精度）：[原文](https://arxiv.org/pdf/2601.17754v1) / [PDF](https://arxiv.org/pdf/2601.17754v1)

10. [Evaluating Cross-Architecture Performance Modeling of Distributed ML Workloads Using StableHLO](/20251020-20261019/2604.12090v1) · 专题
   - 阅读理由：以 StableHLO 为统一表示做跨架构性能建模，为在缺少实体 FPGA 时开展评测提供方法参考。
   - 公布时间：2026-04-13（日精度）
   - arxiv · 2026-04-13（日精度）：[原文](https://arxiv.org/pdf/2604.12090v1) / [PDF](https://arxiv.org/pdf/2604.12090v1)

11. [Cross-Dialect Generalization Without Retraining: Benchmarks and Evaluation of Schema-Derived Constrained Decoding for MLIR](/20251020-20261019/2607.18254v1) · 专题
   - 阅读理由：提供多方言 MLIR 生成与验证的评测基准与约束方法，可作为编译前端与验证环节的补充视角。
   - 公布时间：2026-05-14（日精度）
   - arxiv · 2026-05-14（日精度）：[原文](https://arxiv.org/pdf/2607.18254v1) / [PDF](https://arxiv.org/pdf/2607.18254v1)

任务：研究方向大礼包

固定窗口：arXiv &#91;2025-10-21, 2026-10-21&#41;；会议 &#91;2024-10-21, 2026-10-21&#41;（UTC，结束日不含）

本次候选评审上限 300，最终名单上限 100；本轮内容预算 10。

覆盖限制：仅检索配置范围内已有库存与预算内候选；向量/会议Top-k召回并非全库遍历，不保证找全相关论文。未核实录用、日期边界和缺失库存不能当作已覆盖。

缺失库存：eccv 2025、emnlp 2026、neurips 2026。

研究需求：LiteRT、MLIR 与 IREE 的编译与运行时集成 FPGA SoC

# mlir-iree

以下为本次固定最终名单，按相关性评分降序；不代表全部相关论文。

1. [Scalable Packed Layouts for Vector-Length-Agnostic ML Code Generation](https://arxiv.org/abs/2605.12445v2) · 分数 9
2. [Cross-Dialect Generalization Without Retraining: Benchmarks and Evaluation of Schema-Derived Constrained Decoding for MLIR](https://arxiv.org/abs/2607.18254v1) · 分数 7
3. [ScanWeaver: Compiler-Driven Parallelization of Affine Recurrences via Associative Scan Lowering](https://arxiv.org/abs/2606.00601v1) · 分数 7
4. [Compiler and Hardware Co-Design for Accelerator Architectures](https://arxiv.org/abs/2609.30099v1) · 分数 6
5. [An MLIR pipeline for offloading Fortran to FPGAs via OpenMP](https://arxiv.org/abs/2511.08713v1) · 分数 6
6. [Hexagon-MLIR: An AI Compilation Stack For Qualcomm&&#35;x27;s Neural Processing Units &#40;NPUs&#41;](https://arxiv.org/abs/2602.19762v1) · 分数 6
7. [PolyBlocks: A Compiler Infrastructure for AI Chips and Programming Frameworks](https://arxiv.org/abs/2603.06731v2) · 分数 6
8. [An MLIR-Based Compilation Method for Large Language Models](https://arxiv.org/abs/2607.15865v2) · 分数 6
9. [Analyzing Latency Hiding and Parallelism in an MLIR-based AI Kernel Compiler](https://arxiv.org/abs/2602.20204v1) · 分数 6
10. [TensorLift: Automatic Extraction of Tensor-Level ISA Semantics from Accelerator RTL via MLIR Semantic Lifting](https://arxiv.org/abs/2604.13523v3) · 分数 6
11. [An MLIR Lowering Pipeline for Stencils at Wafer-Scale](https://arxiv.org/abs/2601.17754v1) · 分数 6
12. [EquivFusion: Unifying Hardware Equivalence Checking from Algorithms to Netlists via MLIR](https://arxiv.org/abs/2604.16571v1) · 分数 6
13. [Evaluating Cross-Architecture Performance Modeling of Distributed ML Workloads Using StableHLO](https://arxiv.org/abs/2604.12090v1) · 分数 6

[按公布时间查看](#/starter-pack/20261021-3158e25aa04c/dates)

阅读内容：已完成 13，待补充 0。
