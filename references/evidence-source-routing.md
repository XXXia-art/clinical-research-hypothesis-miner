# 证据来源路由、采集与标准化协议

本文件用于 Phase 2。它接收 Phase 1 产出的标准化研究问题以及核心实体等，决定本轮需要哪些检索来源，执行并把原始返回整理为可交给 Phase 3 使用的标准化证据包。Phase 2 不做正式文献综述、证据强弱判断、趋势分析、创新点判断或科研决策。这些任务交给后续阶段。

只要 Phase 2 执行了 PubMed、arXiv、Web/Tavily 检索，或接收了用户提供的论文、摘要、数据、笔记和网页材料，就必须形成可保存的文件级记录。建议文件名为 `evidence-packet-[topic-slug]-[YYYYMMDD].md`。

## 检索来源职责分工

三类检索来源并不等价。Phase 2 必须先判断每个来源在当前问题中是否需要使用，再决定执行检索、列入待补检索，或标注为不适用。

| 来源 | 核心定位 | 负责内容 | 输入 | Phase 2 输出 | 边界 |
|---|---|---|---|---|---|
| PubMed | 医学文献主干 | 检索生物医学文献，保留标题、摘要、年份、期刊、PMID/DOI、检索式、检索日期和返回数量 | Phase 1 标准化后的疾病、实体、同义词、终点、研究类型、时间范围、排除边界 | PubMed evidence packet：检索状态、检索问题、检索式、返回记录、缺失字段、来源边界 | 不做正式证据综合、质量评级或趋势判断；不得编造记录、PMID、DOI、数量或结论 |
| arXiv | 方法学与预印本线索 | 检索 AI、机器学习、统计建模、生物信息学流程、多组学算法、计算框架和预印本方法线索 | 疾病或数据类型 + AI/统计/多组学/计算方法关键词 | arXiv methods packet：检索状态、检索式、标题、日期、链接、摘要、方法类型初标、来源边界 | 只提供方法线索，不能替代 PubMed 医学证据，不能单独支持临床或机制结论 |
| Web/Tavily | 官方资源和网页核验 | 核验数据集、临床试验、指南、数据库入口、机构页面、官方资料；在 PubMed 返回少时补查论文线索 | 疾病、实体、数据集名、数据库名、试验/指南/机构关键词，或论文标题/作者/DOI/PMID | Web verification packet：检索状态、目标页面、网页标题、机构/数据库、URL、页面类型、可用字段、来源强弱标注、来源边界 | 网页片段不是正式医学证据；不得把网页搜索写成已调用 GEO、TCGA、KEGG、UniProt 等数据库 API |

## 按需检索计划

Phase 2 不要求每个新主题都生成固定数量的检索包。应根据任务目标和 Phase 1 的完整度，生成最小必要检索计划。

| 触发条件 | 需要的来源 | 输出要求 |
|---|---|---|
| 医学机制、标志物、靶点、疾病相关性问题 | PubMed | 生成 1 个核心 PubMed 检索问题；必要时给出宽检索和聚焦检索两个版本 |
| 问题涉及 AI、统计建模、多组学算法、预测模型或计算流程 | arXiv | 生成方法学检索式，并说明它只用于方法线索 |
| 需要核验数据集、临床试验、指南、数据库、官方页面或机构资料 | Web/Tavily | 生成目标页面或网页检索式，并标注优先官方来源 |
| 用户已经提供足够论文标题/摘要，且只要求整理这些材料 | 可不新增检索 | 整理为用户材料包，说明本轮不代表系统检索全貌 |

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

## 标准化证据包交付格式

本节规定 Phase 2 文件级记录的交付结构。无论本轮是执行检索、整理用户材料，还是记录某个来源未获得或不适用，都应按同一结构形成标准化证据包，供 Phase 3 直接读取和复用。

建议文件名：`evidence-packet-[topic-slug]-[YYYYMMDD].md`。

标准化证据包应包含以下部分：

### 证据包元数据

| 字段 | 内容 |
|---|---|
| evidence_packet_id | topic slug + 日期或时间戳 |
| phase | Phase 2 |
| 原始问题 | 用户原始输入 |
| 标准化研究问题 | Phase 1 输出 |
| 生成日期 | 当前日期 |

### 来源状态总表

每个来源都必须列一行，即使未执行或不适用。

| 来源 | 执行状态 | query | 返回数量 | 可进入 Phase 3 | 主要边界 |
|---|---|---|---:|---|---|
| PubMed | 已检索 / 未获得 / 不适用 | PubMed 工具返回的 `query` |  | 是/否 | 基于标题、摘要和元数据；不是全文系统综述 |
| arXiv | 已检索 / 未获得 / 不适用 | arXiv 工具返回的 `query` |  | 是/否 | 不能替代 PubMed 医学证据 |
| Web/Tavily | 已检索 / 未获得 / 不适用 | Web 工具返回的 `query` |  | 是/否 | 普通网页只能作为线索 |
| 用户材料 | 用户提供 / 未提供 / 不适用 | 用户原始材料或文件名 |  | 是/否 | 只代表用户提供范围，不代表系统检索结果 |

### PubMed 返回记录表

PubMed 工具返回 JSON 时，Phase 2 应优先按工具原字段整理，不要改造成无法回溯的自定义字段。工具返回的 `success`、`query` 和 `items` 分别对应执行状态、检索式和返回记录。

| uid | url | doi | title | authors | journal | volume | issue | published | pub_types | abstract | keywords | mesh_terms | language |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|

### arXiv 返回记录表

arXiv 工具返回 JSON 时，Phase 2 应按工具原字段整理。工具返回的 `success`、`query`、`total` 和 `papers` 分别对应执行状态、检索式、返回总数和论文记录。

| title | authors | published | link | abstract |
|---|---|---|---|---|

### Web/Tavily 返回记录表

Web 搜索工具返回 JSON 时，Phase 2 应按工具原字段整理。工具返回的 `success`、`query`、`count` 和 `items` 分别对应执行状态、检索式、返回数量和网页记录。

| title | url | snippet | domain |
|---|---|---|---|

### 用户材料记录表

用户材料可能是论文题录、摘要、PDF 摘录、实验结果、表格、笔记、网页粘贴、对话记录或用户自行整理的清单。Phase 2 只做标准化登记和边界标注，不把用户材料改写成 PubMed、arXiv 或 Web/Tavily 的检索结果。

| material_id | material_type | user_source | title_or_label | authors_or_org | date_or_version | url_or_identifier | provided_content | extractable_fields | missing_fields | source_boundary |
|---|---|---|---|---|---|---|---|---|---|---|

字段说明：

| 字段 | 含义 |
|---|---|
| material_id | 本轮证据包内的用户材料编号，例如 `user-01` |
| material_type | 材料类型，例如 paper, abstract, note, table, dataset, trial, guideline, chat, webpage, experiment |
| user_source | 用户提供方式或文件名，例如 pasted text, uploaded file, 手工清单 |
| title_or_label | 原题名；没有题名时用简短标签 |
| authors_or_org | 用户材料中实际出现的作者、机构或组织；没有则写 `未提供` |
| date_or_version | 用户材料中实际出现的日期、年份、版本或访问时间；没有则写 `未提供` |
| url_or_identifier | DOI、PMID、arXiv ID、URL、试验号、数据集编号等；没有则写 `未提供` |
| provided_content | 对用户材料内容的短摘要或关键摘录说明 |
| extractable_fields | 可从材料中明确抽取的字段，例如疾病、实体、终点、方法、样本、数据集 |
| missing_fields | 影响后续判断但用户未提供的字段 |
| source_boundary | 说明该材料只代表用户提供内容，是否需要 PubMed/Web 等进一步核验 |

## 降级处理规则

当某个来源没有执行、没有返回、结果过少或来源不可靠时，Phase 2 只做状态标注、字段整理和检索调整，不直接生成创新结论。

| 情况 | 处理方式 | 不得做什么 |
|---|---|---|
| PubMed 未执行 | 标注为 `未获得`，保留 PubMed 检索问题、检索式和需要补充的证据类型，并列入待补检索 | 不得写成已获得核心医学文献 |
| PubMed 返回很少 | 先检查疾病、实体、同义词、终点和排除边界是否过窄；必要时生成更宽或更聚焦的 PubMed 补充检索式，并用 Web/Tavily 对相关论文标题、作者、DOI、PMID 或主题词进行补查 | 不得仅凭“文献少”判断存在真实创新空白；不得把 Web/Tavily 论文线索直接写成 PubMed 已检索结果 |
| Web/Tavily 非官方或来源弱 | 标注为背景线索或待核验，并在输出中明确写明：`该网页来源较弱，仅作为线索，不能作为正式医学证据`；优先补查官方数据库、试验注册页、指南或机构页面 | 不得把普通网页、新闻、博客写成正式证据 |
| 用户材料不足 | 标注可用字段和缺失字段，必要时生成待补检索计划 | 不得把用户材料包装成系统检索结果 |
