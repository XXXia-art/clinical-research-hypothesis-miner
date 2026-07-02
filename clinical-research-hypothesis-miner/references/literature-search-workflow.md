# 文献检索工作流

当任务涉及 PubMed、Google Scholar、网页检索、文献搜集、引用追踪或来源边界说明时，使用本参考文件。

## 可用 MCP 工具

官方平台文档目前只列出有限 MCP 检索能力，通常包括：

| 工具类型 | 作用 | 输入 | 输出边界 |
|---|---|---|---|
| PubMed 检索 | 核心生物医学文献检索 | query | 返回标题、摘要、期刊、年份等结构化结果，具体字段以工具返回为准 |
| Web 检索 | 通用网页/生物医学搜索 | query | 返回带摘要或片段的排序网页结果 |

不得虚构 Google Scholar、GEO、TCGA、KEGG、UniProt、ClinicalTrials.gov、CNKI、Web of Science、Scopus 或 Embase 的 MCP 工具。只能把它们作为手动/未来核验来源，或作为网页搜索目标。

这些工具名用于 Skill 内部路由和防幻觉校验。面向用户的科研决策卡不要暴露内部工具名，改用“核心医学文献”“广泛召回与引用线索”“网页与数据库核验”“用户提供材料”等来源类型描述。

## 来源分工

| 来源 | 主要作用 | 最适合 | 边界 |
|---|---|---|---|
| PubMed | 生物医学证据主干 | 机制、临床相关性、综述、转化研究 | 可能漏掉跨学科和引用网络信息 |
| Google Scholar | 广泛召回和引用网络 | 高被引论文、施引论文、方法学论文、预印本、数据库论文 | 当前没有直接 MCP；使用用户粘贴结果或检索计划 |
| Web | 交叉核验和官方页面 | 官方数据库、试验页面、最新背景、数据集页面 | 网页片段证据强度低于正式文献 |
| 用户提供语料 | 当前主要工作集 | 粘贴标题/摘要、导出检索结果、已筛选论文 | 完整性取决于用户检索 |

## 最小检索计划

面对新主题，至少生成：

1. **PubMed 宽检索：** 疾病 + 机制/主题。
2. **PubMed 聚焦检索：** 疾病 + 具体实体 + 终点/模型。
3. **PubMed 转化检索：** 疾病 + 实体 + biomarker/target/prognosis/trial。
4. **Google Scholar 宽检索：** 疾病 + 实体/机制 + 宽泛术语。
5. **Google Scholar 引用追踪计划：** 识别 landmark paper、参考文献链和近年施引论文。
6. **网页官方页面核验：** 按需检查数据库、临床试验或数据集官方页面。

## 检索式模板

| 场景 | PubMed 检索式 | Google Scholar 检索式 |
|---|---|---|
| 机制 | `("Disease") AND ("Mechanism" OR pathway OR inflammation OR fibrosis)` | `"Disease" "Mechanism" mechanism review cited by` |
| 生物标志物 | `("Disease") AND (biomarker OR prognosis OR diagnosis) AND ("Gene/Protein")` | `"Disease" "Gene/Protein" biomarker prognosis` |
| 药物靶点 | `("Disease") AND ("Target") AND (therapeutic target OR druggable OR pathway)` | `"Disease" "Target" therapeutic target cited by` |
| 多组学 | `("Disease") AND (transcriptomics OR proteomics OR single-cell OR spatial) AND outcome` | `"Disease" transcriptomics proteomics treatment response` |
| 引用/拥挤度 | 使用 PubMed 综述和近年论文 | `"Topic" review high cited`，然后手动查看施引论文 |

## Google Scholar 处理规则

如果用户提供 Scholar 结果，将其作为广泛召回/引用语料概述。保留标题、年份、发表来源、用户提供的可见引用次数，以及记录类型。

如果用户没有提供 Scholar 结果，输出：

- 检索式。
- 排序方式：相关性、日期或引用数。
- 需要查看的内容：高被引 landmark paper、近年施引论文、综述集群、方法/数据库论文。
- 建议用户粘贴回来的字段：标题、作者、年份、来源、片段、引用次数和施引论文示例。

不得编造引用次数或 `cited by` 关系。

## 面向用户的来源说明字段

最终回答中保留用户真正需要判断证据可靠性的字段：

- 来源类型：核心医学文献、广泛召回与引用线索、网页与数据库核验、用户提供材料。
- 检索式或语料：保留可复查的 query、关键词组合、论文列表或用户粘贴材料摘要。
- 用途：说明该来源用于建立证据主干、识别引用网络、补充数据集线索，还是做背景核验。
- 纳入范围：已知时说明纳入结果数量、结果类型或时间范围。
- 证据边界：说明是否仅基于标题/摘要、网页片段、用户筛选结果，或尚未执行的计划。
- 执行状态：已检索、用户提供、计划中或背景推断。

内部可记录具体 MCP 工具名，但除非用户要求排查工具调用，否则不要在科研决策卡中展示工具名。

## 结果较弱时

- 如果 PubMed 结果很少，放宽同义词，并补充 Scholar 检索计划。
- 如果 Scholar 显示方向拥挤，用 PubMed 区分高质量生物医学证据和泛化网页噪声。
- 如果网页结果不是官方来源，只能作为方向参考。
- 如果所有来源都很薄弱，必须先确认重要性和可检验性，不能直接称为创新点。
