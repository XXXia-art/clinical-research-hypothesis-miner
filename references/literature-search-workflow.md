# 文献检索工作流

当任务涉及 academic-research PubMed 子流程、arXiv、网页检索、文献搜集、数据库核验或来源边界说明时，使用本参考文件。

## 三源编排

本 Skill 不直接改造 academic-research。医学领域强相关的 PubMed 文献检索交给 academic-research；arXiv 与 Web/Tavily 的搜索和处理逻辑保留在本 Skill 中。三源信息汇合后，再进入文献概述、证据地图、假说生成和科研决策。

| 来源/模块 | 责任边界 | 主要输出 | 由谁处理 |
|---|---|---|---|
| academic-research | PubMed 核心医学文献检索、文献筛选、质量评估、趋势分析、结构化综述 | PubMed 检索式、纳入文献、质量/趋势摘要、证据边界 | academic-research |
| arXiv | AI、计算方法、多组学算法、统计模型和预印本方法线索 | 方法论文、算法路线、预印本摘要、方法适配风险 | clinical-research-hypothesis-miner |
| Web/Tavily | 官方页面、数据库、临床试验、指南、数据集和背景核验 | 官方链接、数据集/试验/指南线索、网页证据边界 | clinical-research-hypothesis-miner |

## 本参赛版本检索能力

| 工具类型 | 作用 | 输入 | 输出边界 |
|---|---|---|---|
| academic-research / PubMed | 核心生物医学文献检索与综述 | research topic / query | 由 academic-research 返回结构化 PubMed 文献综述、质量评估和趋势分析 |
| arXiv 检索 | AI、计算方法、多组学算法和预印本方法线索 | query | 返回标题、作者、日期、链接、摘要等结构化结果，通常不是临床强证据 |
| Web/Tavily 检索 | 通用网页/生物医学搜索 | query | 返回带摘要或片段的排序网页结果，适合官方页面和背景核验 |

本参赛版本只声明三源能力：academic-research/PubMed、arXiv、Web/Tavily。不得虚构其他数据库工具；额外数据库只能作为 Web/Tavily 网页核验目标或用户提供材料处理。

这些工具名用于 Skill 内部路由和防幻觉校验。面向用户的科研决策卡不要暴露内部工具名，改用“核心医学文献”“方法学与预印本补充”“网页与数据库核验”“用户提供材料”等来源类型描述。

## 来源分工

| 来源 | 主要作用 | 最适合 | 边界 |
|---|---|---|---|
| academic-research / PubMed | 生物医学证据主干 | 机制、临床相关性、综述、转化研究、疾病-实体-终点证据 | 可能漏掉跨学科方法论文和非医学预印本 |
| arXiv | 方法学与预印本补充 | AI 模型、统计方法、生信流程、多组学算法、计算框架 | 未必经过同行评议，不能替代医学证据 |
| Web/Tavily | 交叉核验和官方页面 | 官方数据库、试验页面、指南页面、数据集页面、背景资料 | 网页片段证据强度低于正式文献 |
| 用户提供语料 | 当前主要工作集 | 粘贴标题/摘要、导出检索结果、已筛选论文 | 完整性取决于用户检索 |

## 最小检索计划

面对新主题，至少生成：

1. **academic-research / PubMed 宽检索：** 疾病 + 机制/主题。
2. **academic-research / PubMed 聚焦检索：** 疾病 + 具体实体 + 终点/模型。
3. **academic-research / PubMed 转化检索：** 疾病 + 实体 + biomarker/target/prognosis/trial。
4. **arXiv 方法检索：** AI / machine learning / multi-omics / computational model + 疾病或方法主题。
5. **网页官方页面核验：** 按需检查数据库、临床试验、指南、数据集或机构页面。

## 检索式模板

| 场景 | academic-research / PubMed 输入 | arXiv / Web-Tavily 检索式 |
|---|---|---|
| 机制 | `("Disease") AND ("Mechanism" OR pathway OR inflammation OR fibrosis)` | Web/Tavily：`Disease Mechanism pathway official database review` |
| 生物标志物 | `("Disease") AND (biomarker OR prognosis OR diagnosis) AND ("Gene/Protein")` | Web/Tavily：`Disease Gene Protein biomarker guideline dataset clinical trial` |
| 药物靶点 | `("Disease") AND ("Target") AND (therapeutic target OR druggable OR pathway)` | Web/Tavily：`Disease Target therapeutic target ClinicalTrials.gov database` |
| 多组学 | `("Disease") AND (transcriptomics OR proteomics OR single-cell OR spatial) AND outcome` | arXiv：`multi-omics machine learning disease outcome`; Web/Tavily：`Disease transcriptomics proteomics dataset GEO` |
| AI / 计算方法 | `("Disease") AND (machine learning OR artificial intelligence OR prediction model)` | arXiv：`Disease machine learning prediction model`; Web/Tavily：`Disease AI model benchmark dataset` |

## 三源检索规则

### academic-research / PubMed：核心医学证据主干

- 医学领域强相关的 PubMed 检索交给 academic-research，不在本 Skill 中重写其内部检索、筛选、质量评估和趋势分析逻辑。
- 本 Skill 接收 academic-research 返回的检索式、检索日期、返回数量、标题、摘要、年份、期刊、PMID/DOI（如返回）、质量评估和趋势摘要。
- 结论措辞以 PubMed 证据为主，不用 arXiv 或网页片段替代医学证据。

### arXiv：方法学与预印本补充

- 仅在问题涉及 AI、机器学习、统计建模、生物信息学流程、多组学算法或计算方法时使用。
- arXiv 结果可以支持“方法可参考”“算法方向活跃”“预印本线索”，不能单独支持临床结论或机制证实。
- 对与医学问题无直接关系的通用 AI 论文，只作为方法候选，不进入核心证据结论。

### Web/Tavily：官方核验与背景补充

- 用于查找官方数据库页面、临床试验注册、指南、数据集入口、机构页面和背景资料。
- 优先官方或权威来源；普通网页、新闻、博客只能作为背景线索。
- 网页搜索结果必须标注为网页核验，不能伪装成正式文献数据库结果。

## 面向用户的来源说明字段

最终回答中保留用户真正需要判断证据可靠性的字段：

- 来源类型：核心医学文献、方法学与预印本补充、网页与数据库核验、用户提供材料。
- 检索式或语料：保留可复查的 query、关键词组合、论文列表或用户粘贴材料摘要。
- 用途：说明该来源用于建立证据主干、补充方法学线索、补充数据集线索，还是做背景核验。
- 纳入范围：已知时说明纳入结果数量、结果类型或时间范围。
- 证据边界：说明是否仅基于标题/摘要、网页片段、用户筛选结果，或尚未执行的计划。
- 执行状态：已检索、用户提供、计划中或背景推断。

内部可记录具体 MCP 工具名，但除非用户要求排查工具调用，否则不要在科研决策卡中展示工具名。

## 结果较弱时

- 如果 academic-research / PubMed 结果很少，放宽同义词，并用 Web/Tavily 核验是否存在数据集、临床试验或官方资料。
- 如果 AI/多组学方法证据不足，用 arXiv 补充方法线索，但必须标注为预印本或方法学补充。
- 如果网页结果不是官方来源，只能作为方向参考。
- 如果所有来源都很薄弱，必须先确认重要性和可检验性，不能直接称为创新点。
