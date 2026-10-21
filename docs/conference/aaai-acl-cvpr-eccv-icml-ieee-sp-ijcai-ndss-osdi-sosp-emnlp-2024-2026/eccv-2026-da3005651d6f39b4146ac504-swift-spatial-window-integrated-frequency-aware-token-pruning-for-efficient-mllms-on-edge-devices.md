---
title: "SWIFT: Spatial-Window Integrated Frequency-aware Token Pruning for Efficient MLLMs on Edge Devices"
title_zh: SWIFT：面向边缘设备高效多模态大模型的空间窗口融合频域感知令牌剪枝
authors: "Guanglai Liu, Jubo Chen, Xiaosheng Yu"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://media.eventhosts.cc/Conferences/ECCV2026/pdfs/866.pdf"
tags: ["query:coral-npu"]
score: 4.0
evidence: 通过令牌剪枝实现面向边缘设备的高效多模态大模型推理
tldr: 多模态大模型的高分辨率视觉令牌带来二次复杂度，给边缘设备部署造成巨大压力，而现有基于注意力权重的剪枝会丢弃高频细节导致特征过平滑。本文提出SWIFT，融合空间窗口与频域感知进行令牌压缩，作为即插即用框架保留关键高频信号。该工作有效降低了边缘端推理的计算开销，为高效多模态模型部署提供了新方案。
source: ECCV-2026-Accepted-Program
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/eccv-2026-accepted-program/eccv-2026-da3005651d6f39b4146ac504/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 512, \"height\": 256}, {\"url\": \"assets/figures/eccv-2026-accepted-program/eccv-2026-da3005651d6f39b4146ac504/fig-002.webp\", \"caption\": \"\", \"page\": 4, \"index\": 2, \"width\": 14604, \"height\": 8991}]"
motivation: 多模态大模型稠密视觉令牌复杂度高，边缘部署困难且现有剪枝丢失高频细节。
method: 提出SWIFT即插即用框架，结合空间窗口与频域感知信号进行视觉令牌压缩。
result: 方法在降低计算量的同时保留关键高频信号，缓解特征过平滑问题。
conclusion: 该工作为边缘设备上高效部署多模态大模型提供了有效的令牌剪枝方案。
---

## Abstract
While Multimodal Large Language Models (MLLMs) ex-cel in visual understanding, the quadratic complexity imposed by densehigh-resolution visual tokens poses significant challenges for edge-devicedeployment. Current training-free pruning strategies predominantly de-pend on attention weights. However, viewed from the frequency domain,the self-attention mechanism functions as a low-pass filter, causing thediscard of essential high-frequency signals (e.g., textures and edges) andresulting in feature over-smoothing. In this paper, we present Spatial-Window Integrated Frequency-aware Token Pruning (SWIFT), a seam-less plug-and-play framework designed for efficient token compression.SWIFT leverages two novel components: a Frequency-Aware Indicator(FAI), which identifies fine-grained details by estimating high-frequencyresiduals through matrix factorization, and a Spatial-Window Integra-tion (SWI) module, which prevents spatial structure collapse and posi-tional bias via localized retention. Extensive evaluations on Qwen2.5VL-3B and LLaVA benchmarks show that under a practical 50% compressionratio, SWIFT achieves a 2.10× speedup with near-lossless performance.Under an extreme 75% compression stress test, SWIFT yields an averageperformance drop of 5.1% across 8 benchmarks, while delivering a 2.66×speedup and 69.07% KV cache reduction. SWIFT exhibits superior ro-bustness in detail-intensive tasks compared to SOTA methods, providinga potent lightweight solution for on-device MLLM applications. Our codeis available at https://github.com/damo-lgl/SWIFT.

---

## 论文详细总结（自动生成）

# SWIFT 论文中文总结

## 1. 核心问题与整体含义（研究动机与背景）

- **背景**：多模态大模型（MLLM）依赖高分辨率图像产生大量视觉 token，自注意力复杂度随 token 数呈二次增长（O(N²)），在云端尚可接受，但对显存带宽受限的边缘设备（如 Qwen2.5VL-3B）构成严重瓶颈，表现为高延迟和首 token 时间（TTFT）过长。
- **现有方法的问题**：主流免训练剪枝方法（FastV、SparseVLM 等）基于“注意力权重=重要性”的启发式假设，但作者从信号处理频域视角指出，该假设存在根本缺陷：
  - 自注意力在深层网络中趋近于全 1 归一化矩阵（A ≈ (1/n)·11ᵀ），本质上是一个**低通滤波器**，保留低频直流分量（背景）而抑制高频交流分量（纹理、边缘），造成“特征过平滑”；
  - 软归一化导致“注意力汇”（attention sinks），大量权重被分配给低信息 token，基于注意力的剪枝进一步加剧高频信息丢失；
  - 全局非结构化剪枝破坏图像空间连续性，产生“空间盲点”，损害细粒度感知与空间分布相关任务。
- **研究含义**：论文提出一种显式的频域感知机制来对抗自注意力的低通偏置，并结合空间窗口约束，为边缘端高效 MLLM 部署提供即插即用的 token 压缩方案。

## 2. 方法论

- **核心思想**：在 LLM 预填充阶段（插入第 2 层与第 3 层之间），采用“频域—空间”双流选择机制，同时保留高频细节 token 与空间结构完整性，且无需任何训练。
- **整体流程（三步）**：
  1. 通过矩阵分解计算每个视觉 token 的高频贡献（FAI）；
  2. 将 1D token 序列重映射为 2D 空间窗口，计算融合频率与注意力线索的混合分数（SWI）；
  3. 在每个窗口内独立执行局部 Top-K 选择，输出剪枝后的序列。
- **频率感知指标（FAI）**：
  - 将注意力矩阵 A 分解为低频分量 A_LP = (1/n)·11ᵀ 与高频残差 A_HP = A − A_LP；
  - 对第 k 个视觉 token 定义高频贡献 I_freq(k) = (1/n)·Σ_q A_HP(q,k)，即高频注意力矩阵第 k 列的均值；
  - I_freq > 0 表示该 token 被“过度关注”，对应语义主体、边缘或关键纹理（高频 token）；I_freq < 0 表示低于平均关注，对应平滑背景或语义稀疏区域（低频冗余 token）；
  - 采用 Top-K 保留策略，优先保留 I_freq 最高的 r 个 token。
- **空间窗口融合选择（SWI）**：
  - 将序列恢复为 2D 网格并划分为局部窗口，在每个窗口内独立做局部 Top-K 保留；
  - 通过强制局部保留保证空间覆盖均匀性，克服全局排序带来的位置偏置和空间结构崩塌。
- **混合评分公式**：
  - Score(x) = (1−λ)·Rank(I_freq(x)) + λ·Rank(I_attn(x))；
  - 其中 I_attn 采用 LLM 第 2 层注意力矩阵（沿用 FastV 做法），λ 为平衡系数；
  - 超参实验表明 λ ≈ 0.5 时性能最优，说明语义注意力（“看哪里”）与频域显著性（“看清什么”）高度互补。
- **理论支撑**：作者通过第 31 层特征频谱分析（图 2、图 5）验证 FastV 加剧高频衰减，而 SWIFT 在中高频段（>0.25π）保持更高能量，从物理层面解释其对细节敏感任务的鲁棒性。

## 3. 实验设计

- **数据集/基准（共 8 个）**：MME、MMBench（MMB）、POPE、MMStar、Flickr30k、OK-VQA、MMMU、Nocaps，覆盖综合理解、幻觉检测、细粒度描述、知识型 VQA 和多学科推理等场景。
- **测试模型**：
  - LLaVA-1.5-7B（压缩率 50%/75%/90%，保留 288/144/58 个 token，基线 576 token）；
  - LLaVA-1.5-13B（同上压缩率，验证跨规模泛化）；
  - Qwen2.5VL-3B（压缩率 50%/66%/75%，面向边缘部署）。
- **对比方法**：
  - LLaVA-7B 上对比 Random、SparseVLM、FastV 与 Vanilla 全量基线；
  - LLaVA-13B 与 Qwen2.5VL-3B 上主要对比 FastV 与 Vanilla。
- **消融实验**：在 Qwen2.5VL-3B 上分别移除 SWI、FAI、AG（注意力引导），在 50%/66%/75% 三种压缩率下评估 MME、POPE、Nocaps、Flickr30k。
- **超参实验**：在 MME 上扫描 λ ∈ [0,1]，覆盖 LLaVA-1.5-7B、Qwen2.5-VL（840×840、1120×1120）多种架构与分辨率。
- **效率分析**：在 RTX 2080（10GB）上以 840×840 输入测量 TTFT、KV Cache 占用、解码延迟及 FAI/SWI 模块级开销。

## 4. 资源与算力

- 论文明确说明：**所有主要实验在单块 NVIDIA GeForce RTX 4090 PLUS GPU（48GB 显存）上完成**；效率分析在 **NVIDIA RTX 2080（10GB）** 上进行。
- 由于方法为**免训练（training-free）**框架，不涉及训练时长或训练算力消耗；论文未报告 GPU 集群规模、总 GPU 小时数等信息。
- 模块开销方面：FAI 仅约 0.5 ms，SWI 约 34 ms，模块总开销低于整体 TTFT 的 1.8%，额外峰值显存为 0 MB，活跃内存约 5 MB。

## 5. 实验数量与充分性

- **实验组数概览**：
  - 3 个模型 × 3 档压缩率的主实验（LLaVA-7B 表 1、LLaVA-13B 表 2、Qwen2.5VL-3B 表 3）；
  - 1 组消融实验（3 个组件 × 3 档压缩率 × 4 个数据集，表 4）；
  - 1 组 λ 超参扫描（3 种架构/分辨率设置，图 4）；
  - 1 组频谱能量对比分析（图 2、图 5）；
  - 1 组效率剖析（表 5，含 TTFT、KV Cache、解码延迟、模块开销）。
- **充分性评价**：
  - **优点**：覆盖 8 个主流 benchmark、3 个不同规模与架构的模型、多档压缩率（50%–90%），并同时报告精度与效率，实验维度较为完整；消融实验清晰分离了 SWI、FAI、AG 三个组件的贡献。
  - **不足**：对比基线偏少，主要仅与 FastV、SparseVLM 和 Random 对比，未纳入 VisionZip、MustDrop、ToMe、VScan 等论文中提及的同类方法；消融实验仅在 Qwen2.5VL-3B 上进行，未在 LLaVA 系列上验证组件普适性；效率测试仅在 RTX 2080 桌面 GPU 上完成，未在真实移动端/边缘 NPU 上验证。
  - **客观性/公平性**：对比设置基本对齐（相同 token 保留数、相同基线模型），但部分表格（如 LLaVA-13B）仅与 FastV 对比，缺少 SparseVLM 等更强基线，公平性略打折扣；另外 90% 压缩下部分指标（如 SparseVLM 的 Nocaps 50.9）异常偏低，论文未深入解释。

## 6. 主要结论与发现

- **50% 实际压缩**：SWIFT 实现约 **2.10× 推理加速**、**46.05% KV Cache 降低**，性能接近无损（如 LLaVA-13B 在 MME 上 1731 vs. Vanilla 1716，甚至反超）。
- **75% 极端压缩**（Qwen2.5VL-3B 保留 25% token）：8 个 benchmark 平均性能仅下降 **5.1%**，同时获得 **2.66× 加速**、**69.07% KV Cache 降低**、TTFT 降低 **23.0%**、单步解码延迟从 7.74 ms 降至 2.91 ms（降低 62.4%）。
- **细粒度任务优势显著**：在 Nocaps、Flickr30k 等对高频信息敏感的任务上大幅领先 FastV（如 LLaVA-7B 90% 压缩下 Nocaps +19.3%），验证 FAI 对抗特征过平滑的有效性。
- **幻觉抑制**：SWI 有效维持空间结构完整性，在 POPE 上显著优于 FastV（如 LLaVA-13B 90% 压缩下领先 7.7 分）。
- **频谱验证**：SWIFT 在中高频段保留的能量显著高于 FastV，从频域角度解释了其细节保持能力。
- **超参结论**：λ ≈ 0.5 在多种架构与分辨率下均达到性能峰值，说明语义注意力与频域显著性为 1:1 互补关系。

## 7. 优点

- **视角新颖**：首次系统地从频域（低通滤波、特征过平滑）角度剖析注意力剪枝的缺陷，理论动机清晰，并通过频谱可视化提供实证支撑。
- **方法轻量实用**：免训练、即插即用，插入位置固定（第 2–3 层之间），FAI 用矩阵分解近似替代昂贵 FFT，模块开销 <1.8% TTFT、零额外峰值显存，非常适合边缘部署。
- **双流互补设计合理**：FAI 负责“看清细节”，SWI 负责“空间均匀”，AG 负责“语义定位”，三者结合在消融实验中均有明确贡献。
- **实验覆盖较广**：跨 3 个模型（7B/13B/3B）、8 个 benchmark、多档压缩率，并同时报告精度与效率指标，结论具有较强说服力。
- **细节任务鲁棒性突出**：在 Nocaps、Flickr30k、POPE 等任务上相对 SOTA 的优势随压缩率提高而扩大，契合边缘端高压缩需求。
- **代码开源**（GitHub 链接），便于复现与后续研究。

## 8. 不足与局限

- **基线对比不完整**：主要对比 FastV 与 SparseVLM，未与 VisionZip、MustDrop、ToMe、VScan、PyramidDrop 等论文中提及的同类免训练方法进行实验比较，SOTA 声称的说服力有限。
- **消融覆盖不足**：组件消融仅在 Qwen2

5VL-3B 上进行，未在 LLaVA-7B/13B 上验证，难以判断 FAI/SWI 在不同视觉编码器（CLIP ViT vs. Qwen 原生 ViT）与不同分辨率下的普适增益；同时消融只报告了 4 个数据集，未覆盖 MMMU、OK-VQA 等推理型任务。
- **效率验证场景有限**：仅在 RTX 2080 桌面 GPU 上测量 TTFT 与解码延迟，未在 Jetson、手机 NPU、端侧 SoC 等真实边缘硬件上测试；SWI 的窗口划分与局部 Top-K 涉及序列重排，其在低带宽设备上的实际内存搬运开销可能被低估。
- **超参敏感性**：λ 虽在 0.5 附近表现稳健，但论文未给出 λ 随压缩率、层深、模型规模变化的适配策略；在 90% 极端压缩下最优 λ 是否仍为 0.5 缺乏讨论。
- **理论分析的简化**：将 A ≈ (1/n)·11ᵀ 作为低通滤波的解释基于深层注意力趋于均匀化的经验观察，但该近似在浅层（插入层为第 2–3 层）是否成立、以及 softmax 注意力与严格低通滤波之间的等价性，论文未给出严格证明，理论更多是启发性而非严格性。
- **插入位置固定**：方法固定在第 2 与第 3 层之间剪枝，未探讨插入层位置（更浅/更深）对精度—效率权衡的影响，也未讨论与多层逐步剪枝方案的结合。
- **部分异常结果未解释**：如 90% 压缩下 SparseVLM 在 Nocaps 上的异常低分，以及个别指标（如 LLaVA-13B 上 SWIFT 反超 Vanilla）的统计显著性与方差未报告，缺少多次运行的均值±方差，结论稳健性证据略弱。
- **任务覆盖偏差**：基准以图像理解为主，未涉及视频多模态、文档 OCR、医学影像等对高频细节与空间结构更为敏感的场景，方法的泛化边界尚不清晰。

## 9. 总体评价与启示

- **定位**：SWIFT 是一篇“问题洞察驱动”的免训练 token 压缩工作，其最大贡献不在于工程复杂度，而在于用频域视角重新解释了“注意力权重≠重要性”这一被广泛接受的启发式假设，并据此设计了针对性的补偿机制（FAI 补高频、SWI 保空间）。
- **实用价值**：在 50% 压缩近无损、75% 压缩仍保持 5% 以内平均性能损失的前提下实现 2.1–2.66× 加速与显著 KV Cache 节省，模块开销极低，具备较强的边缘部署落地潜力，尤其适合高分辨率输入与细粒度描述类应用。
- **方法论启示**：将信号处理中的“低通滤波/过平滑”概念引入 MLLM 视觉 token 剪枝，为后续工作提供了可迁移的分析框架——未来的剪枝方法可考虑显式建模频率响应、按频段分配 token 预算，而非仅依赖注意力或相似度排序。
- **改进空间**：若能在更多强基线（VisionZip、MustDrop、ToMe 等）上补齐对比、在真实端侧硬件上验证效率、并扩展到视频/文档等场景，其结论的说服力与影响力将进一步提升。
- **可复现性**：代码开源、方法免训练、超参少（主要为 λ 与窗口大小），复现门槛较低，有利于社区快速跟进与改进。

（完）
