---
title: "Ladder: Enabling Efficient {Low-Precision} Deep Learning Computing through Hardware-aware Tensor Transformation"
title_zh: Ladder：通过硬件感知张量变换实现高效低精度深度学习计算
authors: "Lei Wang, Lingxiao Ma, Shijie Cao, Quanlu Zhang, Jilong Xue, Yining Shi, Ningxin Zheng, Ziming Miao, Fan Yang, Ting Cao, Yuqing Yang, Mao Yang"
date: 2024
publication_date: 2024
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://www.usenix.org/system/files/osdi24-wang-lei.pdf"
tags: ["query:litert-ops"]
score: 6.0
evidence: 通过张量变换将自定义低精度类型映射到硬件的编译器
tldr: 低精度计算有望提升深度学习性能，但现有软硬件对不断涌现的自定义数据类型支持不足，难以获得真实加速。本文提出Ladder编译器，借助通用类型系统tType与扩展的张量表达式，将低精度张量变换为硬件支持的格式。该工作弥合了自定义数据类型与固定硬件精度之间的鸿沟，为低精度算子执行提供通用编译支持。
source: OSDI-2024-USENIX
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/osdi-2024-usenix/osdi-2024-ladder-enabling-efficient-low-precision-deep-learning-computing-through-hardware-aware-tensor-transformation/fig-001.webp\", \"caption\": \"\", \"page\": 1, \"index\": 1, \"width\": 875, \"height\": 356}, {\"url\": \"assets/figures/osdi-2024-usenix/osdi-2024-ladder-enabling-efficient-low-precision-deep-learning-computing-through-hardware-aware-tensor-transformation/fig-002.webp\", \"caption\": \"\", \"page\": 1, \"index\": 2, \"width\": 875, \"height\": 360}]"
motivation: 低精度计算有潜力，但现有软硬件对新兴自定义数据类型支持不足，难以获得真实性能提升。
method: 提出Ladder编译器，基于通用类型系统tType和扩展张量表达式做硬件感知的张量变换。
result: 将低精度张量映射为硬件支持的精度格式，从而在现有硬件上实现低精度计算加速。
conclusion: 通过编译期张量变换可弥合自定义低精度类型与固定硬件格式之间的差距。
---

## Abstract
The increasing demand for improving deep learning model performance has led to a paradigm shift in supporting low-precision computation to harness the robustness of deep learning to errors. Despite the emergence of new low-precision data types and optimization approaches, existing hardware and software have insufficient and inefficient support for those evolving data types, making it challenging to achieve real performance gains through low-precision computing. This paper introduces Ladder, a novel compiler designed to bridge the gap between evolving custom data types and the fixed precision formats supported by current hardware. Leveraging a general type system, tType, and an extended tensor expression, Ladder transforms deep neural network (DNN) computations into optimized computing pipelines with custom data types as the first-class citizen, exposing an optimization space for efficiently handling data storage, accesses, and type conversions. Ladder employs a new set of tensor scheduling primitives and a hardware-aware optimization policy to navigate the complex transformation space, ensuring optimal performance across different memory layers and DNN operators. Our evaluation demonstrates Ladder's capability to systematically support a wide array of low-bit precision custom data types, significantly enhancing the performance of DNN computations on modern accelerators without necessitating hardware modifications. This innovation empowers model designers with the ability to explore data type optimizations and offers hardware vendors a flexible solution to expand their support for diverse precision formats.

---

## 论文详细总结（自动生成）

# Ladder 论文中文总结

## 1. 核心问题与整体含义

- **背景**：深度学习模型规模持续扩大（如 LLM），模型对误差的鲁棒性使得低精度计算成为提升性能的关键手段。工业界与学术界不断涌现新的低精度数据类型，包括 FP8、INT4、NF4、MXFP（分组缩放）、甚至 1-bit/2-bit 权重等。
- **核心矛盾**：硬件加速器（GPU）受芯片面积与成本限制，每次只能集成少量标准精度计算单元，难以跟上自定义数据类型的快速演化；同时现有软件对低位宽数据的支持低效——例如 NVIDIA GPU 共享内存 bank 宽度为 4 字节，直接存取 8-bit 元素会造成带宽浪费，需手工打包对齐。
- **量化证据**：高度优化的 CUTLASS 在 INT8 矩阵乘上仅达 422 TFLOPS（68% 利用率）；实测 V100/A100/MI250 上低精度 MatMul 平均利用率普遍低于 60%，且部分硬件已支持的精度（如 INT8）在编译器中缺乏支持，混合精度（如 FP16×INT8）则完全不被支持。
- **整体含义**：论文旨在**在不修改硬件的前提下**，让编译器系统性地支持任意自定义低精度数据类型，弥合“快速演化的自定义类型”与“固定硬件精度格式”之间的鸿沟，为模型设计者提供真实性能反馈的探索空间。

## 2. 方法论

### 核心思想
- **存储与计算分离**：利用硬件内存系统可存储任意位宽的不透明数据块这一事实，以自定义类型存储和传输张量（节省访存流量与内存占用），在计算前通过类型转换转为硬件支持的标准类型（如 NF4→FP16）再执行。
- **关键观察**：张量的存储与访问可变换为多种逻辑等价格式（行/列主序、分块、padding、不同访问粒度），不同格式性能差异巨大，因而存在一个可优化的“张量变换空间”。

### 关键技术细节
- **tType（通用类型系统）**：tile-wise 数据类型，显式定义类型宽度 `nElemBits`、元素形状 `shape`、共享元数据 `metadata`（如分组缩放因子），以及到更宽标准类型的无损转换函数表 `c_tTypes`。可表达 FP16、NF4、OCP-MXFP8（shape=[32]、共享 scale）等。
- **tTile**：带 tType 的细粒度张量抽象（形状 + 类型 + 元数据，行主序存储）。
- **tTile-Operator**：将 DNN 算子表示为一组独立同构的细粒度任务；扩展张量表达式，支持在表达式中用 `@FP16`、`@NF4`、`@FP32` 等**注解计算过程中的数据类型**（含输入、输出、中间累加），从而表达混合精度计算。
- **tTile-device**：将硬件抽象为多层 tTile 层次，每层（DRAM/L2/L1/L0/计算单元）的访问事务粒度、指令形状、支持类型均用 tTile 描述（如 A100 的 MMA 指令 [16,16]、ldmatrix [16,2]@16B、共享内存 [32]@4B、全局内存 [32]@1B）。
- **四个变换原语**：
  - `slice`：切分数据块（数据 tiling）；
  - `map`：按 `map_func` 重排元素地址（布局变换/对齐事务）；
  - `pad`：边界填充以对齐事务长度；
  - `convert`：依据 `c_tTypes` 调用转换函数改变 tType。
- **三阶段流水线**：每个 tTile-operator 的计算流水线拆为 `Transform-Load`（带变换的加载）、`Compute`、`Transform-Store`（带变换的存储）。
- **硬件感知逐层调度策略（Algorithm 1）**：
  - 先将图切分为可融合的子图（连通计算流水线）；
  - 选择“位宽最近”的计算核心（如 NF4 选 FP16 核 312 TFLOPS 而非 FP32 核 19.5 TFLOPS）；
  - 用 LCM 计算各层对齐的粒度（HintGranularity）与形状（HintShape）作为 hint；
  - 从顶层（core）向底层（DRAM）逐层调度 tiling 与变换，检查形状/类型对齐并按需插入 slice/pad/convert/map；
  - 候选方案经 profile 后选优。
- **map_func 推断**：按行主序将元素展平映射到目标事务大小（如 L0 的 [16,2] 映射为 L1 的 [4,8]）。
- **实现**：约 5K 行 Python/C++，基于 TVM（kernel 调度与代码生成）、Roller（tTile 配置推断）、Welder（端到端图优化）；输入 PyTorch 程序，导出扩展 ONNX 图；NVIDIA 侧使用 MMA PTX API、`cp.async`、LOP3 指令优化低位整数转换；AMD 侧实现 HIP 后端与 MFMA 指令。

## 3. 实验设计

- **硬件平台**：NVIDIA Tesla V100 (16GB)、A100 (80GB)、RTX A6000 (48GB)、RTX 4090（仅算子 benchmark）、AMD Instinct MI250 (128GB)；CUDA 12.1 / ROCm 5.7.0 / Ubuntu 20.04。
- **模型与场景**：
  - LLM：LLAMA-70B、BLOOM-176B（因显存限制，评测单 decoder 层，按层线性外推），配置 (BS,SEQ) = (1,1)、(32,1)、(1,4096)，覆盖在线/离线推理、prefill 与 decoding；
  - CV/语音：ResNet-50、ShuffleNet-V2、ViT-Base、Conformer-L，batch size 1 与 128。
- **数据类型配置**：WFP16AFP16、WINT4AFP16、WNF4AFP16、WFP8AFP8、WMXFP8AMXFP8、WINT1AINT8、WINT1AINT4、WINT4AINT4、WINT8AINT4 等，均取自现有文献配置，不引入额外质量损失。
- **端到端基线**：Welder、PyTorch-Inductor、ONNXRuntime、TensorRT、vLLM、vLLM-WINT4AFP16、AMOS、TensorIR（AMD 侧为 Welder、PyTorch-Inductor、ONNXRuntime、TensorIR，并为 Welder/TensorIR 补充 rocBLAS/MIOpen/rocWMMA 支持）。
- **算子 benchmark**：6 个 MatMul（M0–M5）+ 8 个 Conv2d（C0–C7），覆盖 7 种类型配置，对比 cuBLAS、CUTLASS、vLLM、cuDNN、AMOS、TensorIR。
- **其他实验**：内存占用（LLM 推理）、编译时间、优化逐项消融（图 14）、位宽缩放（W/A 从 INT8 到 INT1 逐级下降）、精度-效率联合评估（LLAMA2-3B/7B/13B/70B，对比 PTQ、GPTQ、AFP Q、BitDistiller、OneBit、BitNet-b1.58 等量化方法的 PPL 与延迟）。

## 4. 资源与算力

- 论文**未明确说明**所用 GPU 的具体数量、训练时长或总算力规模；实验均为**单卡推理评测**（每个 GPU 型号独立测试）。
- 论文聚焦推理，未涉及模型训练；LLM 评测因显存限制只运行单 decoder 层，并以层数线性外推全模型延迟。
- 未报告能耗、成本等指标。

## 5. 实验数量与充分性

- **规模**：端到端实验覆盖 6 个模型 × 多组数据类型 × 4 个 GPU 平台（A100/V100/A6000/MI250），另加 RTX 4090 算子实验；算子级 14 个算子 × 7 种类型；另有内存、编译时间、消融、位宽缩放、精度-效率等专项实验，总量较大。
- **充分性**：
  - 覆盖 NVIDIA 两代架构 + AMD 平台，跨厂商验证较好；
  - 数据类型从 FP16 到 1-bit、从元素级到分组缩放（MXFP）均有覆盖；
  - 消融实验（图 14）逐项拆解了变换、PTX 优化、整体调度的贡献，支撑了方法有效性归因。
- **公平性**：
  - 基线选择较全面（含 SOTA 编译器、厂商库、推理框架），并为 AMD 基线补充了矩阵核心支持以避免“不公平地赢”；
  - 数据类型配置均引用现有文献，避免自造有利配置；
  - 但部分基线（如 AMOS、TensorIR）在某些配置下“不支持”，导致对比维度不完整；部分对比（如与 vLLM）的公平性依赖其量化实现细节。

## 6. 主要结论与发现

- Ladder 是**首个系统性地在现有 GPU 上支持通用低比特自定义数据类型**的 DNN 编译器，无需硬件改动。
- 在硬件原生支持的类型（WFP16AFP16）上，A100 上相对 Welder 平均加速 1.0×–2.0×，V100 约 1.1×，A6000 约 1.2×；AMD MI250 上平均 1.5×–10.5×。
- 对硬件不支持的自定义类型：WINT4AFP16 相对 vLLM 平均 2.3×；WINT1AINT8 在 BLOOM 单层上相对 Welder 最高 10×（A100）、13.3×（V100）、14.6×（A6000）。
- 内存占用随位宽近线性下降：WINT1AINT8 相比 FP16，LLAMA 内存减少 74%/74%/24%（三种 BS/SEQ 配置），BLOOM 减少 85%/85%/6%。
- 编译时间比 TensorIR 快约一个数量级、比 AMOS 快约两个数量级，但略高于 Welder（因调度空间更大）。
- 位宽缩放实验显示：内存受限的解码场景（SEQ=1）加速随位宽下降而提升；计算受限的编码场景（SEQ=4096）加速基本不变。
- 精度-效率评估表明 WINT4/WNF4/FP8 对 PPL 影响很小且分别带来约 2.5×/1.7×/1.6× 加速；BitNet-b1.58（WINT2AINT8）在 LLAMA2-70B 上达 4.6× 加速且精度可接受。

## 7. 优点

- **抽象通用性强**：tType/tTile 统一表达元素级与分组级（MXFP）类型，并可扩展到非 2 的幂位宽（如 3-bit，按 4B 粒度存储）。
- **机制与策略分离**：四个变换原语构成清晰的机制层，调度策略可替换，为后续研究留出空间。
- **软硬解耦**：通过 tTile-device 描述硬件，同一套方法可移植到 NVIDIA/AMD，且论文指出可扩展至 Hopper FP8、Graphcore IPU 等。
- **工程落地**：开源（BitBLAS），可集成进现有 DNN/LLM 框架；对 PyTorch 内置类型无需改模型代码。
- **评估较全面**：跨 4 个 GPU 平台、多模型多类型，兼顾性能、内存、编译时间与精度-效率权衡，消融实验清晰。

## 8. 不足与局限

- **仅覆盖推理**：低精度训练（前向+反向）未实现，论文明确列为未来工作；训练场景的收益（更高吞吐计算单元、更大 batch）未验证。
- **未集成多 GPU**：BLOOM-176B、LLAMA2-70B 等需多卡部署的模型，论文仅以单层外推评估，未与多卡并行框架（vLLM、Alpa 等）联合验证。
- **调度策略为启发式**：作者自认不保证最优，变换空间仍有大量未探索部分；候选方案依赖 profile，编译时间虽优于 AMOS/TensorIR 但高于 Welder。
- **硬件描述需人工编写**：每种新硬件需手写 tTile-device 规格文件，移植成本未量化。
- **实验局限**：LLM 仅测单 decoder 层，端到端全
