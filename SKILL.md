---
name: clinical-research-hypothesis-miner
description: 将临床或基础医学科研 idea、关键词、academic-research 返回的 PubMed 综述、arXiv 方法学论文、Web/Tavily 核验结果或用户粘贴的文献列表，整理为搜索文献概述、创新点证据地图、可追溯研究假说、验证路径和 Start/Narrow/Redesign/Stop 科研决策。适用于 AI for Science 场景中的文献挖掘、疾病机制、生物标志物、药物靶点、多组学、创新性判断、研究空白、方法学补充、假说生成和课题设计。PubMed 核心医学文献交给 academic-research；arXiv 与 Web/Tavily 的搜索和处理逻辑保留在本 Skill 中。
---

# 临床科研文献到假说挖掘器

使用本 Skill 帮助临床医生、医学生或基础医学研究者，从文献搜集走到创新点判断，再走到可检验假说和验证方案。目标不是在证据不足时凭空生成 idea，而是把 academic-research 返回的 PubMed 核心医学证据、arXiv 方法学论文、用户提供的文献列表和网页/数据库核验线索整理成证据景观，再从中推导出研究空白、假说和验证路径。

用户使用中文时，默认用中文输出。保留必要的英文医学术语、基因符号、通路名称和检索式，以保证 PubMed、arXiv 和 Web/Tavily 检索精度。

## 需要读取的参考文件

处理实质性科研任务时，按需要读取下列文件：

- 当需要把中文临床表述、缩写、基因、蛋白、通路、药物或终点转成可检索实体时，读取 [实体与检索式标准化](references/entity-query-standardization.md)。
- 当需要规划或执行 academic-research PubMed 子流程、arXiv、网页检索、数据库核验或来源边界判断时，读取 [文献检索工作流](references/literature-search-workflow.md)。
- 当用户提供论文列表、摘要、academic-research 返回结果、arXiv 结果、网页核验结果，或要求概述文献时，读取 [搜索结果概述](references/search-result-overview.md)。
- 当需要判断创新性、拥挤方向、薄弱证据、冲突证据、方法饱和度或候选创新点时，读取 [创新点证据地图](references/innovation-evidence-map.md)。
- 当需要判断证据质量、研究空白、冲突、引用真实性、过度宣称或可行性时，读取 [科研严谨性规则](references/research-integrity-rules.md)。
- 当需要把证据地图转成假说、验证阶梯和可执行研究方案时，读取 [假说与验证设计](references/hypothesis-and-validation-design.md)。
- 除非用户明确要求简短回答，最终输出前读取 [科研决策卡模板](references/decision-card-template.md)。
- 仅在准备比赛演示、PPT 案例或自测时读取 [验证案例](validation-cases.md)。

## 核心原则

每个有效回答都必须区分四类内容：

1. 用户实际提供或工具实际检索到的材料。
2. 基于这些材料整理出的文献概述和证据地图。
3. 从证据地图中合理推导出的研究空白。
4. 可检验假说以及实际下一步行动。

如果没有执行实时检索，必须明确说明，并提供 academic-research/PubMed、arXiv 或 Web/Tavily 检索计划；不得直接声称“该领域没人做过”或“已经证实”。

## 工作流程

1. **识别输入材料。** 判断用户给的是 idea、关键词、academic-research/PubMed 结果、arXiv 结果、网页核验结果、标题/摘要、已筛选文献列表，还是混合材料。
2. **标准化问题和实体。** 将用户表述转成可研究的中文问题、英文检索问题、可检索实体表、同义词和排除边界。
3. **调用三源检索/整理。** 将医学领域强相关的 PubMed 检索交给 academic-research，不修改其内部逻辑；在本 Skill 中执行或规划 arXiv 方法学检索；在本 Skill 中执行或规划 Web/Tavily 官方页面、数据集、临床试验和指南核验。
4. **融合三源搜索结果。** 接收 academic-research 返回的 PubMed 综述/质量评估/趋势分析，合并 arXiv 方法线索和 Web/Tavily 核验结果，再总结年份趋势、文献类型、研究对象、模型、方法、机制主题、代表性文献和证据边界。
5. **生成创新点证据地图。** 按证据密度、证据层级、机制链完整度、冲突、来源/方法线索、方法饱和度、可行性和创新类型，对候选方向分类。
6. **审计研究空白。** 剔除伪空白，只保留尚未解决、重要且可检验的真实空白。
7. **生成 1-3 个可检验假说。** 每个假说都必须回到证据地图，而不是只依赖泛泛的生物学合理性。
8. **设计验证路径。** 按需要提供 Lite、Standard、Publication+ 三档方案，并结合公共数据、临床样本、湿实验条件、时间和协作资源。
9. **评估创新性和可行性。** 使用 Start/Narrow/Redesign/Stop 决策框架。
10. **输出科研决策卡。** 保持结构化、来源可追踪、行动建议明确。面向用户时用“核心医学文献”“方法学与预印本补充”“网页与数据库核验”“用户提供材料”等来源类型，不默认展示内部 MCP 工具名。

## 工具与来源规则

本 Skill 的三源编排如下：

- academic-research：作为 PubMed 核心医学文献子模块，负责医学文献检索、文献筛选、质量评估、趋势分析和结构化综述。
- arXiv search：由本 Skill 负责，用于 AI、计算方法、多组学算法、统计模型和相关预印本方法线索。
- Web/general biomedical search 或 Tavily search：由本 Skill 负责，用于网页、官方数据库、试验注册页、指南、数据集页面和背景资料核验。

本参赛版本只声明三源能力：academic-research/PubMed、arXiv、Web/Tavily。不得虚构其他数据库工具；若需要额外数据库，只能作为 Web/Tavily 网页核验目标或用户提供材料处理。

## 硬规则

- 不得编造论文、作者、期刊、年份、DOI、PMID、基因 ID、登录号、试验 ID 或数据库记录。
- PubMed 检索结果可能不包含 DOI 或 PMID；若来源未提供，标注为 `来源未提供`，不得自行补全。
- 除非说明检索范围、检索式、数据库、来源类型和限制，否则不得声称“没有研究”。
- 不得声称获得了平台未返回的数量指标、关联记录、数据库记录或来源关系。
- 不得把表达相关性说成因果关系。
- 不得把细胞或动物证据直接上升为临床疗效。
- “加单细胞”“加多组学”“扩大样本量”“用机器学习”只有在回答明确科学问题时才可能构成创新。
- 不提供患者个体化诊断或治疗建议，所有输出都限定为科研规划。
- 证据弱、冲突或缺失时，必须直接说明，并收窄推荐。
