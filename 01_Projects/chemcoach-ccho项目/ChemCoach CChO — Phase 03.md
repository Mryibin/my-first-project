# ChemCoach CChO — Phase 03
# Knowledge Ingestion & Retrieval Engine

你正在继续开发同一个长期项目：

# ChemCoach CChO

当前状态：

Phase 01 已完成：
- 工程骨架
- FastAPI
- Next.js
- PostgreSQL / pgvector 基础设施
- AIProvider
- KnowledgeRetriever
- Tool Layer
- Logging / Testing

Phase 02 已完成：
- CChO Domain Model
- Exam
- Question
- QuestionVersion
- QuestionAsset
- KnowledgePoint
- KnowledgeRelation
- Skill
- SolutionMethod
- Solution
- SolutionStep
- ErrorPattern
- KnowledgeSource
- Review / Authority / Copyright 状态
- 多对多关系
- Alembic
- Repository
- Synthetic Example

并且已经确认：

Question 支持多知识点。

Solution 支持 step-level。

ErrorPattern 可关联具体 SolutionStep。

Generated Solution 默认：

GENERATED
+
UNREVIEWED
+
not preferred

当前系统：

仍然不具备真实 CChO 专业答疑能力。

---

# 1. Phase 03 唯一目标

本轮目标：

# 建立可用于真实 CChO 专业知识的 Knowledge Ingestion & Retrieval Engine。

实现：

合法获得的专业资料

→ 导入

→ 解析

→ 标准化

→ 分段

→ 元数据绑定

→ 内容审核

→ 索引

→ 检索

→ 来源追溯

→ 检索 Evaluation

的完整闭环。

注意：

Phase 03 仍然不是 AI Tutor 阶段。

不要实现完整 Solver / Tutor。

本轮核心问题是：

# 系统能不能可靠地找到正确知识？

而不是：

# LLM 能不能把答案说得很好听？

---

# 2. 产品可靠性原则

Knowledge Engine 是整个 ChemCoach 的 Grounding Layer。

必须遵循：

## Principle 1

Retrieval ≠ Ground Truth。

“检索到了”不代表“内容一定正确”。

任何检索结果都必须携带：

source provenance
authority level
review status
version
copyright status

---

## Principle 2

权威来源优先。

推荐排序：

OFFICIAL
>
TEXTBOOK
>
EXPERT
>
CURATED
>
GENERATED

但是不要简单地只按 authority 排序。

相关性仍然必须参与排名。

---

## Principle 3

HUMAN_VERIFIED 与 AI_REVIEWED 必须严格区分。

AI_REVIEWED 不能自动升级为 HUMAN_VERIFIED。

---

## Principle 4

GENERATED 内容不能作为默认 Ground Truth。

除非显式配置允许，否则默认检索策略应该：

降低 GENERATED 内容排序权重。

如果存在可靠的 OFFICIAL / TEXTBOOK / EXPERT 内容：

优先返回可靠来源。

---

## Principle 5

资料不足时必须可以返回：

INSUFFICIENT_EVIDENCE

不要为了保证“有结果”而返回低相关内容。

---

## Principle 6

任何最终可用于未来答题的知识片段必须可追溯回：

原始 Source

原始 Document

Document Version

Chunk

导入时间

审核状态

---

# 3. Phase 03 范围

本轮实现：

KnowledgeSource
→ KnowledgeDocument
→ KnowledgeDocumentVersion
→ KnowledgeChunk
→ Index
→ Retriever
→ RetrievalResult
→ Evaluation

本轮不要实现：

完整 Tutor

Solver Agent

Question Generator

Grader

Student Profile

OCR 试卷批改

多 Agent 循环

自动学习计划

---

# 4. Knowledge Document Model

在现有 Phase 02 Source Provenance 基础上完善：

## KnowledgeDocument

表示一份具体知识文档。

例如未来可能是：

- CChO 官方工作条例
- 竞赛实施细则
- 一份官方真题
- 一份参考答案
- 一本教材中的一个章节
- 一篇专家解析
- 一份经过审核的竞赛训练材料

建议字段：

id

stable_id

source_id

document_type

title

language

publication_year

competition_stage

subject_area

status

current_version_id

created_at

updated_at

注意：

Source 与 Document 不完全等价。

例如：

一本教材是 Source。

某章节可以是 Document。

一个官方网站是 Source。

其中某份 PDF 是 Document。

---

# 5. KnowledgeDocumentVersion

所有导入内容必须版本化。

至少考虑：

id

document_id

version_number

content_hash

raw_storage_uri

normalized_storage_uri

parser_version

ingestion_pipeline_version

created_at

created_by

review_status

is_current

如果源文件改变：

不要直接覆盖旧版本。

---

# 6. KnowledgeChunk

建立正式 KnowledgeChunk persistence model。

至少包含：

id

document_version_id

chunk_index

stable_chunk_id

text

title_path

page_start

page_end

section_path

token_count

content_hash

authority_level

review_status

metadata

embedding_status

created_at

注意：

authority_level / review_status 最终应能够追溯到 Source/Document。

避免产生无法解释的冗余冲突。

如果做 denormalization：

请在文档中说明理由。

---

# 7. Chunking 原则

不要做单纯：

每 500 tokens 截断。

必须支持结构感知 chunking。

优先考虑：

heading

section

paragraph

table

question

solution

formula

reaction

list

未来 chemistry materials 中可能存在：

reaction scheme

chemical formula

table

diagram

equation

spectra

因此 Chunk Model 需要预留：

content_type

例如：

TEXT

TABLE

FORMULA

REACTION

QUESTION

SOLUTION

IMAGE_REFERENCE

MIXED

本轮不需要真正理解所有图片。

但是数据模型不能假设所有 Chunk 都是普通文本。

---

# 8. Parent / Child Chunk

建议考虑 hierarchical chunking。

例如：

Document
→ Section
→ Chunk

未来检索可以：

命中小 chunk

但返回更大的 parent context。

请设计：

parent_chunk_id

或等效机制。

不要过度复杂化。

如果你认为现在不需要真正启用：

可以只预留结构。

---

# 9. Metadata

ChemCoach Retrieval 的核心不是单纯 embedding。

每个 Chunk 至少应该可以过滤：

source_type

authority_level

review_status

publication_year

competition_stage

subject_area

knowledge_point

document_type

language

version

copyright_status

metadata schema 应尽量结构化。

不要把所有字段都塞进一个 arbitrary JSON。

JSON 只用于真正动态的补充字段。

---

# 10. KnowledgePoint Linking

KnowledgeChunk 应支持关联：

KnowledgePoint

many-to-many。

建立：

KnowledgeChunkKnowledgePoint

允许字段：

relevance

role

review_status

以后可以表达：

这段资料主要解释什么知识点？

是核心解释还是补充材料？

---

# 11. SolutionMethod Linking

KnowledgeChunk 也应能够关联：

SolutionMethod

例如一段内容可能专门讲：

电子守恒法

半反应法

逆合成分析

结构推断

建立适当关系模型。

不要用 tag string。

---

# 12. Ingestion Pipeline

建立明确 Pipeline：

Source Registration

→ File Intake

→ File Validation

→ Text Extraction

→ Normalization

→ Structure Detection

→ Chunking

→ Metadata Enrichment

→ Knowledge Linking

→ Review

→ Indexing

要求代码模块边界清晰。

建议：

knowledge/
  ingestion/
  parsers/
  chunking/
  enrichment/
  indexing/
  retrieval/
  evaluation/

具体结构可根据现有代码合理调整。

---

# 13. File Intake

本轮至少支持：

.md

.txt

可选支持：

.pdf

如果支持 PDF：

必须尽量保留：

page number

section position

不要只抽取纯文本后丢失页码。

如果 PDF 解析库产生低质量文本：

必须能够标记：

extraction_quality

不要假装解析完全正确。

---

# 14. Parser Interface

设计：

DocumentParser

例如：

parse(file) -> ParsedDocument

ParsedDocument 应尽量表达：

title

sections

blocks

page info

content type

metadata

不要让 parser 直接产生 embedding。

Parser 与 Indexing 解耦。

---

# 15. Normalization

建立 deterministic normalization。

例如：

Unicode normalization

whitespace cleanup

line-break normalization

heading cleanup

不要做：

LLM 自动“改写教材内容”。

原始资料不能因为 normalization 被改变含义。

必须保留：

raw content

normalized content

---

# 16. Chemistry Content Safety

化学资料有一些特殊内容：

化学式

上下标

箭头

平衡符号

氧化态

离子电荷

数学表达式

LaTeX

Normalization 不能破坏：

H₂SO₄

Fe³⁺

⇌

ΔG

Kₐ

E°

等符号。

请增加对应测试。

---

# 17. Chunking Strategy

实现至少两种策略：

STRUCTURE_AWARE

FIXED_WINDOW

默认：

STRUCTURE_AWARE

FIXED_WINDOW 仅作为 fallback。

Chunking 配置至少考虑：

target_tokens

min_tokens

max_tokens

overlap

preserve_sections

不要把配置硬编码在函数里。

---

# 18. Chunk Quality Validation

建立 ChunkValidator。

至少检查：

chunk 不为空

chunk 长度合理

来源存在

版本存在

chunk_index 唯一

content_hash 正确

metadata 合法

review_status 合法

authority_level 合法

必要时检查：

过短

过长

重复 chunk

---

# 19. Duplicate Detection

同一份资料可能重复上传。

至少使用：

content hash

检测完全重复。

未来可增加：

near duplicate。

本轮只需要 reliable exact duplicate detection。

---

# 20. Embedding Layer

现在正式启用 embedding abstraction。

但是：

不要让 embedding provider 写死。

沿用 AIProvider / EmbeddingProvider 抽象。

支持：

embedding(text)

模型从配置读取。

测试使用：

DeterministicEmbeddingProvider

不要依赖真实 OpenAI。

---

# 21. pgvector

正式实现：

PostgresVectorRetriever

使用 pgvector。

要求：

vector search

metadata filtering

top_k

minimum similarity threshold

可以工作。

同时保留：

InMemoryRetriever

用于测试。

---

# 22. Keyword Retrieval

必须实现关键词检索。

不要只做 vector search。

至少可以使用：

PostgreSQL full text search

或合理替代方案。

Chemistry 中很多内容例如：

Fe3+

KMnO4

NMR

E°

Ksp

精确关键词可能比 embedding 更可靠。

---

# 23. Hybrid Retrieval

实现：

HybridRetriever

组合：

keyword score

vector similarity

metadata filters

authority weight

review status weight

最终生成：

retrieval score

不要把权威性直接等同相关性。

推荐形式：

final_score =
relevance
+
authority adjustment
+
review adjustment

但具体公式由你设计并说明。

要求：

score 可解释。

---

# 24. RetrievalResult

Retriever 不要只返回 text。

正式定义：

RetrievalResult

至少包含：

chunk_id

document_id

source_id

text

score

vector_score

keyword_score

authority_level

review_status

document_version

page_start

page_end

section_path

matched_knowledge_points

warnings

这样未来 Solver 可以知道：

“这段知识从哪里来？”

---

# 25. Retrieval Filters

至少支持：

authority_level

review_status

subject_area

knowledge_point

competition_stage

publication_year

document_type

source_id

默认策略建议：

优先 HUMAN_VERIFIED

允许 AI_REVIEWED

谨慎 GENERATED

但是策略必须配置化。

---

# 26. Retrieval Policy

正式建立：

RetrievalPolicy

例如：

STRICT

BALANCED

EXPLORATORY

建议：

STRICT

只使用高可信资料。

未来正式 Tutor 默认应该倾向 STRICT。

BALANCED

允许专家/curated 内容。

EXPLORATORY

研究/扩展场景可允许更低可信内容。

Phase 03 可以先实现策略对象。

---

# 27. Insufficient Evidence

建立正式状态：

RetrievalStatus

SUCCESS

PARTIAL

INSUFFICIENT_EVIDENCE

NO_RESULTS

例如：

最大相关度低于阈值

或者

只有 GENERATED + UNREVIEWED 内容

则在 STRICT policy 下：

INSUFFICIENT_EVIDENCE

这项行为必须测试。

---

# 28. Citation / Provenance Object

设计统一：

ProvenanceReference

未来 Tutor 使用。

至少包括：

source title

document title

version

page

section

authority

review status

chunk id

禁止未来只返回：

“根据知识库”。

必须能够定位来源。

---

# 29. Ingestion Review Workflow

本轮建立最小审核流程。

状态：

INGESTED

NEEDS_REVIEW

AI_REVIEWED

HUMAN_VERIFIED

REJECTED

注意：

这和内容实体上的 ReviewStatus 需要保持语义一致。

如果你认为应该统一 Enum：

请统一并写 ADR。

---

# 30. CLI / Admin Script

本轮不需要后台管理界面。

实现 CLI 即可。

例如：

python -m chemcoach.knowledge.ingestion ingest path/to/file.md

参数支持：

--source-id

--document-type

--authority

--review-status

--subject-area

--competition-stage

禁止自动猜：

authority

copyright

human verified

这些必须显式指定。

---

# 31. Dry Run

CLI 支持：

--dry-run

用于展示：

解析结果

chunk 数量

metadata

warnings

重复检测

但不写数据库。

---

# 32. Knowledge Validation Report

每次 ingestion 应生成报告：

document

version

parser

chunk_count

duplicate_count

warnings

failed_chunks

index_status

review_status

方便未来知识管理员检查。

---

# 33. 真实资料策略

本阶段可以允许导入：

少量用户提供的、合法获得的材料。

但是 Codex：

不要自行抓取互联网。

不要自行下载 CChO 题库。

不要自行复制受版权限制教材。

不要批量生成“模拟官方知识”。

如果当前工作区没有真实资料：

继续使用明确标记的 SYNTHETIC / DEMO 文档完成开发与测试。

不要阻塞开发。

---

# 34. Demo Dataset

建立 3–5 份很小的 Synthetic Knowledge Documents。

例如：

DEMO：
氧化还原基础

DEMO：
平衡常数定义

DEMO：
酸碱平衡基础

DEMO：
电子守恒方法简介

明确：

SYNTHETIC_DATA

不要称其为：

CChO 官方资料。

---

# 35. Retrieval Evaluation

本轮必须建立：

Retrieval Evaluation Framework。

因为未来不能凭感觉判断 RAG。

定义：

RetrievalEvaluationCase

至少包括：

query

expected_document_ids

expected_chunk_ids 可选

expected_knowledge_points

policy

minimum_relevant_results

should_refuse

---

# 36. Evaluation Metrics

至少实现：

Recall@K

Precision@K

MRR

No-result / refusal accuracy

Source authority compliance

Review-status compliance

本轮数据量可以很小。

重点是框架。

---

# 37. Evaluation Cases

至少建立：

15 个 Synthetic Retrieval Cases。

覆盖：

普通关键词

同义表达

化学式

知识点过滤

authority filter

review filter

低相关查询

资料不存在

Generated + Unreviewed

跨知识点 query

要求包含：

should_refuse = true

的测试。

---

# 38. Retrieval Debugging

建立 debug mode。

可以看到：

query

keyword matches

vector matches

filter result

score composition

最终排序

为什么某个 chunk 被排第一。

这个功能未来非常重要。

可以做成内部 service/CLI。

不要暴露生产敏感信息。

---

# 39. Retrieval API

可以建立内部 API：

POST /api/v1/knowledge/search

输入例如：

{
  "query": "...",
  "top_k": 5,
  "policy": "STRICT",
  "filters": {}
}

输出：

status

results

warnings

但是：

这是开发/debug API。

不是用户 AI Tutor API。

如果你认为现在不需要 HTTP API：

可以只实现 service + CLI。

请说明理由。

---

# 40. Database Indexes

认真设计索引。

至少考虑：

source

document

version

chunk

knowledge relations

FTS

vector index

metadata filters

不要盲目给每列建索引。

在文档中解释主要索引。

---

# 41. Alembic

新增 migration。

要求：

Phase 01 + Phase 02 migration

可以正常升级到 Phase 03。

不要重写旧 migration。

除非存在明确错误。

migration 必须可重复验证。

---

# 42. Observability

记录 ingestion：

run_id

document_id

version

duration

parser

chunk_count

status

warnings

记录 retrieval：

query hash

policy

filters

top_k

duration

result count

status

不要默认记录完整敏感资料内容。

---

# 43. Security

File intake 至少检查：

允许扩展名

文件大小

路径 traversal

mime/type 基础验证

禁止执行上传文件。

Calculator 等旧组件不得受到影响。

---

# 44. Tests

Phase 03 新增完整测试。

至少包括：

Document versioning

exact duplicate detection

Markdown parsing

TXT parsing

PDF parsing（如果实现）

Unicode chemistry normalization

chemical formula preservation

structure-aware chunking

fixed chunk fallback

metadata validation

KnowledgePoint linking

embedding mock

keyword retrieval

vector retrieval

hybrid retrieval

metadata filtering

authority ranking

review ranking

STRICT policy

INSUFFICIENT_EVIDENCE

ProvenanceReference

CLI dry-run

migration

15+ retrieval evaluation cases

Phase 01 / Phase 02 regression tests

---

# 45. Documentation

新增：

docs/knowledge/

至少：

knowledge-ingestion.md

chunking-strategy.md

retrieval-architecture.md

hybrid-ranking.md

review-workflow.md

retrieval-evaluation.md

---

# 46. Architecture Diagram

更新系统架构：

Source
↓
Document Intake
↓
Parser
↓
Normalizer
↓
Chunker
↓
Metadata / Knowledge Linking
↓
Review
↓
Keyword Index
+
Vector Index
↓
Hybrid Retriever
↓
Retrieval Policy
↓
Retrieval Result
+
Provenance

使用 Mermaid。

---

# 47. 新 ADR

至少新增：

ADR-011
为什么采用 Hybrid Retrieval

ADR-012
为什么使用 Structure-aware Chunking

ADR-013
为什么 Source / Document / Version / Chunk 分层

ADR-014
为什么 Retrieval Policy 独立存在

ADR-015
为什么低相关结果应该拒绝而不是强行返回

---

# 48. Phase 03 明确不要做

不要：

Solver Agent

Tutor Agent

Answer Generation

Question Generator

Grader

Student Profile

完整 OCR

自动知识总结

自动生成“CChO知识点大全”

自动爬取竞赛网站

自动批量复制教材

复杂 Knowledge Graph 推理

不要为了展示效果提前进入 Phase 04。

---

# 49. Phase 03 成功标准

完成后：

即使没有 LLM 回答问题，

我们也应该可以输入：

“电子守恒”

系统返回：

最相关知识片段

来源

文档

页码/section

authority

review status

knowledge point

retrieval score

并解释为什么它排名靠前。

如果资料不存在：

例如查询一个知识库完全没有覆盖的问题，

系统应该返回：

INSUFFICIENT_EVIDENCE

而不是硬找一段不相关内容。

---

# 50. 最核心验收问题

Phase 03 完成后请逐项回答：

1.

KnowledgeChunk 是否可以追溯到：

Source
→ Document
→ Version
→ Chunk？

必须：

可以。

2.

Retriever 是否同时支持：

keyword
+
vector
+
metadata？

必须：

可以。

3.

是否支持 Hybrid Retrieval？

必须：

可以。

4.

是否可以按 authority / review status 过滤？

必须：

可以。

5.

GENERATED + UNREVIEWED 内容在 STRICT policy 下是否能自动成为高优先级 Ground Truth？

必须：

不能。

6.

低相关或无资料情况下是否支持：

INSUFFICIENT_EVIDENCE？

必须：

支持。

7.

是否有 Retrieval Evaluation？

必须：

有。

8.

系统目前是否已经具备完整真实 CChO 答疑能力？

正确答案：

仍然不具备。

因为当前只建立了知识导入和检索层。

还没有建立完整 Solver / Tutor / Verification 回答闭环。

---

# 51. 执行流程

现在请：

1. 阅读当前 Phase 01 / Phase 02 项目代码
2. 检查已有 KnowledgeRetriever 和 Domain Model
3. 输出 Phase 03 实施计划
4. 完成 Schema / ORM 设计
5. 建立 migration
6. 实现 ingestion pipeline
7. 实现 parser / normalizer / chunker
8. 实现 indexing
9. 实现 keyword retrieval
10. 实现 vector retrieval
11. 实现 hybrid retrieval
12. 实现 RetrievalPolicy
13. 实现 provenance
14. 建立 synthetic dataset
15. 建立 retrieval evaluation
16. 完成 CLI / debug tool
17. 更新 docs / ADR
18. 运行全部测试
19. 修复失败
20. 验证 Phase 01 / Phase 02 回归

不要跳过测试。

不要只写代码但不实际验证。

---

# 52. 完成报告

完成后输出：

A. Knowledge Engine 总体架构

B. 新增数据模型

C. Ingestion Pipeline

D. Chunking 策略

E. Retrieval 架构

F. Hybrid Ranking 规则

G. Retrieval Policy

H. Provenance 结构

I. Synthetic Knowledge Dataset

J. Evaluation Cases & Metrics

K. Migration 状态

L. 全部测试结果

M. Phase 01 / Phase 02 回归结果

N. 当前技术债

O. Phase 04 建议

另外请给出至少 3 个实际 retrieval 示例：

1.
正常成功检索

2.
metadata filter 改变结果

3.
INSUFFICIENT_EVIDENCE

并输出详细 debug ranking 信息。

---

# 53. 最后原则

Phase 03 的成功不是：

“我们接上了向量数据库。”

而是：

# 我们建立了一个可信、可追溯、可评估、会拒绝乱找资料的 CChO Knowledge Engine。

如果 Retriever 找错知识，

后面的 Solver 越聪明，

答案反而可能越危险。

所以这一阶段优先保证：

retrieval quality
>
feature quantity

现在开始执行 Phase 03。