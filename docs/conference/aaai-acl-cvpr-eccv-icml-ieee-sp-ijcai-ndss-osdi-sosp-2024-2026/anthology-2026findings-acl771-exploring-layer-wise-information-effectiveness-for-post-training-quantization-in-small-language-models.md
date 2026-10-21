---
title: Exploring Layer-wise Information Effectiveness for Post-Training Quantization in Small Language Models
title_zh: 探索小型语言模型训练后量化的逐层信息有效性
authors: "He Xiao, Qingyao Yang, Dirui Xie, Wendong XU, Zunhai Su, Runming Yang, Haobo Liu, Wenyong Zhou, Zhengwu Liu, Ngai Wong"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://aclanthology.org/2026.findings-acl.771.pdf"
tags: ["query:litert-ops"]
score: 4.0
evidence: 保持标准乘法内核的硬件友好训练后量化
tldr: 针对小型语言模型在极低比特压缩下难以保持精度的挑战，本文提出LieQ逐层信息有效性量化框架。方法依据信息有效性指标在层间混合精度、层内保持统一比特宽度，从而保留标准乘法内核并避免不规则内存访问。实验表明其在亚8B模型极端压缩下仍能维持精度。其贡献在于提供了一种硬件友好的量化方案，兼顾压缩率与部署效率。
source: ACL-2026-Findings
selection_source: conference_retrieval
motivation: 小型语言模型部分层信息冗余却占用大量内存与能耗，极低比特量化易损失精度。
method: 提出LieQ框架，按层信息有效性在层间混合精度并保持层内统一比特宽度。
result: 在亚8B模型极端低比特压缩下保持精度且不引入不规则内存访问。
conclusion: 为硬件友好的高效量化部署提供了兼顾精度与效率的方案。
---

## Abstract
Large language models with billions of parameters are often over-provisioned: many layers contribute little unique information yet dominate the memory and energy footprint during inference. We present LieQ (Layer-wise information effectiveness Quantization), a hardware-native, metric-driven post-training quantization framework that addresses the critical challenge of maintaining accuracy in sub-8B models, model parameters less than 8B, under extreme low-bit compression. LieQ keeps uniform bit-width within each layer while mixing precision across layers, preserving standard multiplication kernels and avoiding irregular memory access, codebooks, or irregular formats at inference time. Our method uncovers a strong correlation between layer-wise functional saliency and representational compactness, revealing that layers with higher training-induced energy concentration are functionally irreplaceable. Leveraging this insight, we propose a purely geometry-driven sensitivity proxy that enables automatic bit-width allocation under a target average-bit budget without expensive gradient updates or inference-based perplexity probing. Under an average weight bit-width approaching two bits per parameter, LieQ consistently reduces the large accuracy gap typically observed for naive uniform 2-bit baselines on Qwen3 and LLaMA3.x families, while retaining standard-kernel efficiency. These properties make LieQ a practical path toward deploying small language models on resource-constrained edge devices. Code will be available at: https://github.com/HeXiao-55/LieQ-official.git.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义

- **研究背景**：十亿级参数的大语言模型存在明显“过参数化/过配置”现象，许多层贡献的独特信息有限，却主导推理时的内存与能耗。边缘设备（手机、单板机、机器人、车载/无人机）通常只有 4–12 GB 甚至更低内存预算，连亚 8B 的“小型语言模型”也难以直接部署。
- **核心问题**：极低比特训练后量化（PTQ，尤其平均约 2 bit/权重）会使小型模型精度严重崩溃；现有方法要么依赖不规则稀疏/码本/旋转导致硬件不友好，要么采用统一比特而忽略 Transformer 层间异质性，要么用昂贵的逐层困惑度探测或梯度搜索决定混合精度。
- **整体含义**：论文试图回答三个挑战：①能否在保持规则权重布局的同时实现结构化 PTQ；②能否用静态几何特征预测层功能重要性，免去预探测；③能否在极端低比特下同时获得硬件效率与精度鲁棒性。作者提出 **LieQ**，主张“层间混合精度、层内统一比特宽度”，以几何紧致度作为层重要性代理，在平均约 2 bit 预算下保护关键层。

## 2. 方法论：核心思想与关键技术细节

- **核心思想**：层的功能不可替代性与其“表示紧致度变化”强相关。训练使重要层把信息集中到低秩/低熵流形中；这些层对量化噪声更敏感，应分配更高精度。LieQ 用纯几何指标给层打分，选少量关键层用 4 bit，其余层用 2 bit，从而保持层内统一比特和标准乘法核。
- **功能显著性诊断（作为验证目标）**：对第 ℓ 层做“移除一层”的困惑度扰动：用恒等映射加残差替换该层，计算 \(\Delta PPL_\ell = PPL_{\setminus \ell} - PPL_{base}\)。该指标直接但需 \(L+1\) 次前向，成本高。
- **表示紧致度**：对每个线性投影 \(P\) 的投影表示 \(Z=W_P^{(\ell)}h^{(\ell)}\) 做 SVD；同时构造同分布随机初始化权重对应的 \(\tilde Z\)。对奇异值能量分布 \(p_k=\sigma_k^2/\sum_j\sigma_j^2\)，定义
  \[
  \text{Compact}(Z)=\exp\left(-\sum_k p_k\log p_k\right)
  \]
  即能量分布 Shannon 熵的指数。奇异值越集中，紧致度越低。
- **相对紧致度变化**：
  \[
  \Delta r_\ell^{(P)}=\frac{\text{Compact}(\tilde Z)-\text{Compact}(Z)}{\text{Compact}(\tilde Z)}
  \]
  正值表示训练使表示更集中、该层更关键。另用 Top-k 能量差 \(\Delta E_{k,\ell}^{(P)}=E_k(Z)-E_k(\tilde Z)\) 作为辅助验证。
- **层评分与选择**：对投影和注意力头聚合得到层分数 \(s_\ell=\mathbb{E}_{P,\text{head}}[\Delta r_\ell^{(P,\text{head})}]\)；按 \(s_\ell\) 降序选 Top-\(m\) 层为高精度集合 \(S_{hi}\)，其余为 \(S_{lo}\)。\(S_{hi}\) 分配 4 bit，\(S_{lo}\) 分配 2 bit。
- **预算闭式分配**：目标平均比特 \(\bar b\in[2,4]\)，等参数层时 4-bit 层比例 \(f=(\bar b-2)/(4-2)\)，取 \(m=\text{round}(fL)\)；参数不等时按降序选最小集合，使其参数量占比达到 \(f\)。压缩比定义为
  \[
  CR=\frac{\sum_\ell b_\ell N_\ell}{16\sum_\ell N_\ell}
  \]
  权重相对 FP16 的内存缩减约为 \(16/\bar b\)。
- **部署特性**：层内比特统一，张量连续，每层只需一种乘法核；不引入码本、旋转或逐元素索引。LieQ 与 GPTQ/AWQ 等 PTQ 后端正交，可插件式使用。实验中为探测极限，通常只把 Top-1 层升到 4 bit，其余 2 bit。

## 3. 实验设计

- **模型**：Qwen3-0.6B/1.7B/4B/8B；LLaMA3.2-1B/3B、LLaMA3.1-8B；零样本任务还覆盖 LLaMA2-7B；诊断分析覆盖 LLaMA1/2/3 与 Qwen3 家族。
- **数据集/场景**：
  - 语言建模困惑度：WikiText-2、C4。
  - 诊断相关性：WikiText2、Dolly-15k、HH-RLHF、C4。
  - 零样本推理：PIQA、ARC-e、ARC-c、BoolQ、HellaSwag、Winogrande、MMLU。
  - 端到端：Alpaca 数据集上的解码速度与内存评估。
  - 微基准：LLaMA-3.2-3B 与 LLaMA-3.1-8B 的 gate_proj 层延迟。
- **对比方法**：GPTQ、AWQ、OmniQuant、PB-LLM、SliM-LLM；零样本表中还对比 QuIP#、AQLM；延迟图对比 FP16、GPTQ-4bit、AWQ-4bit、AQLM-2×2bit。
- **评估设置**：PyTorch + HuggingFace 模型；无任务特定校准、无后训练微调；序列长度 512；遵循 AWQ 加速设置做端到端比较。

## 4. 资源与算力

- 文中明确：所有实验在 **单张 NVIDIA RTX 3090（24 GB）** 上完成，启用混合精度，关闭 gradient checkpointing 以保留秩分析所需激活。
- **未明确说明**：GPU 数量（仅单卡）、总实验时长、能耗、校准/前向总成本、不同模型分别耗时。由于是 PTQ，不涉及大规模重训练，但仍有 SVD 与量化校准的前向开销；论文未量化这些开销。

## 5. 实验数量与充分性

- **实验组数概览**：
  - 主困惑度表 2 组：Qwen3 四个尺寸、LLaMA3 三个尺寸，各含 2-bit 与 3-bit，Wiki/C4 两数据集。
  - 零样本表 1 组：Q3-4B、L2-7B、L3-3B，2-bit/3-bit，七个任务。
  - 消融：图 5 在 Qwen3-0.6B/4B、LLaMA3.2-1B/3B 上把 4-bit 层数从 1 增至 16。
  - 效率：图 4 两模型延迟微基准；表 4 三模型端到端解码速度/内存。
  - 诊断：图 1/2/6 跨模型家族与四数据集；报告 \(\Delta PPL\) 与 \(\Delta r\) 的 Spearman 相关 \(\rho>0.8\)。
- **充分性**：覆盖模型家族、尺寸、2/3-bit、困惑度与零样本任务、消融和效率，整体较广；但缺少多次运行方差/置信区间、不同校准数据敏感性、激活量化、生成式长文本任务、更大模型（>8B）验证。
- **客观与公平性**：LieQ 报告了实际平均比特（约 2.00–2.07、3.00），并与同预算方法比较，较规范；但部分基线结果为 N/A，部分对比方法来自不同实现/设置，且 LieQ 的混合精度选择依赖自身诊断，和统一比特基线的“同等预算”需结合压缩比公式细看。端到端仅测解码速度，未系统比较所有基线，公平性有限。

## 6. 主要结论与发现

- **强相关发现**：层功能显著性（移除层导致的 PPL 上升）与表示紧致度变化高度相关，Spearman \(\rho>0.8\)；Top-k 能量差与紧致度趋势一致，可作辅助验证。
- **小模型更脆弱且关键层更集中**：Qwen3-0.6B 等小模型中少数层紧致度变化显著更大，极低比特下更需保护；模型越大，层重要性分布越均匀，固定保护少量层的收益递减。
- **浅层更重要**：Qwen 与 LLaMA 系列中浅层/“super weight”层表现出更高功能重要性，与已有系统性离群值/超权重研究一致。
- **精度结论**：平均约 2 bit 下，LieQ 显著缩小朴素 2-bit 的精度崩溃。例如 Qwen3-0.6B Wiki PPL 从 GPTQ 的 \(2.38\times10^4\) 降至 36.19（FP16 为 20.9）；LLaMA3.2-1B 为 14.53（FP16 9.75）。3-bit 下也具竞争力。
- **零样本任务**：LieQ 在 2-bit 下明显优于 GPTQ/AWQ/OmniQuant/SliM-LLM，但仍低于 FP16；例如 Q3-4B 的 MMLU 63.89 vs FP16 68.25，L2-7B 的 BoolQ 69.45 vs 77.1。
- **效率结论**：层内统一比特保持标准核，gate_proj 微基准显示相对 FP16 的延迟下降；端到端 L1-7B 从 38.85 token/s、12.68 GB 提升到 50.89 token/s、4.32 GB；L1-13B 从 OOM 变为 40.09 token/s、7.83 GB。

## 7. 优点

- **可解释性强**：把量化敏感性与表示几何（奇异值能量熵）联系起来，为“哪些层不能压”提供结构性解释，而非纯经验启发。
- **免昂贵探测**：不需要逐层 PPL 探测、梯度更新或大量验证集搜索，仅用权重统计和单次代表性前向的 SVD，成本较低。
- **硬件友好**：层内统一比特、层间混合精度，避免码本、旋转、不规则索引和碎片化内存访问，保留标准乘法核，工程部署路径清晰。
- **预算可控**：给出闭式平均比特分配规则和压缩比公式，能按目标内存预算自动决定高精度层数。
- **插件式兼容**：可与 GPTQ/AWQ 等后端结合，方法正交性强。
- **实验覆盖较广**：跨 Qwen3、LLaMA3.x、LLaMA2，含困惑度、七项零样本、消融、延迟和端到端内存/速度。

## 8. 不足与局限

- **相关性非因果**：紧致度与功能显著性只是强相关，不能证明几何指标必然导致量化敏感性；随机初始化对照的分布假设也需更多验证。
- **极端配置较简化**：主实验多为 Top-1 层 4-bit、其余 2-bit，可能过度依赖“单层保护”；在 L2-7B 等模型上零样本任务仍有明显下降。
- **覆盖限制**：模型均小于 8B，未验证更大模型、MoE、多模态或长上下文；主要评估权重 PTQ，未系统涉及激活量化。
- **任务覆盖**：以困惑度和多项选择题/常识任务为主，缺少开放式生成、指令跟随、长文本推理和安全性评估。
- **公平性风险**：部分基线为 N/A 或来自不同实现；不同方法的校准

数据、校准样本量、比特预算定义和是否启用后训练微调并不完全一致；LieQ 的层选择依赖自身前向统计，而统一比特基线不具备同等“层间信息分配”机制，因此“同平均比特”比较仍需结合压缩比、实际内存占用和层选择成本综合判断。端到端效率仅覆盖部分模型与部分基线，未系统比较预填充延迟、吞吐、能耗和真实边缘设备上的 kernel 调度开销；混合精度虽保持层内规则布局，但运行时仍需按层切换 2/4-bit 核，可能带来额外分支与启动成本。

- **统计严谨性不足**：未报告多次运行方差、置信区间或显著性检验；量化结果对校准集、随机种子和实现后端的敏感性缺少系统分析。
- **诊断指标的超参敏感性**：紧致度依赖 SVD、随机初始化对照、Top-k 能量差和注意力头聚合方式；不同初始化方差匹配、激活统计层选择和校准样本规模对层排序的影响尚未充分消融。
- **量化后端细节不够透明**：2-bit/4-bit 具体采用 RTN、GPTQ 还是其他后端，层内统一比特下的分组大小、裁剪与补偿策略需更明确，否则复现和公平比较存在障碍。
- **覆盖范围仍偏窄**：未涉及 embedding、LM head、LayerNorm 等非线性投影模块的混合精度；未验证 MoE、多模态、长上下文及大于 8B 的模型；主要评估权重 PTQ，缺少 W4A4/W2A2 等激活量化场景。
- **任务与风险维度不足**：以困惑度和多项选择/常识任务为主，缺少开放式生成、指令跟随、长文本推理、代码、多语言和安全性评估；低比特量化对幻觉、偏见和校准误差的影响未讨论。
- **与混合精度搜索基线比较有限**：未系统对比 HAWQ、混合精度 PTQ、逐层敏感度搜索或强化学习式比特分配方法，难以判断几何评分相对昂贵搜索的净收益。
- **部署验证不完整**：未在手机、单板机、车载等真实边缘硬件上测量端到端能耗、热约束和内存带宽；仅用 RTX 3090 的微基准与服务器端解码速度，不能完全代表目标场景。
- **理论解释仍属相关性框架**：表示紧致度为何导致量化敏感性、何时会失效，缺少理论刻画；随机权重对照的分布假设、层间尺度差异和激活分布漂移可能影响评分稳定性。

## 9. 总体评价与启示

LieQ 的核心贡献不是提出新的 2-bit 量化器，而是提出一种**低成本、硬件友好的层间比特分配准则**：用训练前后表示紧致度变化作为层功能重要性的几何代理，把有限比特预算集中到少数关键层。其价值在于将“哪些层不能压”从昂贵的逐层探测转化为静态权重统计与单次前向 SVD，并与 GPTQ/AWQ 等后端正交，工程可落地性较强。

从证据强度看，论文在 0.6B–8B 的 Qwen3/LLaMA 家族上展示了较一致的诊断相关性和 2-bit 平均预算下的精度 rescue，尤其说明小模型中关键层更集中、浅层更敏感；但实验仍以困惑度和零样本选择题为主，缺少统计误差、生成质量、激活量化和真实边缘部署验证。因此更稳妥的结论是：**LieQ 在“平均约 2 bit、层内统一比特”的受限设定下显著优于朴素统一低比特和若干强 PTQ 基线，并提供了可解释的层选择信号；但它并非无损量化方案，其相对混合精度搜索方法的优势、跨架构泛化和端到端系统收益仍需进一步验证。**

## 10. 开放问题

- 紧致度评分能否推广到 MoE 专家、多模态投影层和长上下文注意力结构？
- 能否将几何评分与轻量敏感度探测结合，在更低成本下获得因果性更强的层排序？
- 在 W4A4/W2A2、KV cache 量化和长文本生成中，层间混合精度是否仍保持优势？
- 真实边缘设备上，混合精度层的 kernel 切换、内存对齐和能耗开销如何抵消理论压缩收益？
- 能否给出紧致度与量化误差上界之间的理论联系，解释何时几何代理会失效？

（完）
