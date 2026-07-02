# 实体与检索式标准化

当用户提供中文临床表述、缩写、基因、蛋白、通路、药物、组学术语或模糊终点时，在检索或文献概述前使用本参考文件。

## 标准化输出

生成一个紧凑表格：

| 项目 | 标准化结果 | 同义词/检索词 | 排除范围或歧义 |
|---|---|---|---|
| 疾病/表型 | 英文标准疾病名 + 中文名 | 同义词、相关疾病 | 排除疾病、分期限制 |
| 机制/通路 | 标准生物过程或通路名 | 通路同义词 | 容易引入噪声的宽泛词 |
| 基因/蛋白/药物 | 已知官方符号或标准名 | 别名、全称 | 未解决或冲突的符号 |
| 人群/模型 | 人群队列、组织、细胞、动物模型 | 模型相关词 | 物种或模型不匹配 |
| 终点 | 诊断、预后、治疗反应、机制、靶点验证 | 终点同义词 | 模糊或不可测量终点 |

未解决项目标注为 `待核验`。不得编造基因 ID、登录号、MeSH ID、试验 ID 或数据库记录。

## 问题改写

必要时输出三个版本：

1. **临床中文问题：** 让临床医生能直接理解。
2. **科研问题：** 明确疾病、实体、终点和研究边界。
3. **英文检索问题：** 面向 academic-research/PubMed、arXiv 和 Web/Tavily 优化。

示例结构：

```text
Disease/phenotype + mechanism/entity + research use + endpoint/model
```

## 检索词分层

按层构建检索词：

- **疾病词：** 英文标准疾病名、缩写、相关综合征、分期。
- **实体词：** 官方基因/蛋白符号、全称、常见别名。
- **机制词：** ferroptosis、pyroptosis、macrophage polarization、immune exhaustion、fibrosis、metabolic reprogramming 等。
- **研究用途词：** biomarker、prognosis、diagnosis、therapeutic target、resistance、mechanism、pathway。
- **证据类型词：** transcriptomics、proteomics、single-cell、spatial transcriptomics、cohort、animal model、clinical trial。

先使用较宽检索词，再逐步聚焦。不要一开始就把检索式写得过窄，导致领域全貌被遮蔽。

## 歧义检查

下结论前标注以下风险：

- 同一缩写有多个生物医学含义。
- 基因符号与英文普通词或旧别名重叠。
- 中文药物名存在多个英文译名。
- 疾病名包含异质性亚型。
- 生物标志物问题没有定义终点。
- 多组学问题没有说明样本匹配和临床变量。

如果歧义会影响结论，应收窄推荐，而不是猜测。
