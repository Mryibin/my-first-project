# ChemCoach CChO — Phase 05
# Knowledge Governance, Human Review & Real Benchmark Foundation

你正在继续开发同一个长期项目：

# ChemCoach CChO

当前已经完成：

## Phase 01 — Engineering Foundation
## Phase 02 — CChO Domain Model & Knowledge Ontology
## Phase 03 — Knowledge Ingestion & Retrieval Engine
## Phase 04 — Evidence-Grounded Solver, Verifier & Socratic Tutor

当前关键 checkpoints：

```text
checkpoint-phase-03
checkpoint-pre-phase-04
checkpoint-phase-04
```

当前 Phase 04 已建立：

Question Understanding
→ Retrieval Plan
→ Evidence Pack
→ Solver
→ Tools
→ StructuredSolution
→ Verifier
→ AnswerPolicy
→ Tutor
→ AnswerTrace

当前 synthetic evaluation 表现良好。

但是：

# 现在仍然不能宣称 ChemCoach 已经具备真正可靠的 CChO 专业答疑能力。

核心原因不是回答链路。

而是：

1. 缺少足够规模、合法来源的真实 CChO 专业资料
2. 缺少完善的 Human Review 工作流
3. 缺少真实 Benchmark
4. 缺少真实 CChO 数据上的系统性评测
5. 缺少可以长期治理知识库质量的机制

因此 Phase 05 的核心目标不是增加更多 AI 功能。

本轮核心目标：

# 把 ChemCoach 从“有可靠回答架构”推进到“开始拥有可信专业知识和真实评测能力”。

---

# 1. Phase 05 总目标

建立：

Knowledge Governance
+
Human Review
+
Benchmark Management
+
Evaluation Pipeline
+
Capability Baseline

最终形成：

Legal Source
↓
Source Registration
↓
Ingestion
↓
Review Queue
↓
Human Review
↓
Verified Knowledge
↓
Benchmark Dataset
↓
Answer Pipeline
↓
Evaluation
↓
Capability Report

---

# 2. 本轮最重要原则

## Principle A

真实资料不是：

“能下载到就能用”。

必须记录：

来源

版权状态

使用范围

审核状态

版本

---

## Principle B

Human Verified 不是一个普通字段。

必须可以回答：

谁审核的？

什么时候审核？

审核了哪个版本？

审核结论是什么？

为什么通过？

后来是否被撤销？

---

## Principle C

Benchmark ≠ Knowledge Base。

知识库是：

帮助系统回答的资料。

Benchmark 是：

用于考系统的题。

两者必须严格隔离。

---

## Principle D

Benchmark Question 不得因为进入知识库而造成 evaluation leakage。

---

## Principle E

Generated Question 不得进入“真实 Benchmark”。

除非明确分类：

SYNTHETIC / GENERATED BENCHMARK。

---

## Principle F

不能只看最终答案正确率。

必须拆解：

Retrieval

Solver

Tool

Verifier

Tutor

每一层。

---

## Principle G

不能让模型自己给自己打高分。

Benchmark evaluation 要尽可能：

deterministic
+
human reference
+
structured comparison

---

# 3. 本轮暂时不要做

不要：

大规模互联网爬虫

自动抓取整个 CChO 网站

自动下载大量版权资料

Question Generator

Exam Generator

自动批改整张试卷

Student Profile

Mistake Notebook

Adaptive Learning

OCR

Computer Vision

Fine-tuning

Multi-Agent

不要顺便进入 Phase 06。

---

# PART A — Knowledge Governance

# 4. Knowledge Governance Domain

在现有 KnowledgeSource / Document / Version / Chunk 基础上扩展治理能力。

至少新增概念：

SourceLicense

SourceUsagePolicy

ReviewTask

ReviewDecision

ReviewAuditEvent

Reviewer

KnowledgeApproval

KnowledgeIssue

ImportBatch

---

# 5. Source Classification

建立：

SourceType

至少：

OFFICIAL_COMPETITION

OFFICIAL_REGULATION

TEXTBOOK

REFERENCE_BOOK

ACADEMIC_PUBLICATION

EXPERT_NOTE

CURATED_NOTE

USER_PROVIDED

SYNTHETIC

GENERATED

OTHER

注意：

这不是 authority。

SourceType 与 AuthorityLevel 分开。

---

# 6. Copyright / Usage Governance

现有 CopyrightStatus 继续保留。

至少支持：

UNKNOWN

PRIVATE_USE

PERMISSION_GRANTED

PUBLIC_DOMAIN

LICENSED

RESTRICTED

如果现有 enum 已包含，不重复造轮子。

新增：

UsageScope

例如：

PRIVATE_STUDY

INTERNAL_EVALUATION

INTERNAL_DEVELOPMENT

COMMERCIAL_PRODUCT

REDISTRIBUTION

PUBLIC_DISPLAY

---

# 7. Usage Decision

建立明确：

UsageDecision

ALLOWED

ALLOWED_WITH_RESTRICTIONS

REVIEW_REQUIRED

PROHIBITED

UNKNOWN

不能根据：

authority = official

就自动推断：

commercial use allowed。

---

# 8. Source Registration

真实资料在进入 ingestion 前：

必须先完成 Source Registration。

至少登记：

source_title

source_type

publisher

author optional

original_url optional

publication_date optional

edition

jurisdiction optional

authority_level

copyright_status

usage_scope

usage_decision

license_note

provenance_note

registered_by

registered_at

---

# 9. Source Intake Gate

建立：

SourceIntakePolicy

如果：

copyright_status = UNKNOWN

并且目标 usage 包含：

COMMERCIAL_PRODUCT

则：

禁止自动进入 production verified knowledge。

可以：

PRIVATE_STUDY / INTERNAL_REVIEW

但必须 warning。

---

# 10. Import Batch

建立：

ImportBatch

用于一次资料导入。

字段至少：

id

name

created_at

created_by

source_ids

file_count

status

success_count

failed_count

duplicate_count

review_task_count

notes

---

# 11. Import Batch Status

CREATED

VALIDATING

INGESTING

AWAITING_REVIEW

PARTIALLY_REVIEWED

VERIFIED

REJECTED

FAILED

---

# PART B — Human Review

# 12. Reviewer Model

建立：

Reviewer

至少：

id

display_name

reviewer_type

status

created_at

---

# 13. Reviewer Type

至少：

OWNER

CHEMISTRY_EXPERT

CONTENT_REVIEWER

LEGAL_REVIEWER

SYSTEM_ADMIN

本轮不需要完整 RBAC。

但数据模型要支持未来：

不同审核职责。

---

# 14. Reviewer Identity

不能再使用简单：

review_status = HUMAN_VERIFIED

而不知道是谁审核。

所有 Human Verified 都必须能够追溯到 Reviewer。

---

# 15. Review Task

建立：

ReviewTask

target_type

target_id

target_version

review_type

priority

status

assigned_reviewer_id optional

created_at

started_at

completed_at

---

# 16. Review Type

至少：

CONTENT_ACCURACY

SOURCE_PROVENANCE

COPYRIGHT_USAGE

METADATA

CHUNK_QUALITY

KNOWLEDGE_LINKING

SOLUTION_ACCURACY

BENCHMARK_VALIDATION

---

# 17. Review Status

QUEUED

IN_REVIEW

APPROVED

APPROVED_WITH_CHANGES

REJECTED

NEEDS_INFORMATION

CANCELLED

---

# 18. Review Decision

ReviewDecision 至少：

review_task_id

reviewer_id

decision

decision_reason

confidence optional

changes_required

created_at

---

# 19. Review Audit Trail

建立：

ReviewAuditEvent

任何：

assignment

status change

approve

reject

reopen

withdraw approval

必须记录。

至少：

event_type

actor

timestamp

old_status

new_status

note

---

# 20. Human Verification Rule

Document / Chunk / Solution 等对象：

不能因为：

review_status 字段被 API 直接改成 HUMAN_VERIFIED

就完成审核。

必须通过：

ReviewTask
→ ReviewDecision
→ Approval

正式产生。

---

# 21. Knowledge Approval

建立：

KnowledgeApproval

至少：

target_type

target_id

target_version

approval_type

approved_by

approved_at

valid_from

valid_until optional

status

---

# 22. Approval Status

ACTIVE

SUPERSEDED

REVOKED

EXPIRED

---

# 23. Version Specific Review

审核必须绑定：

具体 version。

例如：

Document Version 1

被 Human Verified。

Document Version 2 出现后：

不能自动继承：

Human Verified。

新版本必须重新审核。

---

# 24. Review Promotion

建立明确 promotion：

UNREVIEWED
↓
AI_REVIEWED
↓
HUMAN_VERIFIED

但：

不得自动：

AI_REVIEWED → HUMAN_VERIFIED

必须有：

ReviewDecision。

---

# 25. Rejection

如果 review = REJECTED：

默认：

不能用于 STRICT_GROUNDED。

Retriever 必须排除。

---

# 26. Revocation

如果后来发现：

资料错误

版权问题

版本失效

必须可以：

revoke approval。

之后 Retrieval 必须立即不再把它作为 Verified Knowledge。

---

# 27. Review API

建立内部开发 API。

例如：

GET /api/v1/review/tasks

POST /api/v1/review/tasks/{id}/assign

POST /api/v1/review/tasks/{id}/decision

GET /api/v1/review/audit

不需要本轮做复杂前端 UI。

API + CLI 足够。

---

# 28. Review CLI

增加 CLI。

例如：

review list

review show

review approve

review reject

review reopen

必须：

有 reviewer identity。

---

# PART C — Knowledge Quality

# 29. Knowledge Quality Checks

在 Human Review 前提供 automated pre-check。

至少：

missing metadata

invalid provenance

copyright unknown

empty content

duplicate content

broken chemistry symbols

very small chunks

very large chunks

missing source linkage

missing version

unsupported review promotion

---

# 30. Knowledge Quality Score

可以建立：

KnowledgeQualityReport。

但：

不要用一个简单 0–100 数字掩盖问题。

优先：

checks + severity。

---

# 31. Quality Severity

INFO

WARNING

ERROR

BLOCKING

---

# 32. Verified Knowledge Rule

只有满足：

content review approved

provenance valid

usage policy allowed

version active

not revoked

才能：

进入 production-level verified pool。

---

# PART D — Benchmark Architecture

# 33. Benchmark 必须与 Knowledge Base 隔离

建立：

BenchmarkDataset

BenchmarkCase

BenchmarkCaseVersion

BenchmarkReference

BenchmarkRun

BenchmarkResult

BenchmarkMetric

---

# 34. Benchmark Dataset

字段至少：

id

name

description

benchmark_type

source_type

version

status

created_at

created_by

license_status

usage_scope

notes

---

# 35. Benchmark Type

至少：

SYNTHETIC

INTERNAL_CURATED

REAL_CCHO

TEXTBOOK_DERIVED

EXPERT_AUTHORED

---

# 36. Benchmark Status

DRAFT

UNDER_REVIEW

APPROVED

FROZEN

DEPRECATED

---

# 37. Frozen Benchmark

正式 evaluation 使用：

FROZEN benchmark。

Frozen 后：

不能直接修改。

修改必须：

创建新 version。

---

# 38. Benchmark Case

至少包含：

id

dataset_id

case_version

question_text

question_type

difficulty

source_reference

year optional

exam optional

question_number optional

expected_answer

reference_solution

knowledge_points

solution_methods

required_tools

acceptable_alternatives

grading_criteria

should_answer

expected_refusal_reason optional

assets optional

review_status

---

# 39. Benchmark Reference

Benchmark 的 expected answer：

不能只是一条字符串。

至少支持：

final_answer

required_claims

optional_claims

forbidden_claims

required_steps

accepted_methods

numeric_tolerance

unit_requirements

equation_requirements

---

# 40. Benchmark Review

真实 Benchmark case：

必须走：

BENCHMARK_VALIDATION

Human Review。

未经审核的真实题：

不能进入正式 baseline。

---

# 41. Benchmark Leakage Prevention

这是 Phase 05 P0。

必须设计：

BenchmarkLeakagePolicy。

正式 Benchmark：

不得被默认 ingestion 到 production KnowledgeRetriever。

建立：

benchmark_only = true

或独立 schema / repository / index。

优先：

逻辑和数据层都明确隔离。

---

# 42. Leakage Check

Evaluation 前执行：

BenchmarkLeakageCheck。

检查：

question exact hash

normalized hash

reference solution hash

document chunks

embedding near-duplicate

如果 Benchmark 内容出现在 Knowledge Base：

标记：

LEAKAGE_DETECTED。

---

# 43. Leakage Severity

EXACT_MATCH

NEAR_DUPLICATE

REFERENCE_ANSWER_MATCH

POSSIBLE_SEMANTIC_OVERLAP

---

# 44. Benchmark Run

建立：

BenchmarkRun

至少：

id

dataset_id

dataset_version

started_at

completed_at

system_version

git_commit

solver_version

verifier_version

prompt_versions

provider_models

retrieval_policy

grounding_policy

configuration

status

---

# 45. Reproducibility

每次正式 benchmark：

必须记录：

Git commit

Benchmark version

Prompt versions

Provider

Model

Temperature / deterministic config where applicable

Knowledge snapshot/version

Tool versions

---

# PART E — Evaluation Framework

# 46. Evaluation 不只看 final answer

建立分层：

StageEvaluation。

至少：

QUESTION_UNDERSTANDING

RETRIEVAL

EVIDENCE

SOLVER

TOOLS

VERIFIER

ANSWER_POLICY

TUTOR

FINAL

---

# 47. Retrieval Metrics

至少：

Recall@K

MRR

Relevant Evidence Coverage

Authority Compliance

Review Compliance

No-Result Precision

---

# 48. Solver Metrics

至少：

Final Answer Correctness

Required Claim Coverage

Unsupported Claim Rate

Method Correctness

Step Correctness

Tool Selection Accuracy

Calculation Accuracy

---

# 49. Verifier Metrics

至少：

Verification Catch Rate

False Pass Rate

False Block Rate

Critical Error Catch Rate

Unsupported Claim Catch Rate

Calculation Error Catch Rate

---

# 50. Answer Policy Metrics

至少：

Refusal Accuracy

False Refusal Rate

Unsafe Answer Release Rate

Clarification Accuracy

---

# 51. Tutor Metrics

至少：

Tutor Level Compliance

Final Answer Leakage

Knowledge Point Coverage

Citation Coverage

Common Error Relevance

---

# 52. Overall Capability

不要创建一个虚假的：

“AI Chemistry Score = 96”。

可以建立：

CapabilityReport

展示：

不同维度。

例如：

Retrieval

Reasoning

Calculation

Verification

Refusal

Teaching

---

# 53. Difficulty Breakdown

Benchmark report 至少按：

difficulty

question_type

knowledge_point

tool_required

multi_step

进行分组。

---

# 54. Failure Taxonomy

建立：

FailureCategory

至少：

QUESTION_MISUNDERSTANDING

RETRIEVAL_MISS

RETRIEVAL_NOISE

INSUFFICIENT_EVIDENCE

WRONG_METHOD

REASONING_ERROR

CALCULATION_ERROR

TOOL_ERROR

UNSUPPORTED_CLAIM

VERIFIER_FALSE_PASS

VERIFIER_FALSE_BLOCK

POLICY_ERROR

TUTOR_LEAKAGE

CITATION_ERROR

INPUT_MISSING

BENCHMARK_ISSUE

---

# 55. Root Cause

每个 failed BenchmarkResult：

至少可以标记：

primary_failure_category

secondary_failure_categories

notes

human_review_required

---

# PART F — First Real Benchmark Foundation

# 56. 不要一开始导入大量真实题

Phase 05 第一批真实 benchmark：

目标不是数量。

目标是：

流程正确。

如果用户已经合法拥有真实 CChO 题目/答案：

可以设计导入能力。

但是：

不要主动从互联网批量抓取。

---

# 57. Benchmark Import

建立：

benchmark import CLI/API。

支持：

Markdown

JSON

CSV optional

不要本轮做 PDF OCR。

---

# 58. Manual Benchmark Template

提供标准模板。

例如 JSON：

question

source

year

question_number

expected_answer

reference_solution

knowledge_points

difficulty

review metadata

---

# 59. Synthetic Benchmark 保留

现有 Phase 04：

30 synthetic cases

继续保留。

但分类必须：

SYNTHETIC。

不能和 Real Benchmark 混合报告。

---

# 60. Real Benchmark Zero-State

如果本轮没有用户提供合法真实资料：

Phase 05 仍然必须完成。

结果可以是：

REAL_CCHO benchmark cases = 0

这是可以接受的。

不要为了让数字好看：

偷偷抓题。

---

# 61. 示例 Curated Cases

可以创建少量：

INTERNAL_CURATED

chemistry benchmark。

但必须明确：

不是官方 CChO。

---

# 62. Benchmark Approval

只有：

APPROVED + FROZEN

dataset 才能用于：

正式 baseline。

---

# PART G — Knowledge Snapshot

# 63. Knowledge Snapshot

为了 Benchmark 可复现：

建立：

KnowledgeSnapshot。

至少：

id

created_at

source_versions

document_versions

chunk_count

approved_only

hash

---

# 64. Benchmark Run 绑定 Snapshot

正式 BenchmarkRun 必须记录：

knowledge_snapshot_id。

否则未来知识库变化后：

结果不可复现。

---

# PART H — Baseline Reporting

# 65. Capability Baseline Report

建立机器可读：

JSON

以及人类可读：

Markdown。

例如：

reports/benchmark/

---

# 66. Baseline Report

至少包含：

Run Metadata

Dataset

Knowledge Snapshot

Overall Results

Stage Metrics

Question Type Breakdown

Difficulty Breakdown

Failure Taxonomy

Top Failure Modes

Refusal Analysis

Verifier Analysis

Tutor Analysis

Leakage Check

Known Limitations

---

# 67. 不允许夸大结论

如果真实 benchmark：

只有 10 道题。

报告必须写：

small sample。

不能说：

“ChemCoach CChO accuracy = 90%”

然后暗示整体水平。

应该：

“在该 10-case benchmark 上……”

---

# PART I — First Knowledge Governance Demo

# 68. Demo A — Register Source

使用 synthetic/source fixture：

登记：

source

copyright

usage

authority。

---

# 69. Demo B — Review

Source / Document Version：

UNREVIEWED

→ ReviewTask

→ Reviewer decision

→ HUMAN_VERIFIED

展示完整 audit trail。

---

# 70. Demo C — New Version

Document Version 1：

Human Verified。

创建 Version 2。

验证：

Version 2 不自动继承 Human Verified。

---

# 71. Demo D — Reject

ReviewTask：

REJECT。

验证：

STRICT Retriever 不返回。

---

# 72. Demo E — Revoke

已 Approved 内容：

撤销 Approval。

验证：

下一次 STRICT retrieval：

不能使用。

---

# 73. Demo F — Copyright Restriction

Source：

RESTRICTED。

Usage：

COMMERCIAL_PRODUCT。

结果：

禁止 promotion 到 production verified pool。

---

# PART J — Benchmark Demo

# 74. Demo G — Create Dataset

创建：

synthetic benchmark dataset。

---

# 75. Demo H — Freeze Version

Benchmark：

DRAFT
→ APPROVED
→ FROZEN

冻结后：

禁止直接修改。

---

# 76. Demo I — Leakage

故意把 benchmark question/reference 加入 KB fixture。

Leakage Checker：

必须检测。

---

# 77. Demo J — Benchmark Run

执行：

至少 30 Phase 04 synthetic cases。

生成：

BenchmarkRun

BenchmarkResults

Metrics

CapabilityReport。

---

# 78. Demo K — Failure Taxonomy

故意插入：

retrieval miss

solver wrong calculation

verifier false pass mock

确认：

failure category

能正确记录。

---

# PART K — Security & Governance

# 79. Reviewer Action Security

虽然本轮不做完整 Auth：

Review APIs 不能匿名自动 approve。

至少在开发环境：

必须提供 reviewer identity。

---

# 80. Audit Immutability

ReviewAuditEvent：

原则上 append-only。

不要提供普通：

UPDATE audit log。

---

# 81. Benchmark Reference Protection

默认 API：

不要把：

expected_answer

reference_solution

直接暴露给 student-facing endpoint。

Benchmark 服务与学生 API 分离。

---

# PART L — Database

# 82. Alembic

为 Phase 05 新表：

创建正式 migration。

---

# 83. Indexes

至少考虑：

ReviewTask status

target type/id

Reviewer

Benchmark dataset/version

Benchmark case dataset

Benchmark run

failure category

source usage decision

---

# 84. Existing Data

Migration 必须：

兼容 Phase 01–04 当前数据库。

不能破坏：

AnswerRun

Knowledge data

Synthetic fixtures。

---

# PART M — API / CLI

# 85. Governance API

至少开发内部 API：

source registration

review queue

review decision

approval status

audit

---

# 86. Benchmark API

内部 API：

create dataset

add case

review case

freeze dataset

run benchmark

get report

---

# 87. CLI

推荐：

```text
chemcoach source register
chemcoach review list
chemcoach review approve
chemcoach review reject
chemcoach benchmark import
chemcoach benchmark validate
chemcoach benchmark freeze
chemcoach benchmark run
chemcoach benchmark report
```

具体命令形式适配现有架构。

---

# PART N — Tests

# 88. Governance Tests

至少：

source registration

usage policy

unknown copyright

commercial restriction

review task

review decision

human verification

version-specific review

reject

revoke

audit trail

reviewer identity

STRICT exclusion

---

# 89. Benchmark Tests

至少：

dataset lifecycle

case versioning

freeze immutable

benchmark review

leakage exact match

near duplicate

reference answer leak

snapshot

run reproducibility

failure taxonomy

metrics

report generation

---

# 90. Regression

重新执行：

Phase 01–04 所有 backend tests。

以及：

真实 PostgreSQL integration。

---

# PART O — Documentation

# 91. 新增：

docs/governance/

knowledge-governance.md

source-usage-policy.md

human-review-workflow.md

approval-revocation.md

---

# 92. 新增：

docs/benchmark/

benchmark-architecture.md

benchmark-leakage.md

benchmark-evaluation.md

failure-taxonomy.md

capability-reporting.md

---

# 93. ADR

至少增加：

ADR-024
Why Benchmark and Knowledge Base are isolated

ADR-025
Why Human Verification requires auditable reviewer identity

ADR-026
Why review is version-specific

ADR-027
Why approvals can be revoked

ADR-028
Why Benchmark datasets are frozen and versioned

ADR-029
Why benchmark runs bind to KnowledgeSnapshot

ADR-030
Why capability is reported by dimensions instead of one score

ADR-031
Why synthetic and real benchmark results must be separated

---

# PART P — Git

# 94. 开发前

确认：

checkpoint-phase-04

存在。

工作区 clean。

---

# 95. 当前 remote

如果：

main ahead origin/main

不要擅自 push。

本轮不要求远程操作。

---

# 96. Phase 05 Commit

全部通过后：

```text
feat: add knowledge governance and benchmark foundation
```

创建 annotated tag：

```text
checkpoint-phase-05
```

---

# PART Q — Phase 05 Acceptance Criteria

完成后逐项回答：

1.

Human Verified 是否必须通过 Reviewer + ReviewTask + ReviewDecision？

必须：

是。

2.

是否能知道：

谁审核了哪个 version？

必须：

是。

3.

新 Version 是否自动继承旧 Version Human Verified？

必须：

不能。

4.

Approval 是否可以 revoke？

必须：

可以。

5.

Rejected content 是否被 STRICT Retrieval 排除？

必须：

是。

6.

Copyright / Usage 是否独立于 Authority？

必须：

是。

7.

Restricted content 是否可以自动进入 commercial verified pool？

必须：

不能。

8.

Benchmark 是否和 Knowledge Base 隔离？

必须：

是。

9.

Benchmark 是否支持 Frozen Version？

必须：

是。

10.

Frozen benchmark 是否可以直接编辑？

必须：

不能。

11.

Benchmark evaluation 是否检查 leakage？

必须：

是。

12.

Benchmark Run 是否记录 Git commit / prompt / model / knowledge snapshot？

必须：

是。

13.

是否存在 Failure Taxonomy？

必须：

是。

14.

是否同时衡量 Solver 和 Verifier？

必须：

是。

15.

Synthetic 和 Real Benchmark 是否分开报告？

必须：

是。

16.

没有真实 CChO benchmark 时是否会伪造数据？

必须：

不会。

17.

当前是否可以宣称：

“ChemCoach 已达到可靠 CChO 竞赛水平”？

必须：

不能。

---

# PART R — 最终报告

完成后输出：

# A. Governance Architecture

---

# B. Source / Copyright / Usage

---

# C. Reviewer & Human Review

---

# D. Approval / Revocation

---

# E. Knowledge Quality

---

# F. Benchmark Architecture

---

# G. Leakage Prevention

---

# H. Knowledge Snapshot

---

# I. Evaluation Metrics

---

# J. Failure Taxonomy

---

# K. Capability Report

---

# L. Demo A–K

---

# M. Tests

Backend：

PostgreSQL integration：

TypeScript：

Next.js：

git diff --check：

---

# N. Database

Migration：

Tables：

Indexes：

---

# O. Git

Commit：

Tag：

Working tree：

---

# P. Real Data Status

明确说明：

真实 CChO Source 数：

真实 CChO Knowledge Document 数：

Human Verified 数：

真实 CChO Benchmark Case 数：

Synthetic Benchmark Case 数：

不要混在一起。

---

# Q. Technical Debt

---

# R. Phase 06 Recommendation

只推荐。

不要实现。

---

# S. 最终判断

回答：

ChemCoach 是否已经：

1. 有可信 Knowledge Governance？
2. 有 Human Review Audit Trail？
3. 有 Benchmark Isolation？
4. 有 Leakage Prevention？
5. 有可复现 Benchmark Run？
6. 有 Capability Baseline？

最后明确：

# PHASE 05 COMPLETE

或者：

# PHASE 05 NOT COMPLETE

并说明阻塞项。

---

# 最终产品原则

从 Phase 05 开始：

ChemCoach 不能只问：

“AI 能不能答对？”

而要同时问：

“它依据什么答？”

“资料是否合法？”

“谁审核过？”

“哪个版本被审核？”

“测试题有没有泄漏进知识库？”

“这次评测能不能复现？”

“错误到底发生在哪一层？”

“这个能力结论建立在多少真实样本上？”

只有这些问题都能回答，

ChemCoach 才开始从：

AI Demo

走向：

# 可治理、可审核、可评测的专业教育产品。

现在开始执行 Phase 05。