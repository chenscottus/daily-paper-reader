---
title: "NanoVSR: Towards Real-Time Video Super-Resolution on Edge Devices"
title_zh: NanoVSR：面向边缘设备的实时视频超分
authors: "Filip Pawlicki, Marcel Kańduła, Marcin Pucek, Kamil Dobies"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://media.eventhosts.cc/Conferences/ECCV2026/pdfs/15115.pdf"
tags: ["query:coral-npu"]
score: 5.0
evidence: 可折叠为标准卷积、便于边缘硬件加速器部署的实时视频超分架构
tldr: 现有视频超分方法依赖Transformer与显式光流，带来额外计算与自定义算子，难以部署到TensorRT等硬件加速器。作者提出NanoVSR，一种面向资源受限边缘设备的全卷积可扩展架构，借助结构重参数化在推理时折叠为标准卷积，从而保证硬件兼容性并几乎不增加运行时开销。在REDS4基准上取得有竞争力的复原质量，为边缘端低延迟实时推理部署提供了实用路径。
source: ECCV-2026-Accepted-Program
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/eccv-2026-accepted-program/eccv-2026-1cd694904909dbecdb1da038/fig-001.webp\", \"caption\": \"\", \"page\": 5, \"index\": 1, \"width\": 1280, \"height\": 720}, {\"url\": \"assets/figures/eccv-2026-accepted-program/eccv-2026-1cd694904909dbecdb1da038/fig-002.webp\", \"caption\": \"\", \"page\": 5, \"index\": 2, \"width\": 1280, \"height\": 720}, {\"url\": \"assets/figures/eccv-2026-accepted-program/eccv-2026-1cd694904909dbecdb1da038/fig-003.webp\", \"caption\": \"\", \"page\": 5, \"index\": 3, \"width\": 1280, \"height\": 720}, {\"url\": \"assets/figures/eccv-2026-accepted-program/eccv-2026-1cd694904909dbecdb1da038/fig-004.webp\", \"caption\": \"\", \"page\": 10, \"index\": 4, \"width\": 780, \"height\": 720}, {\"url\": \"assets/figures/eccv-2026-accepted-program/eccv-2026-1cd694904909dbecdb1da038/fig-005.webp\", \"caption\": \"\", \"page\": 10, \"index\": 5, \"width\": 625, \"height\": 576}, {\"url\": \"assets/figures/eccv-2026-accepted-program/eccv-2026-1cd694904909dbecdb1da038/fig-006.webp\", \"caption\": \"\", \"page\": 10, \"index\": 6, \"width\": 430, \"height\": 480}]"
motivation: 现有视频超分依赖Transformer与显式光流，计算开销大且含自定义算子，阻碍在边缘硬件加速器上的部署。
method: 提出全卷积可扩展架构NanoVSR，用结构重参数化使推理阶段折叠为标准卷积，并通过渐进训练隐式学习时空对齐。
result: 在REDS4基准上以极小运行时开销获得有竞争力的复原质量，并具备良好的硬件兼容性。
conclusion: 表明通过架构重参数化设计可兼顾复原质量与边缘加速器部署可行性。
---

## Abstract
Recent Video Super-Resolution (VSR) methods rely heavilyon transformers and explicit optical flow, creating computational over-head and custom operations that hinder deployment on hardware acceler-ators like TensorRT. To address this, we introduce NanoVSR, a scalable,fully convolutional architecture designed for resource-constrained edgedevices. Using structural reparameterization, NanoVSR collapses intostandard convolutions during inference, ensuring seamless hardware com-patibility and negligible runtime overhead. Furthermore, despite lackingexplicit motion compensation, it maintains competitive restoration qual-ity by implicitly learning spatio-temporal alignments through progres-sive training. Evaluated on the REDS4 benchmark, NanoVSR demon-strates an exceptional balance between accuracy and computational ef-ficiency, significantly improving the trade-off for compact architectures.Our NanoVSR-644k baseline yields 28.64 dB PSNR on REDS4 whiledelivering 27.20 FPS on the NVIDIA Jetson Orin NX 16GB (25W),offering massive speed gains over heavier models. The scaled NanoVSR-1.7M variant reaches 29.15 dB with a throughput of 19.58 FPS, providingsuperior, edge-optimized upscaling.

---

## 论文详细总结（自动生成）

## 一、核心问题与研究背景

- **问题定位**：当前主流视频超分（VSR）方法大量依赖 Transformer 与显式光流/可变形卷积，带来高昂计算开销和大量自定义算子，难以在 TensorRT 等硬件加速器上高效部署。
- **边缘场景矛盾**：高端 GPU 尚可承受，但边缘设备（如 NVIDIA Jetson）的内存、功耗与算力有限，执行开销一旦超过实时阈值，方法便失去实用价值。
- **研究目标**：设计一种可扩展、纯卷积、对边缘硬件友好的 VSR 架构，在复原质量与推理吞吐之间取得更优折中，并保证 ONNX/TensorRT 原生兼容。

## 二、方法论

- **整体架构**：全卷积双向循环网络，流程为「浅层特征提取 → 双向时序传播 → 融合与重建」，完全避开光流、可变形卷积与注意力机制。
- **结构重参数化（核心）**：
  - 训练时每个基础块采用多分支拓扑：3×3 卷积 + 1×1 卷积 + 恒等映射，各接 BatchNorm。
  - 推理前将 BN 统计量吸收进卷积核，并把多分支零填充、空间对齐后线性融合为单个 3×3 卷积。
  - 效果：训练保持强表达能力，推理变为连续标准卷积流，消除内存碎片与 kernel 启动开销。
- **直接加性传播**：
  - 隐藏状态更新采用逐元素相加而非通道拼接：`h_i = H(f_i + h_{i-1})`，前向/后向对称。
  - 避免通道翻倍导致的计算与带宽膨胀，迫使网络隐式学习残差运动表示。
- **融合与重建**：
  - 前向、后向隐藏状态通道拼接后经 1×1 卷积降维融合。
  - 两级 PixelShuffle 做 4× 上采样，PReLU 激活，末层 3×3 卷积输出 RGB。
  - 全局残差：以双线性上采样的 LR 输入作为基底相加，网络只学高频残差。
- **两阶段渐进训练**：
  - 阶段一：Vimeo-90K 7 帧短序列预训练 5 万次迭代，稳定空间特征提取。
  - 阶段二：REDS 30 帧长序列微调 10 万次迭代，学习长程循环与隐式对齐。
- **优化细节**：Charbonnier 损失、Adam、Cosine Annealing（3e-4→1e-7）、AMP BFloat16、梯度裁剪 L2=0.5、CutBlur 等数据增强。

## 三、实验设计

- **数据集/基准**：
  - 训练：Vimeo-90K（阶段一）、REDS（阶段二），REDS4 的四个 clip 严格排除。
  - 测试：REDS4（RGB 通道）、Vid4（Y 通道）、Vimeo-90K-T（Y 通道），统一 4× 上采样。
- **对比方法**：
  - 重型/主流基线：EDVR、EDVR-M、BasicVSR、IconVSR、RVRT。
  - 高效基线：SPAN（逐帧 SISR）、MobileRNN（Mobile AI 2022 基线）。
  - 自身缩放变体：22k、31k、48k、226k、644k、1.7M、5.4M、9.6M。
- **评估指标**：PSNR/SSIM（保真）、LPIPS/VMAF（感知）、ST-RRED（时序一致性）。
- **硬件基准**：
  - 质量与延迟：NVIDIA H100，FP32，180×320 输入。
  - 边缘部署：Jetson Orin NX 8GB/15W 与 16GB/25W，TensorRT 10.3、FP16、T=15 帧窗口。

## 四、资源与算力

- **训练侧**：仅说明使用 Tricity Academic Supercomputer & Network（CI TASK）的算力，**未明确给出 GPU 型号、数量与总训练时长**；可确认的训练配置为 15 万次迭代、全局 batch size 12、256×256 patch。
- **推理/评测侧**：明确使用单张 NVIDIA H100（FP32）做质量与延迟基准；边缘端使用 Jetson Orin NX 8GB/15W 与 16GB/25W 两种配置，FP16 + TensorRT。
- **结论**：算力信息在推理侧较清晰，但训练侧资源披露不充分。

## 五、实验数量与充分性

- **实验规模**：
  - 主表 1 组：3 数据集 × 9 个基线 + 4 个自研变体，含参数量与运行时。
  - 感知/时序表 1 组：Vid4/REDS4 上的 LPIPS、VMAF、ST-RRED。
  - 边缘效率表 1 组：2 设备 × 2 分辨率 × 5 模型。
  - 消融表 1 组：NOFUSE、SINGLE、NOPRET、SPYNET、ONEWAY 共 5 个变体。
  - 模型缩放分析：22k 至 9.6M 共 8 个规模点。
  - 定性对比 2 组（REDS4 clip 015、Vid4 Calendar/Walk）。
- **充分性评价**：
  - 覆盖多数据集、多指标、多硬件、多规模，消融较系统地验证了重参数化、课程训练、显式对齐、双向窗口四个关键设计。
  - 消融仅在 226k 小模型上完成，作者声称结论可迁移至 644k，但未直接验证。
  - SWRN 因复现 PSNR 明显低于原文而被省略；MobileRNN 因官方权重覆盖 REDS4 而缺失该数据集结果，比较覆盖存在缺口。
- **公平性**：
  - 所有基线在相同硬件上重新测延迟，指标计算协议统一，整体较客观。
  - 但 LPIPS/VMAF/ST-RRED 及 SPAN、MobileRNN 结果由作者自行计算，存在实现差异风险。

## 六、主要结论与发现

- **精度-效率折中显著改善**：
  - NanoVSR-644k：REDS4 28.64 dB，Jetson Orin NX 16GB 上 27.20 FPS（180×320）。
  - NanoVSR-226k：43.86 FPS（180×320）、19.55 FPS（270×480），适合严格流式约束。
  - NanoVSR-1.7M：REDS4 29.15 dB，H100 上 4.268 ms/帧。
- **相对重型模型的速度优势**：RVRT 需 47.867 ms/帧，BasicVSR 在 Orin NX 16GB 仅 8.04 FPS，NanoVSR 实现数量级加速。
- **重参数化收益**：融合后参数量 245k→226k，推理时间 3.471 ms→1.910 ms，几乎减半，且精度数学等价。
- **显式对齐代价高**：加入 SPyNet 带来 +1.21 dB（REDS4），但参数膨胀至 1.7M、延迟翻倍至 3.953 ms，边缘场景不划算。
- **双向窗口价值**：相比单向，Vid4 提升 0.26 dB，但引入 15 帧缓冲延迟。
- **缩放饱和**：5.4M→9.6M 仅 +0.17 dB，644k 为实用平衡点。
- **隐式对齐可行性**：无显式运动补偿仍能保持时序一致性，ST-RRED 随容量单调改善且稳定。

## 七、优点

- **部署友好性突出**：纯卷积、无自定义 CUDA 算子，ONNX/TensorRT 原生兼容，工程落地价值高。
- **训练-推理解耦设计巧妙**：利用结构重参数化在训练期保留多分支表达能力，推理期塌缩为单卷积流，兼顾质量与速度。
- **加性传播降低带宽**：以逐元素相加替代通道拼接，直接缓解边缘设备的内存带宽瓶颈。
- **渐进训练策略合理**：短序列预训练 + 长序列微调，缓解隐式对齐网络在长序列上的训练不稳定性。
- **评测维度完整**：同时报告保真、感知、时序三类指标，并在真实边缘硬件上实测吞吐。
- **消融设计有针对性**：逐一量化重参数化、课程训练、显式光流、双向窗口的收益与代价。
- **学术诚实度较好**：主动说明 SWRN 复现偏差、MobileRNN 数据缺失、双向缓冲延迟等限制。

## 八、不足与局限

- **质量上限受限**：与 RVRT 等 SOTA 仍有约 3–4 dB PSNR 差距；极端非刚性局部运动下高频纹理重建能力不足。
- **双向传播引入延迟**：T=15 的 look-ahead 缓冲不适合严格逐帧实时场景，降级为单向会损失约 0.26 dB。
- **分辨率覆盖有限**：仅在 180×320 与 270×480 上评测，更高输入分辨率未验证。
- **训练算力披露不足**：未说明 GPU 型号、数量与总训练时长，复现成本不透明。
- **消融代表性有限**：仅在 226k 模型上做消融，向 644k 的迁移性为经验性声明。
- **基线覆盖缺口**：SWRN 被省略、MobileRNN 缺 REDS4 结果，横向比较不够完整。
- **自算指标风险**：LPIPS/VMAF/ST-RRED 及部分基线结果由作者自行计算，可能与原论文口径存在偏差。
- **应用伦理风险**：超分合成的是合理细节而非真实信号，存在被用于非自愿监控的 dual-use 风险，作者仅作原则性呼吁。

（完）
