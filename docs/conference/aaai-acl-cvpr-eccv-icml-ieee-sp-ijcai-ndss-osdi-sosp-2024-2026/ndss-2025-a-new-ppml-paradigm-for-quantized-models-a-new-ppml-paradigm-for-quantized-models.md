---
title: A New PPML Paradigm for Quantized Models
title_zh: 面向量化模型的新型PPML范式
authors: "Tianpei Lu, Bingsheng Zhang, Xiaoyuan Zhang, Kui Ren"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://www.ndss-symposium.org/wp-content/uploads/2025-2872-paper.pdf"
tags: ["query:litert"]
score: 4.0
evidence: 用量化算子序列建模推理并以查找表实现每个算子
tldr: 量化模型能提升效率，但在隐私保护机器学习中量化算子内部结构复杂，导致现有协议效率低下。本文提出一种面向量化模型的新PPML范式，将推理过程视为量化算子序列，并用查找表实现每个算子以简化求值。该做法可忽略任意函数的复杂内部构造，从而降低量化算子评估的协议开销，为隐私场景下高效利用量化模型提供了新思路。
source: NDSS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-a-new-ppml-paradigm-for-quantized-models/fig-001.webp\", \"caption\": \"\", \"page\": 3, \"index\": 1, \"width\": 1994, \"height\": 1211}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-a-new-ppml-paradigm-for-quantized-models/fig-002.webp\", \"caption\": \"\", \"page\": 8, \"index\": 2, \"width\": 2649, \"height\": 1927}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-a-new-ppml-paradigm-for-quantized-models/fig-003.webp\", \"caption\": \"\", \"page\": 10, \"index\": 3, \"width\": 1627, \"height\": 1522}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-a-new-ppml-paradigm-for-quantized-models/fig-004.webp\", \"caption\": \"\", \"page\": 11, \"index\": 4, \"width\": 639, \"height\": 479}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-a-new-ppml-paradigm-for-quantized-models/fig-005.webp\", \"caption\": \"\", \"page\": 11, \"index\": 5, \"width\": 639, \"height\": 479}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-a-new-ppml-paradigm-for-quantized-models/fig-006.webp\", \"caption\": \"\", \"page\": 11, \"index\": 6, \"width\": 639, \"height\": 479}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-a-new-ppml-paradigm-for-quantized-models/fig-007.webp\", \"caption\": \"\", \"page\": 11, \"index\": 7, \"width\": 639, \"height\": 479}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-a-new-ppml-paradigm-for-quantized-models/fig-008.webp\", \"caption\": \"\", \"page\": 12, \"index\": 8, \"width\": 800, \"height\": 500}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-a-new-ppml-paradigm-for-quantized-models/fig-009.webp\", \"caption\": \"\", \"page\": 12, \"index\": 9, \"width\": 695, \"height\": 417}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-a-new-ppml-paradigm-for-quantized-models/fig-010.webp\", \"caption\": \"\", \"page\": 12, \"index\": 10, \"width\": 682, \"height\": 417}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-a-new-ppml-paradigm-for-quantized-models/fig-011.webp\", \"caption\": \"\", \"page\": 17, \"index\": 11, \"width\": 800, \"height\": 500}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-a-new-ppml-paradigm-for-quantized-models/fig-012.webp\", \"caption\": \"\", \"page\": 17, \"index\": 12, \"width\": 800, \"height\": 500}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-a-new-ppml-paradigm-for-quantized-models/fig-013.webp\", \"caption\": \"\", \"page\": 17, \"index\": 13, \"width\": 800, \"height\": 500}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-a-new-ppml-paradigm-for-quantized-models/fig-014.webp\", \"caption\": \"\", \"page\": 17, \"index\": 14, \"width\": 800, \"height\": 500}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-a-new-ppml-paradigm-for-quantized-models/fig-015.webp\", \"caption\": \"\", \"page\": 18, \"index\": 15, \"width\": 2605, \"height\": 2161}]"
motivation: 量化模型在隐私保护机器学习中因量化算子内部结构复杂而导致协议效率低下。
method: 将推理建模为量化算子序列，并用查找表实现每个算子以简化求值。
result: 查找表可绕过函数内部复杂构造，降低量化算子评估的协议开销。
conclusion: 为隐私保护场景下高效利用量化模型提供了新范式。
---

## Abstract
Model quantization has become a common practice in machine learning (ML) to improve efficiency and reduce computational/communicational overhead. However, adopting quantization in privacy-preserving machine learning (PPML) remains challenging due to the complex internal structure of quantized operators, which leads to inefficient protocols under the existing PPML frameworks. In this work, we propose a new PPML paradigm that is tailor-made for and can benefit from quantized models. Our main observation is that look-up tables can ignore the complex internal constructs of any functions which can be used to simplify the quantized operator evaluation. We view the model inference process as a sequence of quantized operators, and each operator is implemented by a look-up table. We then develop an efficient private look-up table evaluation protocol, and its online communication cost is only $log n$, where $n$ is the size of the look-up table. On a single CPU core, our protocol can evaluate $2^{26}$ tables with 8-bit input and 8-bit output per second. The resulting PPML framework for quantized models offers extremely fast online performance. The experimental results demonstrate that our quantization strategy achieves substantial speedups over SOTA PPML solutions, improving the online performance by $40sim 60 times$ w.r.t. convolutional neural network (CNN) models, such as AlexNet, VGG16, and ResNet18, and by $10sim 25 times$ w.r.t. large language models (LLMs), such as GPT-2, GPT-Neo, and Llama2. Model quantization has become a common practice in machine learning (ML) to improve efficiency and reduce computational/communicational overhead. However, adopting quantization in privacy-preserving machine learning (PPML) remains challenging due to the complex internal structure of quantized operators, which leads to inefficient protocols under the existing PPML frameworks. In this work, we propose a new PPML paradigm that is tailor-made for and can benefit from quantized models. Our main observation is that look-up tables can ignore the complex internal constructs of any functions which can be used to simplify the quantized operator evaluation. We view the model inference process as a sequence of quantized operators, and each operator is implemented by a look-up table. We then develop an efficient private look-up table evaluation protocol, and its online communication cost is only $log n$, where $n$ is the size of the look-up table. On a single CPU core, our protocol can evaluate $2^{26}$ tables with 8-bit input and 8-bit output per second. The resulting PPML framework for quantized models offers extremely fast online performance. The experimental results demonstrate that our quantization strategy achieves substantial speedups over SOTA PPML solutions, improving the online performance by $40sim 60 times$ w.r.t. convolutional neural network (CNN) models, such as AlexNet, VGG16, and ResNet18, and by $10sim 25 times$ w.r.t. large language models (LLMs), such as GPT-2, GPT-Neo, and Llama2.

---

## 论文详细总结（自动生成）

# 论文总结：A New PPML Paradigm for Quantized Models

## 1. 核心问题与整体含义

- **研究背景**：模型量化在普通机器学习中已广泛用于压缩模型、降低内存带宽和计算/通信开销，尤其适合 4-bit/8-bit 低精度推理。
- **核心问题**：将量化直接引入隐私保护机器学习（PPML）并不高效。量化算子内部通常包含反量化/再量化步骤，涉及私密缩放因子 \(s\)、偏移 \(b\) 的高精度安全乘法以及模切换/显著位提取等昂贵操作，导致基于 MPC 的量化 PPML 反而可能比未量化版本通信更多。
- **现有方案局限**：
  - 若将量化参数设为公开且限制为 2 的幂，可避免安全乘法，但会泄漏模型信息并损害模型精度。
  - 现有查找表方案多假设表公开，或依赖 FSS/混淆电路/私有函数评估，通信与计算开销仍较高。
- **论文主张**：提出一种**面向量化模型定制、并能从量化中获益的 PPML 新范式**：把模型推理视为量化算子序列，每个算子用查找表实现；模型服务器将权重和量化参数嵌入私有查找表，在线只进行高效的私有查表。

## 2. 方法论

### 2.1 核心思想

- **用查找表隐藏复杂算子内部结构**：对输入/输出均为低精度整数的量化算子，查找表可忽略其内部反量化、缩放、偏移等复杂构造。
- **模型服务器预处理生成表**：服务器持有明文模型，可在离线阶段对每个量化算子遍历所有可能输入，生成表项 \(T(r)=op^*(r)\)，并将权重 \(w'\)、缩放因子 \(s_i\)、偏移 \(b_i\) 硬编码进表。
- **在线私有查表**：客户端持有私有输入的秘密分享 \(\langle x'\rangle\)，双方通过私有查找表评估协议得到 \(\langle y'\rangle=\langle T(x')\rangle\)，无需暴露索引和表内容。
- **算子融合**：多个 SISO 查找表可融合为单个表；SISO 与 MISO 连接也可融合；反量化可融合进最后一层查找表，降低在线开销。

### 2.2 量化与查表流程

- 量化方案形式化为 \(Q=(G,E,F,D)\)：生成量化参数、编码、量化函数执行、解码。
- 典型编码：\(x'=\lfloor x/s\rfloor-b\)，解码：\(y=s(y'+b)\)。
- 对乘法 \(y=x\cdot w\)，可重写为仅依赖量化输入 \(x'\) 的函数：
  \[
  y'=f(x')=\frac{s_1s_0}{s_2}(w'-b_1)(x'-b_0)-b_2
  \]
  服务器已知 \(w',s_i,b_i\)，故可预生成 256 项表（8-bit 输入）。
- 输入量化阶段使用 VOLE 与 `cut` 协议：通过随机掩码 \(r\)、VOLE 三元组和显著位截取，将高精度秘密分享 \(\langle x\rangle\) 转为低精度 \(\langle x'\rangle\)。
- 输出阶段将反量化与最后一层算子融合，生成输入 \(2^{\ell'}\)、输出高精度 \(\ell\) 的表，减少额外通信。

### 2.3 私有查找表评估协议

- 目标：服务器私有输入表 \(T^{\ell_x,\ell_y}\)，双方输入共享索引 \(\langle x\rangle\)，输出共享查表结果 \(\langle T(x)\rangle\)。
- 采用“移位查找表对”思路：预处理生成 \((\langle T'\rangle,\langle r\rangle)\)，满足 \(T'(x)=T(x+r)\)；在线重构 \(\delta=x-r\)，再取 \(T'(\delta)=T(x)\)。
- 关键技术是**环上向量不经意移位评估（VOSE）**：扩展 Lu 等人的二进制 VOSE 到 \(\mathbb{Z}_{2^\ell}\)，基于 \((2^{\ell_x}-1,2^{\ell_x})\)-ROT 构造随机 VOSE，再生成私有循环移位查找表对。
- 在线通信开销主要与输入位宽/表大小对数相关；论文称在线通信仅为 \(\log n\)（8-bit 输入表约 8 bit 量级），单 CPU 核每秒可评估 \(2^{26}\) 个 8-bit 输入/8-bit 输出表。
- 安全性：在半诚实模型下基于 UC 框架证明，假设参与方不共谋。

### 2.4 系统设置与 MISO 处理

- 支持两种部署：
  - **外包设置**：数据客户端 C、模型服务器 S、两个计算节点 \(P_0,P_1\)，假设计算节点不共谋。
  - **C/S 设置**：客户端与服务器直接两方计算，使用 VOLE 和私有查表协议。
- **MISO 算子**：
  - 卷积/全连接/矩阵乘视为“同一索引查询多个表”，将同一输入对应的多个权重输出拼接成一个多输出表，在线成本约等于一次查表。
  - 加法可拆为两个单输入表后本地相加。
  - 双输入乘法通过让上一层输出 \(\ell'/2\) 位并拼接为 \(\ell'\) 位，转为 SISO 查表，避免中间乘积溢出。
  - Max pooling 需逐层比较，较昂贵；average pooling 可本地求和加除法表。
  - Softmax、LayerNorm、Gather/Word2Vec 等给出量化查表实现。

## 3. 实验设计

- **模型与场景**：
  - CNN：LeNet、AlexNet、VGG16、ResNet18、SqueezeNet、ResNet50。
  - LLM：GPT-2-Base、GPT-Neo-1.3B、GPT-Neo-2.7B、Llama2-7B、Llama2-13B。
- **数据集**：
  - CIFAR-10、Tiny ImageNet、ImageNet；LLM 使用 Hugging Face 的 8-bit 量化模型，输入序列长度 128、输出 1 token。
- **Benchmark 与对比方法**：
  - 私表评估：对比基于 FSS 的私有查表、混淆电路查表、公开表编译为电路等方案。
  - CNN：对比 CryptoFlow2、Falcon、Bicoptor、Piranha-Falcon 等。
  - LLM：对比 CrypTen、Sigma。
  - 精度：对比原始 fp32、16-bit、8-bit、4-bit 量化；CNN 8-bit 精度用 TensorRT 测试。
- **网络与设备**：
  - LAN/WAN 两种网络；不同带宽/延迟设置。
  - 设备包括单核 CPU、96 线程 CPU、4×RTX 2080Ti GPU 服务器、Raspberry Pi 4B。
- **测量内容**：
  - 在线/离线运行时间、通信量、不同量化位宽、不同设备、CNN/LLM 端到端推理、精度损失。

## 4. 资源与算力

- 文中明确给出的硬件：
  - CPU 服务器：Intel Xeon Silver 4214 @2.20GHz，48 CPUs，128GB 内存，Ubuntu 18.04.2。
  - GPU 服务器：在上述 CPU 服务器基础上增加 **4×Nvidia RTX 2080Ti，8GB**。
  - 边缘设备：Raspberry Pi 4B，1.5GHz CPU，4GB DDR4。
  - 安全参数 \(\lambda=128\)，OT 使用 libOTe。
- **未明确说明**：
  - 论文主要做推理基准，未报告模型训练/量化校准所需算力、训练时长、能耗。
  - 未给出完整实验总 GPU 小时数或 CPU 核时。
  - 部分基线因代码不可得，采用论文数据或模拟其运行环境，实际算力对齐程度有限。

## 5. 实验数量与充分性

- **实验组数概览**：
  - 私表评估：4-bit/8-bit，LAN/WAN，对比 FSS、混淆电路等。
  - CNN：AlexNet、VGG16、SqueezeNet、ResNet50 等，对比 CryptoFlow2、Falcon、Bicoptor。
  - LLM：GPT-2、GPT-Neo、Llama2 多尺寸，对比 CrypTen、Sigma。
  - 设备：Raspberry Pi、GPU、96 线程 CPU。
  - 量化位宽：4/8/16-bit 的性能与精度。
  - 离线阶段：不同表元素规模、LAN/WAN。
  - 精度：多个 CNN 模型和 Transformer 模型。
- **充分性评价**：
  - 覆盖面较广，涵盖 CNN/LLM、LAN/WAN、在线/离线、不同设备、不同量化位宽，实验数量较充分。
  - 对比了多个 SOTA PPML 框架，并报告在线加速比和通信量。
- **客观性与公平性风险**：
  - 部分基线代码不可得，使用论文数据或模拟环境；硬件、线程数、GPU/CPU 设置不完全一致。
  - 论文强调自身单核 CPU 已优于部分 GPU 方案，但不同框架的优化程度、实现成熟度、离线成本分摊方式可能影响可比性。
  - 缺少与所有基线在完全相同硬件和网络下的统一复现实验。

## 6. 主要结论与发现

- 查找表范式可有效绕开量化算子内部复杂结构，使 PPML 真正从量化中获益。
- 私有查表协议在线通信低、在线速度快；8-bit 输入/输出表在单核 CPU 上可达约 \(2^{26}\) 表/秒。
- CNN 上，8-bit 量化 PPML 在线性能相比 CryptoFlow2 提升约 **40–60×**；相比 Falcon/Bicoptor 提升约 **5–15×**。
- LLM 上，相比 CrypTen/Sigma 提升约 **10–25×**。
- 离线阶段较重，但即使计入离线成本，部分场景仍优于 CryptoFlow2。
- 8-bit 量化在 CNN 上精度损失较小（约 0.2%–1%）；Transformer 的 8-bit 精度损失较明显，建议对 LayerNorm 使用 16-bit 或混合量化。
- 8-bit 是效率与精度平衡的较优选择；4-bit 更快但 CNN 精度下降更明显，16-bit 更准但开销更高。

## 7. 优点

- **范式创新**：将量化 PPML 从“在 MPC 中模拟量化算子”转为“私有查表”，思路简洁且与量化低精度 IO 特性高度契合。
- **隐私性更好**：权重和量化参数嵌入私有表，避免公开缩放因子导致的模型泄漏。
- **在线效率突出**：在线通信主要依赖输入位宽/表大小对数，适合低精度量化场景。
- **支持算子融合**：SISO/SISO、SISO/MISO、反量化融合等显著减少查表次数。
- **系统实现完整**：覆盖 C/S 与外包两种部署，给出 UC 半诚实安全证明，并在 CPU、GPU、Raspberry Pi 多平台基准。
- **实验覆盖较广**：CNN 与 LLM、LAN/WAN、4/8/16-bit、在线/离线、精度与性能均有涉及。

## 8. 不足与局限

- **安全模型有限**：仅考虑半诚实敌手和静态腐化；外包设置依赖计算节点不共谋，未覆盖恶意安全、合谋或侧信道。
- **离线成本与存储开销大**：需为每个算子生成查找表，表数量和大小随模型规模增长；MISO-MISO 融合会导致表大小指数增长，因此融合受限。
- **部分算子仍昂贵**：Max pooling 需多层比较；双输入乘法转 SISO 会牺牲位宽/精度；矩阵乘需较高精度输出表。
- **量化依赖校准数据**：量化参数由校准集生成，校准数据分布偏移可能影响精度；论文未系统评估校准集敏感性。
- **Transformer 8-bit 精度风险**：8-bit 对 GPT 类模型精度损失较明显，需 16-bit LayerNorm 或混合量化，削弱了低精度带来的全部收益。
- **实验公平性风险**：部分基线使用论文数据/模拟环境，硬件与实现优化不一致；缺少统一复现和端到端真实部署评估。
- **资源报告不完整**：未报告训练/校准算力、能耗、完整离线预处理时间分布；GPU 加速收益有限，主要依赖多线程 CPU。
- **应用限制**：更适合低精度、表较小的算子和模型；对超大词表 Gather、超大矩阵乘、复杂动态控制流可能不经济。

（完）
