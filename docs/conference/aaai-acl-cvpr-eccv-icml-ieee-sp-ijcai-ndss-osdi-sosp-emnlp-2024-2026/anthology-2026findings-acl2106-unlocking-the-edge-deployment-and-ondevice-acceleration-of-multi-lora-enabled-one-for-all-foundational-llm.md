---
title: Unlocking the Edge deployment and ondevice acceleration of multi-LoRA enabled one-for-all foundational LLM
title_zh: 释放多LoRA一体化基础大模型的边缘部署与端侧加速
authors: "Sravanth Kodavanti, Sowmya Vajrala, Srinivas Soumitri Miriyala, Utsav Tiwari, Uttam Kumar, Utkarsh Kumar Mahawar, Achal Pratap Singh, Arya D, Narendra Mutyala, Vikram Nelvoy Rajendiran, Sharan Kumar Allur, Euntaik Lee, Dohyoung Kim, HyeonSu Lee, Gyusung Cho, JungBae Kim"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://aclanthology.org/2026.findings-acl.2106.pdf"
tags: ["query:coral-npu"]
score: 5.0
evidence: 面向智能手机芯片的低延迟端侧推理硬件感知框架
tldr: 针对在智能手机上部署大模型受内存、延迟与运行时灵活性严格约束的问题，本文提出一种硬件感知的端侧推理框架，将多任务LoRA作为运行时输入注入单一冻结推理图，并设计多流解码机制。实验在三星Galaxy S24与S25的高通SM8650与SM8750芯片上验证了无需重编译的动态任务切换与高效并发生成。该工作为资源受限移动设备上部署多用途基础模型提供了可行路径，与低延迟端侧推理加速目标一致。
source: ACL-2026-Findings
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl2106/fig-001.webp\", \"caption\": \"\", \"page\": 3, \"index\": 1, \"width\": 1692, \"height\": 547}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl2106/fig-002.webp\", \"caption\": \"\", \"page\": 4, \"index\": 2, \"width\": 746, \"height\": 414}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl2106/fig-003.webp\", \"caption\": \"\", \"page\": 6, \"index\": 3, \"width\": 838, \"height\": 428}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl2106/fig-004.webp\", \"caption\": \"\", \"page\": 10, \"index\": 4, \"width\": 1565, \"height\": 406}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl2106/fig-005.webp\", \"caption\": \"\", \"page\": 10, \"index\": 5, \"width\": 989, \"height\": 401}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl2106/fig-006.webp\", \"caption\": \"\", \"page\": 11, \"index\": 6, \"width\": 991, \"height\": 410}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl2106/fig-007.webp\", \"caption\": \"\", \"page\": 11, \"index\": 7, \"width\": 963, \"height\": 502}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl2106/fig-008.webp\", \"caption\": \"\", \"page\": 12, \"index\": 8, \"width\": 950, \"height\": 241}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl2106/fig-009.webp\", \"caption\": \"\", \"page\": 12, \"index\": 9, \"width\": 727, \"height\": 214}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl2106/fig-010.webp\", \"caption\": \"\", \"page\": 12, \"index\": 10, \"width\": 949, \"height\": 521}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl2106/fig-011.webp\", \"caption\": \"\", \"page\": 13, \"index\": 11, \"width\": 735, \"height\": 513}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl2106/fig-012.webp\", \"caption\": \"\", \"page\": 13, \"index\": 12, \"width\": 730, \"height\": 464}]"
motivation: 在智能手机上部署大语言模型面临内存、延迟与运行时灵活性的严格约束。
method: 提出硬件感知框架，将应用特定LoRA作为运行时输入接入单一冻结推理图，并引入多流解码机制。
result: 在三星Galaxy S24与S25的高通芯片上实现无重编译的动态任务切换与高效端侧推理。
conclusion: 为资源受限移动设备上的多用途基础模型部署提供了低延迟、低内存开销的解决方案。
---

## Abstract
Deploying large language models (LLMs) on smartphones poses significant engineering challenges due to stringent constraints on memory, latency, and runtime flexibility. In this work, we present a hardware-aware framework for efficient on-device inference of a LLaMA-based multilingual foundation model supporting multiple use cases on Samsung Galaxy S24 and S25 devices with SM8650 and SM8750 Qualcomm chipsets respectively. Our approach integrates application-specific LoRAs as runtime inputs to a single frozen inference graph, enabling dynamic task switching without recompilation or memory overhead. We further introduce a multi-stream decoding mechanism that concurrently generates stylistic variations—such as formal, polite, or jovial responses—within a single forward pass, reducing latency by up to 6×. To accelerate token generation, we apply Dynamic Self-Speculative Decoding (DS2D), a tree-based strategy that predicts future tokens without requiring a draft model, yielding up to 2.3× speedup in decode time. Combined with quantization to INT4 and architecture-level optimizations, our system achieves 4–6× overall improvements in memory and latency while maintaining accuracy across 9 languages and 8 tasks. These results demonstrate practical feasibility of deploying multi-use-case LLMs on edge devices, advancing the commercial viability of Generative AI in mobile platforms.

---

## 论文详细总结（自动生成）

# 论文结构化总结

**论文**：*Unlocking the Edge Deployment and On-Device Acceleration of Multi-LoRA Enabled One-for-All Foundational LLM*（ACL 2026 Findings，三星研究院）

---

## 1. 核心问题与研究背景

- **端侧部署 LLM 的三重约束**：内存（ROM/RAM）、延迟（首 token 与解码吞吐）、运行时灵活性。手机端是**冻结的推理图**，不能像服务器那样重编译、重新量化或动态加载权重。
- **核心矛盾**：服务端可用 PEFT（如 LoRA）灵活适配多任务，但端侧要把“多任务、多语言、多风格”塞进一个模型，同时不能成倍增加内存与图数量。
- **工程目标**：在三星 Galaxy S24（SM8650）与 S25（SM8750）的高通 NPU 上，用**单个 LLaMA 系基础模型 + 8 个应用级 LoRA**，覆盖 9 种语言、8 类任务（纠错、风格/语气改写、智能回复、摘要、写作助手、健康能量分等），实现无云、全端侧实时推理。
- **整体含义**：这是一篇工业系统型论文，价值在“可商用落地”，而非提出新的模型结构或训练范式；其方法论对端侧 AI 的图编译、量化与解码加速有普适参考价值。

---

## 2. 方法论

### 2.1 多 LoRA 端侧启用（三种方案，最终选“LoRA 即输入”）
- **方案 A（多图共享权重）**：为每个 LoRA 任务建独立图，非 LoRA 层权重共享；省内存但图数量多（示例为 24 个图）。
- **方案 B（单图 + one-hot 掩码）**：单图内置全部 LoRA，运行时用 one-hot mask 选择；切换快但常驻内存高。
- **方案 C（LoRA-as-Input，本文采用）**：基础图只留 LoRA 权重**占位符**，LoRA 权重与 token 一同作为运行时输入注入。兼顾延迟与内存，实现“即插即用、免重编译、免重新量化”。**前提假设：所有 LoRA 维度一致**（同 rank、同形状）。
- LoRA 施加于 Q/K/V/O 投影：`Y = XW + X(sAB)`，`s` 为缩放因子。

### 2.2 面向 NPU 的架构与编译优化
- **注意力并行化**：将多头注意力（MHA）拆成多个独立单头注意力（SHA），便于映射到并行核心、做 head-wise 调度。
- **线性层 → 1×1 卷积重参数化**：数学等价，但可复用移动 GPU/NPU 上高度优化的卷积 kernel（Winograd、tiling、流水线），降低延迟与能耗。
- **常量折叠 + 算子融合**：预计算 LayerNorm 的静态缩放、静态 reshape/permute；融合相邻线性层与激活，减少 kernel 启动开销。
- **混合精度量化（QAT）**：激活 per-tensor INT8、权重 per-channel INT4；LoRA 权重保持 INT16。

### 2.3 并发 token 生成（CTG）
- 观察：风格变体（礼貌/正式/活泼…）主要由**第一个 token** 决定，且所有用例共享同一冻结图与内存布局。
- 流程：一次 Prefill → 采样器产出 8 个不同首 token → 8 路生成流并行推进；KV-Cache 切分为“公共 prefill 段 + 8 个独立句段”，用掩码保证每路只关注自身上下文。
- 效果：把“串行 8 次解码”变为“1 次 prefill + 8 路并行”，论文称最高 6× 延迟/内存收益。

### 2.4 动态自推测解码（DS2D）
- 借鉴 BiTA：用 **Prefix Tuning** 在 prompt 嵌入后追加 `m` 个 forecast embedding，使模型一次预测 `1+m` 个 token（半自回归 SAR），**无需 draft 模型、无需重训主干**。
- 首 token 仍从冻结模型原始分布采样（保证无损），后续 `m` 个为低保真草稿，需在下一步验证。
- **树形分支**：如 (3,2) 配置表示从第 1 个 forecast 位置取 top-3、第 2 个位置取 top-2，构造 9 个草稿 token 树；下一步同时做“验证 + 新草稿生成”。
- **分支配置动态可选**：因 NPU 对 2 的幂尺寸最友好，把输入行数对齐到 32，在可行配置中按微调学到的接受率选最优；通过掩码在推理期实现。
- 因果掩码需改造，避免 forecast prefix 与 prompt token 相互干扰。

---

## 3. 实验设计

- **硬件/场景**：
  - 1B LLaMA：Galaxy S24（SM8650 NPU），36 个用例 / 4 类任务（风格语气、拼写语法纠错、智能回复、健康摘要）× 9 语言（韩、英、德、西、法、意、荷、中、日），4 个 LoRA。
  - 3B LLaMA：Galaxy S25 Ultra（SM8750），8 个商用用例（Correction、Composer、Style、Health、Summarization、Natural Language、ST Energy、AI Brief），启用 DS2D。
- **评测指标**：Prefill 延迟、解码时间、tokens/s、峰值内存
