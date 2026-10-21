---
title: "DIAA: A Decoding-Efficient Inference Acceleration Approach for On-Device Large Language Models"
title_zh: DIAA：面向端侧大模型的解码高效推理加速方法
authors: "Hao Tian, Sheng Lu, Fuwen Tian, Guangming Cui, Zheng Li, Xuyun Zhang, Quan Z. Sheng, Wanchun Dou"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39789/43750"
tags: ["query:coral-npu"]
score: 5.0
evidence: 面向边缘内存受限端侧大模型的高效解码推理加速
tldr: 针对端侧大模型受限于算力与内存、现有推测解码依赖额外草稿模型与静态令牌树导致内存开销大且推理次优的问题，本文提出DIAA，一种解码高效的端侧推理加速方法。该方法在草稿-验证范式下避免引入辅助模型并动态构造验证结构，以降低内存占用并提升推理效率。实验表明其能在不损生成质量的前提下加速端侧推理，为边缘设备上的低延迟本地大模型服务提供了可行方案。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39789/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39789/fig-002.webp\", \"caption\": \"\", \"page\": 2, \"index\": 2, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39789/fig-003.webp\", \"caption\": \"\", \"page\": 3, \"index\": 3, \"width\": 886, \"height\": 738}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39789/fig-004.webp\", \"caption\": \"\", \"page\": 3, \"index\": 4, \"width\": 886, \"height\": 738}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39789/fig-005.webp\", \"caption\": \"\", \"page\": 3, \"index\": 5, \"width\": 3098, \"height\": 1824}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39789/fig-006.webp\", \"caption\": \"\", \"page\": 3, \"index\": 6, \"width\": 876, \"height\": 1525}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39789/fig-007.webp\", \"caption\": \"\", \"page\": 3, \"index\": 7, \"width\": 1743, \"height\": 218}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39789/fig-008.webp\", \"caption\": \"\", \"page\": 3, \"index\": 8, \"width\": 751, \"height\": 670}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39789/fig-009.webp\", \"caption\": \"\", \"page\": 3, \"index\": 9, \"width\": 742, \"height\": 680}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39789/fig-010.webp\", \"caption\": \"\", \"page\": 3, \"index\": 10, \"width\": 695, \"height\": 217}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39789/fig-011.webp\", \"caption\": \"\", \"page\": 3, \"index\": 11, \"width\": 695, \"height\": 218}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39789/fig-012.webp\", \"caption\": \"\", \"page\": 3, \"index\": 12, \"width\": 695, \"height\": 218}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39789/fig-013.webp\", \"caption\": \"\", \"page\": 3, \"index\": 13, \"width\": 695, \"height\": 217}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39789/fig-014.webp\", \"caption\": \"\", \"page\": 3, \"index\": 14, \"width\": 1896, \"height\": 1525}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39789/fig-015.webp\", \"caption\": \"\", \"page\": 3, \"index\": 15, \"width\": 832, \"height\": 1072}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39789/fig-016.webp\", \"caption\": \"\", \"page\": 3, \"index\": 16, \"width\": 676, \"height\": 309}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39789/fig-017.webp\", \"caption\": \"\", \"page\": 3, \"index\": 17, \"width\": 676, \"height\": 464}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39789/fig-018.webp\", \"caption\": \"\", \"page\": 3, \"index\": 18, \"width\": 832, \"height\": 1072}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39789/fig-019.webp\", \"caption\": \"\", \"page\": 3, \"index\": 19, \"width\": 676, \"height\": 309}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39789/fig-020.webp\", \"caption\": \"\", \"page\": 3, \"index\": 20, \"width\": 383, \"height\": 483}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39789/fig-021.webp\", \"caption\": \"\", \"page\": 3, \"index\": 21, \"width\": 377, \"height\": 483}]"
motivation: 端侧设备算力与内存受限，现有推测解码依赖额外草稿模型与静态令牌树，带来内存负担与次优性能。
method: 提出解码高效的推测解码加速方法，去除辅助草稿模型并改进验证结构以适配端侧资源。
result: 在不损生成质量前提下提升端侧大模型推理效率并降低内存开销。
conclusion: 为边缘设备上的低延迟本地大模型推理提供了资源友好的加速方案。
---

## Abstract
Large Language Models (LLMs) have revolutionized intelligent interactions, enabling mobile applications such as personal assistants on edge devices for local execution. Speculative decoding (SD) has emerged as a promising paradigm to accelerate LLM inference without compromising generation quality, employing a draft-then-verify manner. However, due to the constrained computing and memory resources on edge devices, existing SD works heavily rely on an auxiliary draft model that incurs additional memory burden and hinders the adaptability, as well as static token trees that yield suboptimal inference performance. To this end, we propose DIAA, a Decoding-efficient Inference Acceleration Approach for on-device LLMs. DIAA achieves plug-and-play and model-agnostic inference speedup with memory and computation efficiency for edge devices. Specifically, a pair of lightweight look-up tables (LUTs) is constructed by Top-K token sampling to cache historical tokens and probabilities for rapid candidate drafting. DIAA integrates a dynamic token tree with prior LUTs enabling paralleled verification, updated during decoding process, to adapt the online context. A computation overlap is then employed to pipeline the update operations of token tree, LUTs, and KV cache to improve the computational efficiency. Finally, through extensive experiments implemented on edge platform NVIDIA Jetson, DIAA outperforms existing baselines in generation speed and inference wall-clock time, while incurring minimal memory overhead.

---

## 论文详细总结（自动生成）

# DIAA：面向端侧大模型的解码高效推理加速方法——论文总结

## 1. 核心问题与整体含义（研究动机与背景）

- **背景**：LLM 正从云端向端侧（IoT、移动设备）迁移，以降低延迟、保护隐私并减少通信成本。但自回归解码（AR）逐 token 顺序生成、每步都需完整前向传播，在资源受限的边缘设备上成为主要计算瓶颈。
- **现有路线及其局限**：剪枝、量化、蒸馏存在精度与效率的权衡；云端/边缘卸载带来不可控的通信开销。推测解码（SD）通过“草稿—验证”范式实现无损加速，是较有前景的替代方案，但现有 SD 在端侧存在三大问题：
  1. **模型依赖**：基于模型的方法需要额外微调一个对齐的轻量草稿模型，阻碍快速适配与领域定制；
  2. **内存负担**：辅助草稿模型带来额外显存开销（如 8GB Jetson 上额外约 1GB），并增加部署与维护复杂度；
  3. **静态结构**：现有树状方法采用一次性构建、固定不变的静态验证树，无法随生成上下文动态调整，导致接受率次优、加速有限。
- **核心研究问题**：如何在不引入辅助模型的前提下，为端侧 LLM 最大化推理加速，同时动态适应生成上下文？

## 2. 方法论

- **整体框架**：DIAA 采用“离线配置 + 在线解码”两阶段。离线阶段用 warm-up 数据集（ShareGPT）初始化一对 LUT；在线阶段迭代执行动态树构建、草稿、树状注意力验证与 LUT/KV Cache 更新，直至生成 EOS。
- **关键技术一：LUT 增强的候选 token 初始化**
  - **t-LUT**：以部分上下文（定长 token 前缀或哈希嵌入）为索引，缓存 Top-K 最可能的下一 token ID（按似然降序）。
  - **p-LUT**：存储对应 Top-K token 的概率，用于预算感知的树扩展与低置信路径早期剪枝。
  - **在线更新机制**：① *add* 操作——已验证的（上下文，token）对若不存在则新建条目；② *replacement* 操作——Top-K 列表满时，新验证 token 替换信息量最小的条目，p-LUT 同步更新。
- **关键技术二：动态 token 树构建与草稿**
  - 目标是**在 token 预算 B 约束下最大化根到叶路径的累积概率**。树效用定义为各路径概率连乘之和，代价为草稿 token 总数，需满足 C(T) ≤ B。
  - 使用**优先队列贪心扩展**：默认扩展第一层节点；路径打分引入**深度衰减 δ 与宽度衰减 α**，即得分 = 路径概率 × δ^(|π|−1) × α^(topk(π)−1)，低于阈值 τ 的路径被剪枝。
  - 树在解码过程中按预定义间隔动态刷新，以平衡搜索深度与效率。
- **关键技术三：动态树验证**
  - 选取得分最高的路径进行验证；验证正确后将其对应 KV cache 条目拷贝至主缓存缓冲区，避免重复计算隐状态。
  - 树节点中的 token 与概率同步回写至 t-LUT 与 p-LUT。
- **关键技术四：资源效率导向的计算重叠**
  - 利用 **CUDA 流级并行**，将 KV cache 回滚与 LUT 更新卸载到后台流，主流程继续下一轮 GPU 操作，同时 CPU 并行构建/更新动态树，从而隐藏树构建开销、提升流水线效率。

## 3. 实验设计

- **模型**：Llama-3.2-3B-Instruct-INT4、Llama-3.2-1B-Instruct、Vicuna-7b-v1.5-INT4（均用 GPTQ 量化为 INT4 以适配 Jetson 内存）。
- **Benchmark**：Spec-Bench，涵盖 6 类任务——摘要（Sum）、数学推理（Math）、多轮对话（MT）、问答（QA）、检索增强生成（RAG）、机器翻译（Trans），生成长度上限 512 token。
- **设置**：边缘场景、贪心解码、batch size = 1、Top-K = 8；LUT 用 ShareGPT 数据集构建。
- **对比基线**：① Vanilla（标准自回归）；② SpS（辅助草稿模型推测采样）；③ SWIFT（即插即用、无模型的自推测解码）；④ SAM-D（基于后缀自动机的检索式草稿）；⑤ TR（Token Recycling，静态草稿树）。
- **评价指标**：Goodput（tokens/s，被目标模型验证接受的 token 速率）、Latency（每 token 平均墙钟时延，ms）、Speedup ratio（相对 Vanilla 的端到端时延加速比），三次试验取平均。

## 4. 资源与算力

- **硬件平台**：NVIDIA Jetson Orin Nano Super（典型边缘平台），8GB 统一内存、256GB SSD、1024 核 GPU、67 TOPS（INT8）。
- **软件环境**：Jetpack v6.2、CUDA v12.6、PyTorch v2.7.0。
- **模型来源与量化**：Hugging Face 下载，经 GPTQ-Model 量化为 INT4。
- **训练时长**：文中未提及——因为 DIAA 属于推理加速方法，不涉及模型训练；仅使用离线 warm-up 数据集构建 LUT，未报告该过程耗时。总体属于**单设备推理实验**，未使用多卡或多机算力。

## 5. 实验数量与充分性

- **主实验规模**：3 个模型 × 6 个任务 × 6 种方法（含 DIAA）= 108 组 goodput/latency 结果（表 1），并汇总 3 个模型的平均加速比（表 2）。
- **消融实验**：分别去除 LUT+DT、LUT、DT、计算重叠（CO），共 5 种配置 × 6 个任务（表 3），验证各组件贡献。
- **敏感性实验**：不同树节点规模 × 不同更新间隔 N（20/50/100）对 goodput 与 latency 的影响（图 8）；不同 LUT 初始化模型的性能对比（图 9）。
- **开销分析**：t-LUT/p-LUT 内存占用（表 4）、解码阶段时间分解（图 10）。
- **动机验证实验**：显存随上下文长度变化（图 2）、静态 vs 动态树的速度与接受率（图 3）、6 任务接受率热力图（图 4）。
- **充分性与公平性评价**：整体实验较充分，覆盖多模型、多任务、多基线，并有消融与敏感性分析；采用统一量化、贪心解码、三次取平均，基线选择覆盖主流 SD 路线，比较基本公平。但缺少多种子方差报告、缺少统计显著性检验；所有基线在同一设备上复现程度未明确说明；任务仅限 Spec-Bench 六类，场景覆盖有限。

## 6. 主要结论与发现

- DIAA 在多数任务上取得最高或接近最高的 goodput，且 per-token 延迟一致更低，跨模型（1B–7B）表现稳定。
- **平均加速比**：Llama-3.2-3B 上 2.22×、Vicuna-7B 上 1.78×、Llama-3.2-1B 上 2.05×，均优于 SpS、SWIFT、SAM-D、TR。
- **LUT 与动态树均关键**：去除任一组件 goodput 明显下降，动态树对多路径推测类任务尤为重要；计算重叠进一步提升并行效率。
- **树规模存在最优区间**：适中节点数即可兼顾草稿覆盖与验证开销。
- **LUT 内存极轻**：3B 模型约 11.7MB、Vicuna-7B 约 2.9MB；同词表模型可共享 LUT，支持快速适配，且不同初始化下性能相近。
- **时间开销分析**：前向计算占解码耗时 91%–96%，动态树更新仅约 0.15%、LUT 更新约 2.7%，说明关键组件时间效率高。

## 7. 优点

- **即插即用、模型无关**：无需辅助草稿模型或重训练，规避了端侧内存负担与适配难题，且保持无损生成。
- **内存开销极小**：LUT 仅数 MB 级，且同词表模型可共享，适合资源严格受限设备。
- **动态自适应**：动态 token 树结合深度/宽度衰减与阈值剪枝，能随上下文与预测置信度调整，接受率优于静态树。
- **工程优化到位**：CUDA 流级计算重叠将树构建（CPU）与 LUT/KV cache 更新（GPU 后台流）流水化，隐藏额外开销。
- **验证环节务实**：验证后 KV cache 复用避免重算，兼顾正确性与效率。
- **实验贴近真实部署**：在真实边缘平台 Jetson 上、统一 INT4 量化条件下与 5 类主流基线对比，指标（goodput/latency/speedup）贴合实际交互体验。

## 8. 不足与局限

- **硬件覆盖单一**：仅在 Jetson Orin Nano Super（8GB）上评测，未验证其他边缘芯片、内存规格或异构设备，泛化性存疑。
- **模型规模有限**：仅评测 1B–7B 模型，未涉及更大模型或非 Llama/Vicuna 架构（如 MoE 模型）。
- **解码设置受限**：

- **解码设置受限**：仅采用贪心解码（greedy）、batch size = 1，未涉及采样解码（temperature / top-p / beam search）、批处理或多请求并发场景。而真实端侧服务往往需要多样本采样与多用户并发，DIAA 在这些设置下的加速比、接受率与内存表现尚不明确，尤其动态树在 batch > 1 时如何共享与调度未讨论。
- **任务与长度覆盖有限**：仅评测 Spec-Bench 六类任务、生成长度上限 512 token，未涉及超长生成、代码生成、结构化/JSON 输出、工具调用等对草稿质量敏感的负载，长上下文下 LUT 命中率与树构建开销的演化也未刻画。
- **基线复现公平性存疑**：文中未明确说明 SpS、SWIFT、SAM-D、TR 是否在同一 Jetson 设备、同一 INT4 量化与同一超参调优预算下完整复现，跨方法比较的公平性缺少细节支撑。
- **统计严谨性不足**：仅“三次试验取平均”，未报告方差、置信区间或显著性检验，小样本下的性能差异（尤其任务间接近的 goodput）可能不稳健。
- **LUT 依赖 warm-up 分布**：LUT 由 ShareGPT 构建，若目标领域（如医疗、法律、代码）与 warm-up 分布偏移较大，初始接受率可能下降；在线更新虽可缓解，但收敛速度与冷启动代价未量化。
- **超参数选取依据不充分**：深度衰减 δ、宽度衰减 α、剪枝阈值 τ、树刷新间隔 N 等关键超参的取值来源、调优过程及联合敏感性分析较为简略，削弱了方法的可复现性与迁移指导性。
- **对比前沿方法缺失**：未与 EAGLE、Medusa、Lookahead 等更具代表性的推测解码/多头草稿方案对比，难以判断 DIAA 相对当前 SOTA 的真实位置。
- **能效指标缺位**：端侧核心约束之一是功耗与热设计，但全文未报告能耗、功耗或能效（tokens/J），仅以 goodput/latency 衡量，评价维度不够完整。

## 9. 启示与可借鉴点

- **“无模型”路线的工程价值**：DIAA 证明了在不引入草稿模型、不重训练的前提下，仅靠轻量 LUT + 动态树即可获得 1.78×–2.22× 的实测加速，这对内存与部署复杂度极度敏感的端侧场景具有很强的落地参考意义。
- **检索式先验与动态搜索的结合范式**：以 LUT 提供高概率候选种子、以预算约束下的贪心树扩展做组合搜索，本质上是“统计先验 + 在线搜索”的混合策略，可迁移到其他需要候选生成的加速场景（如约束解码、结构化生成）。
- **计算重叠的系统级思路**：将 CPU 侧树构建与 GPU 后台流的 KV cache 回滚/LUT 更新流水化，把“算法开销”隐藏在“计算等待”之中，这一工程手法对任何带额外预处理开销的推理加速方法都具有普适借鉴价值。
- **以接受率为核心的树设计**：深度衰减 + 宽度衰减 + 阈值剪枝的组合，为“如何在固定 token 预算下平衡探索深度与广度”提供了一个可复用的启发式打分模板。
- **轻量状态共享**：同词表模型可共享 LUT、LUT 仅数 MB，这一设计启示我们：端侧多模型共存时，可将公共先验做成可共享的“热缓存”，降低多模型部署的边际成本。

## 10. 总体评价

DIAA 是一篇面向**真实端侧部署**的推理加速工作，其核心贡献在于提出了一条“**无辅助模型、内存极轻、动态自适应**”的推测解码新路径：用 LUT 缓存局部上下文的高概率续写作为候选种子，用预算约束下的动态 token 树做组合扩展，再以树状注意力验证保证无损，并通过 CUDA 流级计算重叠隐藏额外开销。方法设计目标明确、组件职责清晰，且实验在真实边缘平台（Jetson Orin Nano Super）上以统一 INT4 量化与多类主流基线对比，指标贴合实际交互体验，结论（约 1.78×–2.22× 平均加速、LUT 仅数 MB、动态树优于静态树）具有较强说服力。

其局限主要集中在**评测广度与统计严谨性**：单硬件、1B–7B 模型、贪心单 batch、六类任务与 512 token 上限，加之缺少方差/显著性报告与前沿方法（EAGLE、Medusa 等）对比，使其“普适优越性”的论断仍需更广验证；此外超参选取依据与能效指标亦有补强空间。

综合来看，DIAA 在“端侧无模型推测解码”这一细分方向上提供了扎实的工程化方案与可信的实证支撑，是一篇**方法务实、实验贴近落地、可复现性较好但外推性有待检验**的系统性工作，对边缘 LLM 推理优化的后续研究具有明确的参考与借鉴价值。

（完）
