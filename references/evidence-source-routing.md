# 证据来源路由与采集协议

本文件用于 Phase 2。它接收 Phase 1 产出的标准化研究问题、核心实体、同义词、排除边界和用户已提供材料状态，决定本轮需要哪些检索来源，并把实际返回或待检索计划交给 Phase 3。

Phase 2 只负责来源路由和采集边界，不负责文献综述、证据综合、质量评级、趋势判断、研究空白或科研决策。

## 职责边界

| 文件 | 负责什么 | 不负责什么 |
|---|---|---|
| `input-question-standardization.md` | 标准化问题、实体、同义词、排除边界、用户材料状态 | 决定检索来源、采集结果 |
| `evidence-source-routing.md` | 判断是否需要 PubMed、arXiv、Web/Tavily；生成按需检索计划；记录原始返回、来源状态和交付字段 | 正式文献概述、证据强弱判断、趋势分析、创新点判断 |
| `evidence-synthesis-overview.md` | 接收 Phase 2 的来源包，整理文献分布、证据边界、代表性记录和待补证据 | 重新设计检索路由、抓取新结果 |

用户提供材料不是第四个检索源。它是 Phase 1 已识别的输入材料，Phase 2 只需要保留其状态，并判断是否还需要额外检索补充。

## 检索来源职责分工

三类检索来源并不等价。Phase 2 必须先判断每个来源在当前问题中是否需要使用，再决定执行检索、列为计划，或标注为不适用。

| 来源 | 核心定位 | 负责内容 | 输入 | 交付给 Phase 3 | 边界 |
|---|---|---|---|---|---|
| PubMed | 医学文献主干 | 检索生物医学文献，保留标题、摘要、年份、期刊、PMID/DOI、检索式、检索日期和返回数量 | Phase 1 标准化后的疾病、实体、同义词、终点、研究类型、时间范围、排除边界 | PubMed 原始来源包：检索状态、检索问题、检索式、返回记录、缺失字段、来源边界 | 不做正式证据综合、质量评级或趋势判断；不得编造记录、PMID、DOI、数量或结论 |
| arXiv | 方法学与预印本线索 | 检索 AI、机器学习、统计建模、生物信息学流程、多组学算法、计算框架和预印本方法线索 | 疾病或数据类型 + AI/统计/多组学/计算方法关键词 | arXiv 原始来源包：检索状态、检索式、标题、日期、链接、摘要、方法类型初标 | 只提供方法线索，不能替代 PubMed 医学证据，不能单独支持临床或机制结论 |
| Web/Tavily | 官方资源和网页核验 | 核验数据集、临床试验、指南、数据库入口、机构页面、官方资料；在 PubMed 返回少时补查论文线索 | 疾病、实体、数据集名、数据库名、试验/指南/机构关键词，或论文标题/作者/DOI/PMID | Web 原始来源包：检索状态、目标页面、网页标题、机构/数据库、URL、页面类型、可用字段、来源强弱标注 | 网页片段不是正式医学证据；不得把网页搜索写成已调用 GEO、TCGA、KEGG、UniProt 等数据库 API |

## 按需检索计划

Phase 2 不要求每个新主题都生成固定数量的检索包。应根据任务目标和 Phase 1 的完整度，生成最小必要检索计划。

| 触发条件 | 需要的来源 | 输出要求 |
|---|---|---|
| 医学机制、标志物、靶点、疾病相关性问题 | PubMed | 生成 1 个核心 PubMed 检索问题；必要时给出宽检索和聚焦检索两个版本 |
| 问题涉及 AI、统计建模、多组学算法、预测模型或计算流程 | arXiv | 生成方法学检索式，并说明它只用于方法线索 |
| 需要核验数据集、临床试验、指南、数据库、官方页面或机构资料 | Web/Tavily | 生成目标页面或网页检索式，并标注优先官方来源 |
| PubMed 返回很少或缺少明显论文线索 | Web/Tavily | 对论文标题、作者、DOI、PMID 或主题词进行补查，并标注为论文线索补查 |
| 用户已经提供足够论文标题/摘要，且只要求整理这些材料 | 可不新增检索 | 标注为 `用户提供`，说明本轮不代表系统检索全貌 |

如果 Phase 1 缺少关键实体、疾病场景或终点，Phase 2 应先输出待补信息或低风险检索计划，不得强行扩展为完整三源检索。

## 检索式示例

以下只是常见检索式示例，不是每次必须使用的模板。

| 场景 | PubMed 示例 | arXiv / Web-Tavily 示例 |
|---|---|---|
| 机制 | `("Disease") AND ("Mechanism" OR pathway OR inflammation OR fibrosis)` | Web/Tavily：`Disease Mechanism pathway official database review` |
| 生物标志物 | `("Disease") AND (biomarker OR prognosis OR diagnosis) AND ("Gene/Protein")` | Web/Tavily：`Disease Gene Protein biomarker guideline dataset clinical trial` |
| 药物靶点 | `("Disease") AND ("Target") AND (therapeutic target OR druggable OR pathway)` | Web/Tavily：`Disease Target therapeutic target ClinicalTrials.gov database` |
| 多组学 | `("Disease") AND (transcriptomics OR proteomics OR single-cell OR spatial) AND outcome` | arXiv：`multi-omics machine learning disease outcome`; Web/Tavily：`Disease transcriptomics proteomics dataset GEO` |
| AI / 计算方法 | `("Disease") AND (machine learning OR artificial intelligence OR prediction model)` | arXiv：`Disease machine learning prediction model`; Web/Tavily：`Disease AI model benchmark dataset` |

## 来源包交付字段

每次检索或检索规划结束后，向 Phase 3 交付下列来源包。没有实际执行的来源必须标注为 `计划中`，不得写成已检索。

```text
来源包

1. PubMed 原始来源包
   - 执行状态：已检索 / 计划中 / 用户提供 / 不适用
   - 执行者：本 Skill / PubMed search
   - 检索问题：
   - 中文研究问题：
   - English PubMed question：
   - 核心实体、同义词、排除边界：
   - 优先研究类型、时间范围、重点终点：
   - 检索式或用户语料：
   - 原始返回：标题、摘要、年份、期刊、PMID/DOI、链接或来源字段
   - 缺失字段：
   - 来源边界：通常基于摘要和元数据，不等同于全文系统综述；不得用 arXiv 或网页片段替代 PubMed 医学证据

2. arXiv 原始来源包
   - 执行状态：已检索 / 计划中 / 不适用
   - 检索目的：AI、统计模型、多组学算法、计算框架、预印本方法线索
   - 检索式：
   - 原始返回：标题、日期、链接、摘要、方法类型初标
   - 来源边界：

3. Web 原始来源包
   - 执行状态：已检索 / 计划中 / 不适用
   - 核验目的：数据集、临床试验、指南、数据库、官方页面、背景资料或论文线索补查
   - 检索式或目标页面：
   - 原始返回：网页标题、机构/数据库、URL、页面类型、可用字段
   - 来源强弱：官方 / 权威机构 / 数据库页面 / 普通网页 / 来源弱
   - 来源边界：
```

## 降级处理规则

当某个来源没有执行、没有返回、结果过少或来源不可靠时，Phase 2 只做状态标注和检索调整，不直接生成创新结论。

| 情况 | 处理方式 | 不得做什么 |
|---|---|---|
| PubMed 返回很少 | 先检查疾病、实体、同义词、终点和排除边界是否过窄；必要时生成更宽或更聚焦的 PubMed 补充检索式，并用 Web/Tavily 对相关论文标题、作者、DOI、PMID 或主题词进行补查 | 不得仅凭“文献少”判断存在真实创新空白；不得把 Web/Tavily 论文线索直接写成 PubMed 已检索结果 |
| Web/Tavily 非官方或来源弱 | 标注为背景线索或待核验，并在输出中明确写明：`该网页来源较弱，仅作为线索，不能作为正式医学证据`；优先补查官方数据库、试验注册页、指南或机构页面 | 不得把普通网页、新闻、博客写成正式证据 |
