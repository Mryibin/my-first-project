# ChemCoach CChO — Phase 02
# CChO Domain Model & Knowledge Ontology

你正在继续开发同一个项目：

ChemCoach CChO

Phase 01 已完成。

当前项目已经具备：

- Next.js 前端骨架
- FastAPI 后端
- PostgreSQL / pgvector 基础设施
- SQLAlchemy async session
- AIProvider 抽象
- KnowledgeRetriever 抽象
- Tool 接口
- 安全 Calculator
- Logging
- pytest
- Architecture Decision Records

当前系统明确：

NO REAL CChO KNOWLEDGE INGESTED

本轮不要改变这个事实。

---

# 1. Phase 02 唯一目标

本轮不是实现 AI Tutor。

不是实现 RAG。

不是接入真实大模型。

不是做出题或批改。

本轮目标是：

# 建立 ChemCoach CChO 的专业领域模型和知识本体基础。

也就是让系统第一次具备结构化表达以下对象的能力：

- 一道 CChO 题
- 一个知识点
- 多知识点关系
- 一种解题方法
- 一个标准解答
- 一个解题步骤
- 一个常见错误
- 一项能力要求
- 一份考试
- 一份知识来源
- 一个内容版本
- 一个审核状态

最终目标是：

未来任何一道 CChO 题，都可以被结构化描述，而不是只存成一段文本。

---

# 2. 重要原则

## Principle 1

Question 不是 text blob。

一道题不能只存：

question_text

必须可以表达：

- 来源
- 年份
- 考试
- 题号
- 题型
- 难度
- 多个知识点
- 多个能力维度
- 多个解题方法
- 标准解
- 解题步骤
- 常见错误
- 图片/结构式等资产
- 版本
- 审核状态

---

## Principle 2

KnowledgePoint 不是 tag。

知识点之间存在关系。

例如：

前置知识

依赖关系

属于

相关

易混淆

可迁移

因此 KnowledgePoint 必须可以建立 KnowledgeRelation。

---

## Principle 3

Solution 不是一段完整答案。

Solution 必须可拆成：

Solution
→ SolutionStep
→ 使用知识点
→ 使用方法
→ 工具调用
→ 中间结果
→ 最终结果

未来自动批改需要判断：

学生错在哪一步。

所以现在必须支持 step-level solution。

---

## Principle 4

SolutionMethod 独立于 Question。

例如：

- 电子守恒法
- 半反应法
- 极限法
- 守恒法
- 差量法
- 平衡常数比较
- 逆合成分析
- 光谱结构推断

一种方法可以服务多道题。

一道题也可以有多种方法。

因此：

Question ↔ SolutionMethod

many-to-many。

---

## Principle 5

ErrorPattern 独立建模。

不要只记录：

“答案错了”。

错误必须可以表达：

- 概念错误
- 推理错误
- 方法选择错误
- 计算错误
- 符号错误
- 单位错误
- 审题错误
- 化学方程式错误
- 氧化态判断错误
- 条件遗漏
- 结构判断错误
- 结论正确但过程错误

ErrorPattern 未来会连接：

Question

KnowledgePoint

SolutionStep

Student Mistake

Training Recommendation

---

# 3. 核心领域实体

请设计并实现以下核心实体。

注意：

不要机械照抄字段。

先进行领域建模，再给出合理实现。

---

## Exam

表示一套竞赛或训练考试。

建议考虑：

id
name
year
competition
stage
round
source
version
review_status

例如未来可能存在：

中国化学奥林匹克初赛
决赛
冬令营相关考试
训练卷
校内模拟卷

不要在本轮填充真实考试数据。

---

## Question

核心题目实体。

至少考虑：

id

stable_id

title

stem

question_type

difficulty

competition_level

estimated_time

source_id

exam_id

question_number

language

review_status

created_at

updated_at

Question 必须支持版本。

不要假设题目永远只有纯文本。

未来需要支持：

image
chemical structure
table
chart
reaction scheme
formula

因此请设计 QuestionAsset。

---

## QuestionVersion

题目可能：

原始导入

OCR修正

人工修订

结构化修订

必须保留版本历史。

至少考虑：

question_id
version_number
content
change_reason
created_by_type
review_status

---

## QuestionAsset

支持：

image
structure
reaction_scheme
table
chart
attachment

至少包含：

asset_type
storage_uri
mime_type
description
ordering

Phase 02 不需要真正做文件上传。

---

# 4. Knowledge Ontology

设计：

## KnowledgePoint

不要直接采用某一本教材目录作为数据库结构。

知识本体需要长期稳定。

至少考虑：

id

code

name

description

domain

subdomain

level

status

例如未来 domain 可能包括：

general_chemistry
inorganic
organic
physical_chemistry
analytical
structural_chemistry
experimental

但是本轮不要声称这是官方 CChO 分类。

它只是内部 ontology。

---

## KnowledgeRelation

支持知识点之间关系。

关系类型至少考虑：

PREREQUISITE_OF

PART_OF

RELATED_TO

OFTEN_CONFUSED_WITH

APPLIED_IN

EXTENDS

请使用 Enum / controlled vocabulary。

不要自由字符串。

KnowledgeRelation 至少需要：

source_knowledge_id
target_knowledge_id
relation_type
confidence
review_status

---

# 5. Question ↔ KnowledgePoint

必须 many-to-many。

关系本身需要信息。

例如：

QuestionKnowledgePoint

字段可考虑：

question_id
knowledge_point_id

role

importance

required_depth

review_status

role 例如：

PRIMARY

SECONDARY

SUPPORTING

不要把这些信息塞入 JSON。

---

# 6. Skill / Competency

知识点和能力不是同一件事。

例如一道题可能考：

知识：

化学平衡

能力：

定量推理
模型建立
信息提取
结构推断

因此建立：

Skill

以及：

QuestionSkill

未来 Skill 可能包括：

quantitative_reasoning
qualitative_reasoning
structure_inference
mechanism_reasoning
experimental_design
data_interpretation
multi_step_reasoning

这些只是内部分类。

不要把它们描述为官方标准。

---

# 7. SolutionMethod

建立独立实体：

SolutionMethod

至少考虑：

code
name
description
applicable_conditions
advantages
limitations
review_status

建立：

QuestionSolutionMethod

many-to-many。

未来需要支持：

recommended

alternative

advanced

等角色。

---

# 8. Solution

Question 可以有多个 Solution。

例如：

official

textbook

expert

curated

generated

建立：

Solution

至少考虑：

question_id

solution_type

summary

final_answer

source_id

authority_level

review_status

is_preferred

version

重要：

generated solution 永远不能默认：

human_verified。

---

# 9. SolutionStep

Solution 必须拆成步骤。

建立：

SolutionStep

至少考虑：

solution_id

step_number

title

explanation

expression

intermediate_result

final_result

step_type

difficulty

注意：

SolutionStep 未来需要连接：

KnowledgePoint

SolutionMethod

ToolCall

ErrorPattern

因此请设计关系，而不是把所有东西塞进 explanation。

---

# 10. SolutionStep ↔ KnowledgePoint

建立：

SolutionStepKnowledgePoint

使系统未来能够回答：

“学生具体在第几步暴露了哪个知识点问题？”

---

# 11. ErrorPattern

建立：

ErrorPattern

建议至少考虑：

code

name

category

description

severity

diagnostic_hint

remediation_strategy

review_status

Error category 请使用受控 Enum。

例如：

CONCEPT

REASONING

METHOD_SELECTION

CALCULATION

ALGEBRA

UNIT

SIGN

CONDITION

READING

FORMULA

STRUCTURE

EXPERIMENTAL

OTHER

不要在本轮大量生成具体错误内容。

只建立模型和少量 DEMO 数据。

---

# 12. Question ↔ ErrorPattern

建立：

QuestionErrorPattern

用于表达：

这类题经常有哪些错误。

并允许：

error_probability 或 frequency_weight

但不要假装我们现在已经拥有统计数据。

如果使用数值，请明确标记为 curated/demo。

---

# 13. SolutionStep ↔ ErrorPattern

这个关系尤其重要。

未来批改时可以判断：

学生在哪一步犯了什么典型错误。

建立：

SolutionStepErrorPattern

---

# 14. Source Provenance

Phase 01 已经考虑来源。

Phase 02 正式建立来源模型。

至少包括：

KnowledgeSource

字段考虑：

id

source_type

title

author_or_org

publication_year

edition

version

uri

authority_level

copyright_status

license_notes

review_status

checksum

ingested_at

---

# 15. Authority Level

使用严格 Enum：

OFFICIAL

TEXTBOOK

EXPERT

CURATED

GENERATED

不要使用字符串散落在代码里。

---

# 16. Review Status

至少：

UNREVIEWED

AI_REVIEWED

HUMAN_VERIFIED

REJECTED

注意：

AI_REVIEWED != HUMAN_VERIFIED

任何系统逻辑都不能把二者当成同一级别。

---

# 17. Copyright Status

考虑：

UNKNOWN

PRIVATE_USE

PERMISSION_GRANTED

PUBLIC_DOMAIN

LICENSED

RESTRICTED

本轮不进行法律判断。

只是建立字段。

不要自动推断版权状态。

---

# 18. 内容版本

所有核心知识内容需要考虑版本历史：

QuestionVersion

SolutionVersion 或 Solution.version

KnowledgeSource.version

未来知识点也可能调整。

请不要过度复杂化，但要避免以后只能覆盖旧数据。

---

# 19. ID 设计

请认真设计：

database id

stable domain id

display code

不要把数据库自增 ID 当作永远稳定的业务标识。

可以考虑 UUID。

但是需要给出 ADR / 理由。

---

# 20. Soft Delete / Status

竞赛知识内容通常需要：

废弃

替换

修订

而不是物理删除。

请考虑：

status

archived_at

superseded_by

但不要给每张表机械增加一堆字段。

请从领域角度设计。

---

# 21. Database Migration

Phase 02 正式引入 Alembic。

要求：

建立 initial domain migration。

PostgreSQL 为主要目标。

SQLite 仅可用于部分测试，如果 SQLAlchemy 模型允许。

不要为了测试而牺牲 PostgreSQL 数据设计。

---

# 22. Repository Layer

Phase 01 已有 repositories 目录。

请建立最必要的 Repository。

例如：

QuestionRepository

KnowledgePointRepository

SolutionRepository

不要每张表都机械创建 Repository。

优先服务领域聚合。

---

# 23. Domain Layer

不要让 SQLAlchemy Model 就等于完整 Domain Model。

请明确说明：

哪些属于 ORM persistence model

哪些属于 domain schema / entity

哪些属于 API schema

避免后续业务逻辑全部绑在 ORM 上。

---

# 24. Validation

使用 Pydantic / domain validation 保证：

difficulty 有合法范围

relation_type 合法

authority_level 合法

review_status 合法

solution steps 顺序合法

question relation 不允许明显自引用错误

KnowledgeRelation 的 source / target 不得相同

不要只依赖数据库报错。

---

# 25. DEMO 数据原则

可以建立很少量 DEMO 数据用于测试。

但是：

不能称作真实 CChO 真题。

不能使用未经确认的 CChO 官方题目。

建议使用：

自定义基础化学示例

并明确：

DEMO / SYNTHETIC DATA

例如可以使用非常简单的：

酸碱

氧化还原

化学平衡

示例数据。

目的只是测试关系模型。

---

# 26. Domain Example

请建立至少一个完整的 Synthetic Question Example。

要求这道 DEMO 题能够完整串起：

Exam

Question

QuestionVersion

QuestionAsset（可以为空）

KnowledgePoint × 多个

KnowledgeRelation

Skill

SolutionMethod

Solution

SolutionStep × 多步

ErrorPattern

Source

并在 docs 中展示这张关系图。

目的是证明模型能表达：

“一道题是如何被 ChemCoach 理解的”。

---

# 27. 不要做的事情

Phase 02 不要：

接真实 OpenAI

做 Solver

做 Tutor

做问答 API

做 RAG

做 embeddings

做 pgvector 搜索

做 OCR

做扫描试卷

做自动出题

做学习画像

导入真实 CChO 数据

批量生成知识点

抓取网站

---

# 28. Tests

增加完整测试。

至少覆盖：

Question 多知识点关系

KnowledgeRelation

QuestionSkill

Solution 多步骤

SolutionStep 顺序

SolutionMethod 关系

ErrorPattern 关系

Source authority

Review status

Generated 内容不能默认 human verified

Repository 基础 CRUD

Alembic migration

Domain validation

Synthetic Example 完整加载

---

# 29. Documentation

建立：

docs/domain/

至少：

ccho-domain-model.md

knowledge-ontology.md

question-model.md

solution-model.md

source-provenance.md

并生成：

Mermaid ER Diagram

以及：

Synthetic Question 从 Question → Knowledge → Method → Solution → Error 的完整示例。

---

# 30. ADR

新增 ADR：

ADR-005：
为什么 Question 与 KnowledgePoint 使用 many-to-many

ADR-006：
为什么 KnowledgePoint 和 Skill 分离

ADR-007：
为什么 Solution 使用 step-level model

ADR-008：
为什么 ErrorPattern 是一等实体

ADR-009：
为什么 Source Provenance 和 Review Status 必须独立

ADR-010：
ID Strategy

---

# 31. Phase 02 验收标准

完成后我们应该能够回答：

“一道化学竞赛题，在 ChemCoach 数据模型里是什么？”

答案不能只是：

一段题目 + embedding。

而应该能够展示：

Question
│
├── Source
├── Exam
├── Versions
├── Assets
│
├── KnowledgePoints
│     └── KnowledgeRelations
│
├── Skills
│
├── SolutionMethods
│
├── Solutions
│     └── SolutionSteps
│           ├── KnowledgePoints
│           └── ErrorPatterns
│
└── Common ErrorPatterns

如果模型无法清晰表达这个结构，Phase 02 不算完成。

---

# 32. 执行流程

请：

1. 阅读当前 Phase 01 项目
2. 不破坏现有测试
3. 输出 Phase 02 实施计划
4. 完成 domain design
5. 建立 ORM models
6. 建立 Pydantic/domain schemas
7. 建立 Alembic migration
8. 建立最小 repository
9. 建立 synthetic demo
10. 完成 docs
11. 运行全部测试
12. 修复错误
13. 验证旧 Phase 01 功能仍然正常

不要直接跳到 RAG。

---

# 33. 完成报告

完成后请输出：

A. 数据模型总览

B. ER Diagram

C. 新增数据库表

D. 核心关系

E. Synthetic Question 完整示例

F. Alembic migration 状态

G. 测试结果

H. Phase 01 回归测试结果

I. 当前技术债

J. Phase 03 建议

并明确回答：

1.

Question 是否已经支持多知识点？

2.

Solution 是否已经支持 step-level 表达？

3.

ErrorPattern 是否可以关联到具体 SolutionStep？

4.

Generated Solution 是否可能未经审核直接成为 Ground Truth？

正确答案应该是：

不应该。

5.

系统当前是否已经具备真实 CChO 答疑能力？

正确答案仍然是：

不具备。

因为 Phase 02 只建立领域模型，没有导入真实 CChO 专业知识。

现在开始执行 Phase 02。