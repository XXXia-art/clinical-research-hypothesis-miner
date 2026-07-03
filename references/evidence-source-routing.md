# 三源检索与采集工作流

当任务涉及 academic-research/PubMed、arXiv、Web/Tavily 的检索规划、工具路由、结果采集、预处理或来源状态标注时，使用本参考文件。

本文件只回答：**三源信息应该如何取得、如何预处理、如何交付给后续汇总环节。**

注意三源并不完全等价：

- academic-research/PubMed 是复用现有 skill 包，本 Skill 只定义调用契约和返回包接收规则，不重写其内部 PubMed 检索流程。
- arXiv 和 Web/Tavily 由本 Skill 负责检索规划、结果筛选、来源边界标注和预处理。

不要在本文件中完成三源综合概述、证据地图、创新点判断或科研决策；这些任务交给 [证据融合概述](evidence-synthesis-overview.md)、[创新点证据地图](innovation-evidence-map.md) 和 [科研决策卡模板](decision-card-template.md)。

## 职责边界

| 文件 | 负责什么 | 不负责什么 |
|---|---|---|
| `evidence-source-routing.md` | academic-research 调用契约、arXiv 与 Web/Tavily 抓取策略、结果字段、来源状态、预处理规则 | 三源融合概述、研究空白、假说、决策 |
| `evidence-synthesis-overview.md` | 接收三源证据包，汇总文献分布、主题、证据边界和缺口 | 设计检索式、决定工具调用、抓取网页或数据库 |

## 三源编排

本 Skill 不直接改造 academic-research。医学领域强相关的 PubMed 文献检索交给 academic-research；本 Skill 只负责向它提交标准化医学检索问题，并接收其返回的 PubMed evidence packet。arXiv 与 Web/Tavily 的搜索、筛选和预处理逻辑保留在本 Skill 中。

本文件输出标准化的 **三源证据包**，再交给 `evidence-synthesis-overview.md` 汇总处理。

| 来源/模块 | 获取方式 | 本 Skill 的责任 | 交付给汇总环节的内容 |
|---|---|---|---|
| academic-research/PubMed | 复用 existing skill | 提交标准化医学问题；接收检索式、文献、质量评估、趋势分析和证据边界；不模拟其内部检索 | PubMed evidence packet |
| arXiv | 本 Skill 自行规划或执行 | 设计方法学检索式；筛选 AI/统计/多组学/计算方法论文；标注预印本边界和方法迁移风险 | arXiv methods packet |
| Web/Tavily | 本 Skill 自行规划或执行 | 查找官方页面、数据集、临床试验、指南和数据库入口；区分官方来源、网页片段和普通背景网页 | Web verification packet |

## 三源证据包格式

每次检索或检索规划结束后，向后续汇总环节交付下列结构。没有实际执行的来源必须标注为 `计划中`，不得写成已检索。

```text
三源证据包

1. PubMed evidence packet
   - 执行状态：已检索 / 计划中 / 用户提供 / 不适用
   - 执行者：academic-research
   - 检索问题：
   - 检索式或用户语料：
   - 返回内容：标题、摘要、年份、期刊、PMID/DOI、质量评估、趋势摘要等
   - 证据边界：

2. arXiv methods packet
   - 执行状态：已检索 / 计划中 / 不适用
   - 检索目的：AI、统计模型、多组学算法、计算框架、预印本方法线索
   - 检索式：
   - 返回内容：标题、日期、链接、摘要、方法类型、可迁移性风险
   - 证据边界：

3. Web verification packet
   - 执行状态：已检索 / 计划中 / 不适用
   - 核验目的：数据集、临床试验、指南、数据库、官方页面、背景资料
   - 检索式或目标页面：
   - 返回内容：网页标题、机构/数据库、URL、页面类型、可用字段
   - 证据边界：
```

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

1. **academic-research 调用问题包：** 疾病 + 核心机制/实体 + 终点 + 排除边界。
2. **academic-research 聚焦问题包：** 疾病 + 具体基因/蛋白/通路/干预 + 临床或实验终点。
3. **academic-research 转化问题包：** 疾病 + 实体 + biomarker/target/prognosis/trial。
4. **arXiv 方法检索：** AI / machine learning / multi-omics / computational model + 疾病或方法主题。
5. **Web/Tavily 官方页面核验：** 按需检查数据库、临床试验、指南、数据集或机构页面。

## 检索式模板

| 场景 | academic-research / PubMed 输入 | arXiv / Web-Tavily 检索式 |
|---|---|---|
| 机制 | `("Disease") AND ("Mechanism" OR pathway OR inflammation OR fibrosis)` | Web/Tavily：`Disease Mechanism pathway official database review` |
| 生物标志物 | `("Disease") AND (biomarker OR prognosis OR diagnosis) AND ("Gene/Protein")` | Web/Tavily：`Disease Gene Protein biomarker guideline dataset clinical trial` |
| 药物靶点 | `("Disease") AND ("Target") AND (therapeutic target OR druggable OR pathway)` | Web/Tavily：`Disease Target therapeutic target ClinicalTrials.gov database` |
| 多组学 | `("Disease") AND (transcriptomics OR proteomics OR single-cell OR spatial) AND outcome` | arXiv：`multi-omics machine learning disease outcome`; Web/Tavily：`Disease transcriptomics proteomics dataset GEO` |
| AI / 计算方法 | `("Disease") AND (machine learning OR artificial intelligence OR prediction model)` | arXiv：`Disease machine learning prediction model`; Web/Tavily：`Disease AI model benchmark dataset` |

## 三源检索规则

### academic-research/PubMed：复用 skill 调用契约

academic-research 专注于基于 PubMed 的医学文献检索、文献筛选、质量评估、趋势分析和结构化综述。本 Skill 复用它作为 PubMed 核心医学证据子模块。

本 Skill 只做四件事：

1. **准备输入。** 向 academic-research 提交标准化医学检索问题，包括疾病、实体、同义词、终点、研究类型、时间范围和排除边界。
2. **保留返回。** 接收并保留 academic-research 返回的检索式、检索日期、返回数量、标题、摘要、年份、期刊、PMID/DOI（如返回）、质量评估、趋势分析和综合评述。
3. **标注边界。** 标明该结果来自 PubMed evidence packet，通常基于摘要和元数据，不等同于全文系统综述。
4. **交给汇总。** 将返回内容原样纳入三源证据包，交给 `evidence-synthesis-overview.md` 做三源融合。

本 Skill 不做以下事情：

- 不重写 academic-research 的 PubMed 检索、筛选、质量评估和趋势分析逻辑。
- 不在 academic-research 未返回时自行编造 PubMed 记录、PMID、DOI、数量或趋势。
- 不用 arXiv 或网页片段替代 PubMed 医学证据。

建议提交给 academic-research 的问题包格式：

```text
academic-research input
- 中文研究问题：
- English PubMed question：
- 核心实体：
- 同义词：
- 排除边界：
- 优先研究类型：
- 时间范围：
- 需要特别关注的终点：
```

### arXiv：方法学与预印本补充

- 仅在问题涉及 AI、机器学习、统计建模、生物信息学流程、多组学算法或计算方法时使用。
- arXiv 结果可以支持“方法可参考”“算法方向活跃”“预印本线索”，不能单独支持临床结论或机制证实。
- 对与医学问题无直接关系的通用 AI 论文，只作为方法候选，不进入核心证据结论。

### Web/Tavily：官方核验与背景补充

- 用于查找官方数据库页面、临床试验注册、指南、数据集入口、机构页面和背景资料。
- 优先官方或权威来源；普通网页、新闻、博客只能作为背景线索。
- 网页搜索结果必须标注为网页核验，不能伪装成正式文献数据库结果。

## 证据包交付字段

交付给 `evidence-synthesis-overview.md` 时，每个来源至少保留下列字段：

- 来源类型：核心医学文献、方法学与预印本补充、网页与数据库核验、用户提供材料。
- 检索式或语料：保留可复查的 query、关键词组合、论文列表或用户粘贴材料摘要。
- 用途：说明该来源用于建立证据主干、补充方法学线索、补充数据集线索，还是做背景核验。
- 纳入范围：已知时说明纳入结果数量、结果类型或时间范围。
- 证据边界：说明是否仅基于标题/摘要、网页片段、用户筛选结果，或尚未执行的计划。
- 执行状态：已检索、用户提供、计划中、不适用。

内部可记录具体 MCP 工具名，但交付给汇总环节时优先使用“核心医学文献”“方法学与预印本补充”“网页与数据库核验”“用户提供材料”等来源类型。

## 结果较弱时

- 如果 academic-research / PubMed 结果很少，放宽同义词，并用 Web/Tavily 核验是否存在数据集、临床试验或官方资料。
- 如果 AI/多组学方法证据不足，用 arXiv 补充方法线索，但必须标注为预印本或方法学补充。
- 如果网页结果不是官方来源，只能作为方向参考。
- 如果所有来源都很薄弱，必须先确认重要性和可检验性，不能直接称为创新点。
