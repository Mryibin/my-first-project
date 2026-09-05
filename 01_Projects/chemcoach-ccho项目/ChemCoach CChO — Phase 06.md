# ChemCoach CChO — Phase 06
# Real CChO Corpus Pilot & Benchmark Calibration

你正在继续开发同一个长期项目：

# ChemCoach CChO

本阶段是项目非常重要的转折点。

此前 Phase 01–05 主要证明：

# 系统架构是否可靠。

Phase 06 开始要第一次回答：

# ChemCoach 在真实、合法、经过人工治理的 CChO 数据上，到底表现如何？

---

# 0. Current Project State

当前已经完成：

## Phase 01 — Engineering Foundation

## Phase 02 — CChO Domain Model & Knowledge Ontology

## Phase 03 — Knowledge Ingestion & Retrieval Engine

## Phase 04 — Evidence-Grounded Solver, Verifier & Socratic Tutor

## Phase 05 — Knowledge Governance, Human Review & Benchmark Foundation

当前关键 checkpoints：

```text
checkpoint-phase-03
checkpoint-pre-phase-04
checkpoint-phase-04
checkpoint-phase-05
```

Phase 05 已确认：

- Knowledge Governance 可运行
- Reviewer / ReviewTask / ReviewDecision / Approval 可运行
- Human Verification 绑定具体版本
- Approval 可 revoke
- Benchmark 与 Knowledge Base 隔离
- Benchmark Dataset 可审核、冻结和版本化
- Benchmark Leakage Detection 可运行
- KnowledgeSnapshot 可运行
- BenchmarkRun 可复现
- Capability Report 可生成
- Failure Taxonomy 已建立

Phase 05 验证：

- pytest：92 passed
- PostgreSQL migration：通过
- Benchmark synthetic cases：30
- synthetic baseline：27/30
- leakage check：clear
- KnowledgeSnapshot：0 approved chunks

当前真实数据状态：

REAL CChO Sources = 0

REAL CChO Knowledge Documents = 0

REAL CChO Benchmark Cases = 0

因此：

# 当前绝对不能宣称 ChemCoach 已具备可靠的真实 CChO 专业能力。

---

# 1. Phase 06 Mission

本阶段目标：

建立 ChemCoach 第一套：

# Small-Scale Real CChO Evaluation Loop

目标链路：

Legally Available Source
↓
Source Registration
↓
Rights / Usage Review
↓
Corpus Intake
↓
Knowledge Review
↓
Human Verified Knowledge
↓
Knowledge Snapshot
↓

Independent Benchmark Intake
↓
Benchmark Review
↓
Leakage Check
↓
Frozen Benchmark
↓

Answer Pipeline
↓
Benchmark Run
↓
Human Evaluation
↓
Failure Analysis
↓
Calibration
↓
Real Capability Baseline

---

# 2. Phase 06 Success Is NOT “More Data”

本阶段禁止把成功定义为：

“导入了 1000 道题。”

真正成功标准是：

# 我们拥有第一套可信、合法、可审核、无明显泄漏、可复现的真实能力评估流程。

即使最终只有：

3 个 Sources

20 个 Knowledge Documents

30 个 Benchmark Cases

也比：

1000 个来源不明的数据

更有价值。

---

# 3. Target Pilot Scale

本阶段建议目标：

## Real Sources

3–5 个。

## Human Verified Knowledge

目标：

20–50 个 Documents / Sections

或者：

足够形成至少 50–150 个高质量 verified chunks。

注意：

这是目标，不是为了数字硬凑。

## Real Benchmark

目标：

30–50 cases。

## Knowledge Domains

优先覆盖：

3–5 个代表性领域。

例如可以考虑：

Physical Chemistry

Inorganic Chemistry

Organic Chemistry

Analytical / Experimental Chemistry

Structure / Bonding

但：

# 不要假装这是官方 CChO 分类。

最终领域应根据用户实际提供的合法资料决定。

---

# 4. P0 Legal Boundary

这是 Phase 06 最重要规则。

# Codex 不得自行从互联网批量搜索、抓取、下载真实 CChO 试题、答案、教材、竞赛书籍或培训资料。

除非：

用户明确提供了资料

或者：

用户明确指定某个来源并授权进行处理。

---

# 5. Allowed Input

真实资料只能来自：

A.

用户手动放入指定 intake directory 的文件。

B.

项目已有、且 Governance 中已经登记合法 usage 的资料。

C.

用户明确指定并确认可使用的来源。

---

# 6. Forbidden Behavior

禁止：

自动 Google / Bing 搜题

自动爬中国化学会题库

自动抓培训机构题库

自动抓百度文库

自动抓 Scribd

自动抓网盘

自动抓论坛附件

自动抓微信公众号资料

自动从 GitHub 搜集未知版权竞赛资料

自动把互联网上能找到的题当训练资料

不要因为：

“公开可访问”

就推断：

“可以商业使用”。

---

# 7. Zero-Data Behavior

如果用户尚未提供真实资料：

# 不要阻塞 Phase 06 工程开发。

应该：

完成所有 Phase 06 pipeline

schema

CLI

API

review workflow

benchmark workflow

calibration framework

report framework

tests

synthetic fixtures

然后明确输出：

REAL DATA INTAKE PENDING

而不是：

偷偷寻找真实数据。

---

# PART A — Corpus Intake

# 8. Real Corpus Intake Directory

建立清晰目录，例如：

```text
data/
  intake/
    knowledge/
    benchmark/
```

或者适配当前项目结构。

必须：

默认 gitignored。

避免真实版权资料进入 Git。

---

# 9. Intake Manifest

每个真实资料 intake 必须有：

IntakeManifest。

至少包含：

file_name

source_title

source_type

source_reference

provided_by

intended_usage

copyright_status

usage_decision

authority_level

document_type

language

notes

---

# 10. No Manifest → No Production Import

真实文件没有 manifest：

不能直接：

HUMAN_VERIFIED

不能进入：

production verified knowledge。

允许：

QUARANTINED

等待补充 metadata。

---

# 11. Intake Status

建立：

RECEIVED

QUARANTINED

VALIDATING

READY_FOR_INGESTION

INGESTED

AWAITING_REVIEW

APPROVED

REJECTED

BLOCKED_BY_RIGHTS

FAILED

---

# 12. File Fingerprint

对 intake 文件计算：

SHA-256。

记录：

file_hash

size

mime

ingested_at

用于：

duplicate detection

provenance

benchmark leakage

audit。

---

# PART B — Corpus Curation

# 13. Corpus Curation

真实资料不能：

ingest → automatically verified。

必须：

ingest

→ quality checks

→ review tasks

→ human approval。

---

# 14. Corpus Unit

对真实资料定义可审核单元：

CorpusUnit。

例如：

Document

Section

Problem Explanation

Formula Reference

Concept Note

Official Regulation Section

不要默认整个 500 页 PDF：

一次 approve。

---

# 15. Review Granularity

优先：

section-level / logical-unit-level review。

Human Reviewer 应能确认：

内容准确

来源正确

metadata 正确

usage 允许

chunk 边界合理

knowledge point mapping 合理。

---

# 16. CorpusUnit Fields

至少：

id

source_id

document_id

document_version

section

page_range

content_type

authority

review_status

usage_decision

knowledge_points

hash

---

# 17. Verified Corpus

正式进入：

Verified Corpus

必须满足：

rights allowed

provenance valid

content reviewed

version approved

not revoked

quality gate passed。

---

# 18. Knowledge Snapshot

Phase 06 正式 benchmark：

必须创建：

REAL_CCHO_PILOT KnowledgeSnapshot。

记录：

approved source versions

approved document versions

approved chunk IDs

snapshot hash

created_at

review state

---

# PART C — Benchmark Intake

# 19. Benchmark Must Be Independent

Benchmark 资料：

不得和 Knowledge Corpus 使用同一个题目/答案文件直接复制。

Benchmark 应作为：

独立 evaluation asset。

---

# 20. Benchmark Intake Directory

例如：

```text
data/intake/benchmark/
```

默认：

gitignored。

---

# 21. Benchmark Manifest

至少：

source

year

exam

question_number

rights status

provided_by

question text

asset references

reference answer availability

reference solution availability

review status

---

# 22. Real Benchmark Import

支持：

JSON

Markdown

CSV optional

以及：

manual structured entry。

本轮：

# 不做 OCR。

如果资料是 PDF：

允许人工提取 / 已有文本。

不要自动进入 OCR Phase。

---

# 23. Benchmark Gold Reference

每个正式 case 尽可能建立：

GoldReference。

至少：

expected_final_answer

required_claims

accepted_alternative_answers

accepted_methods

required_conditions

numeric_tolerance

unit_tolerance

equation_constraints

grading_notes

---

# 24. Gold Reference Review

GoldReference：

必须独立 Human Review。

不能：

LLM 生成答案

→ 自动变 GoldReference。

---

# 25. Reference Missing

如果真实题没有可信 reference：

允许进入：

EXPLORATORY_SET

但不能进入：

GOLD_BENCHMARK。

---

# 26. Benchmark Tier

建立：

BenchmarkTier。

至少：

GOLD

SILVER

EXPLORATORY

---

# 27. GOLD

要求：

question verified

source verified

reference verified

rights usable

no known leakage

---

# 28. SILVER

允许：

question verified

reference 部分人工确认

但存在：

limited grading certainty。

---

# 29. EXPLORATORY

用于：

研究和 failure discovery。

不得与 GOLD accuracy 混合。

---

# PART D — Benchmark Leakage Prevention 2.0

# 30. Leakage Detection

在 Phase 05 基础上加强：

Exact Hash

Normalized Hash

Reference Answer Match

N-gram overlap

Near Duplicate

Embedding similarity

Metadata match

Source overlap

---

# 31. Source-Level Leakage

如果：

Knowledge Corpus

和：

Benchmark

来自同一份“题目 + 标准答案”资料，

即使文本 chunk 不完全相同，

也必须标记：

SOURCE_LEVEL_LEAKAGE_RISK。

---

# 32. Question Family Leakage

如果：

Benchmark question

和 Knowledge Base 中存在：

同题改数字

同题改变量名

高度近似题

必须标记：

QUESTION_FAMILY_OVERLAP。

---

# 33. Leakage Policy

正式 GOLD Benchmark：

遇到：

EXACT_MATCH

REFERENCE_ANSWER_MATCH

SOURCE_LEVEL_LEAKAGE

默认：

BLOCK RUN。

Near duplicate：

默认：

REVIEW_REQUIRED。

---

# 34. Leakage Report

正式 benchmark 前输出：

LeakageReport。

至少：

case_id

match_type

matched_chunk

similarity

source overlap

severity

decision

---

# PART E — Domain Coverage

# 35. Coverage Matrix

建立：

CoverageMatrix。

维度至少：

Knowledge Domain

Knowledge Point

Question Type

Difficulty

Tool Requirement

Multi-step

Evidence Requirement

---

# 36. Corpus Coverage

统计：

verified chunks

按 KnowledgePoint / Domain 分布。

---

# 37. Benchmark Coverage

统计：

benchmark cases

按：

domain

difficulty

question type

knowledge point

分布。

---

# 38. Coverage Gap

建立：

CoverageGap。

例如：

Benchmark：

Organic = 12 questions

Verified Corpus：

Organic = 0

则必须明确：

CORPUS_COVERAGE_GAP。

不能把失败都算：

Solver 不行。

---

# PART F — Benchmark Calibration

# 39. Calibration Goal

本阶段不是只跑：

accuracy。

要回答：

# 系统的 confidence 和实际正确性是否匹配？

---

# 40. Confidence Calibration

当前 ConfidenceLevel：

HIGH

MEDIUM

LOW

UNKNOWN

继续保留。

建立：

ConfidenceCalibrationReport。

---

# 41. Calibration Matrix

统计：

HIGH：

多少正确

多少错误

多少拒答

MEDIUM：

多少正确

多少错误

LOW：

多少正确

多少错误

---

# 42. Critical Rule

如果：

HIGH confidence

但：

Human Gold = Wrong

这是：

# HIGH_CONFIDENCE_ERROR

必须作为 P0 failure。

---

# 43. Calibration Metrics

至少：

High Confidence Accuracy

Medium Confidence Accuracy

High Confidence Error Count

Confidence vs Correctness Matrix

如果未来使用概率：

再考虑：

ECE / Brier Score。

本轮不要为了高级指标硬造概率。

---

# PART G — Human Evaluation

# 44. Human Evaluation

真实 Benchmark 不能完全由系统自己判。

建立：

HumanEvaluation。

至少：

benchmark_result_id

reviewer_id

final_correctness

method_correctness

major_error

minor_error

citation_quality

pedagogical_quality optional

failure_category

notes

reviewed_at

---

# 45. Correctness

至少：

CORRECT

PARTIALLY_CORRECT

INCORRECT

UNANSWERABLE

REFERENCE_ISSUE

---

# 46. Error Severity

NONE

MINOR

MAJOR

CRITICAL

---

# 47. Double Review Support

架构支持：

two independent reviewers。

本轮 pilot：

不要求所有题双审。

但建议：

至少：

全部错误 case

全部 HIGH_CONFIDENCE_ERROR

全部 Verifier PASS 但 Human Incorrect

进行第二次 review。

---

# 48. Reviewer Disagreement

建立：

ReviewerDisagreement。

记录：

reviewer A

reviewer B

disagreement fields

resolution

adjudicator optional

---

# PART H — Failure Analysis

# 49. Failure Attribution

每个真实错误：

不能只写：

wrong answer。

必须尽量归因。

---

# 50. Failure Pipeline

检查：

Question Understanding
↓
Retrieval
↓
Evidence
↓
Method Selection
↓
Reasoning
↓
Tool
↓
Verifier
↓
Policy
↓
Tutor

找：

first meaningful failure。

---

# 51. Primary Failure Category

沿用 Phase 05 taxonomy。

增加如有必要：

CORPUS_COVERAGE_GAP

GOLD_REFERENCE_ISSUE

CHEMISTRY_NOTATION_UNSUPPORTED

MULTI_STEP_REASONING_FAILURE

STRUCTURE_REASONING_FAILURE

EXPERIMENTAL_REASONING_FAILURE

---

# 52. First Failure Principle

例如：

Retriever 根本没找到关键资料

→ primary = RETRIEVAL_MISS

即使 Solver 后面答错：

不要只归因 Solver。

---

# 53. Verifier False Pass

这是 P0。

定义：

Solver 最终错误

+

Verifier PASS / APPROVE

+

答案被释放给 Tutor

=

VERIFIER_FALSE_PASS。

必须单独统计。

---

# 54. Verifier False Block

正确可答问题：

被 Verifier 错误阻断。

也要统计。

---

# PART I — Core Real Metrics

# 55. GOLD Benchmark Metrics

只对 GOLD 单独报告：

Answerable Accuracy

Final Answer Accuracy

Partial Correct Rate

Refusal Accuracy

False Refusal Rate

Unsafe Answer Release Rate

Verifier False Pass Rate

Verifier False Block Rate

Unsupported Claim Rate

Citation Validity

Tool Accuracy

High Confidence Accuracy

High Confidence Error Count

---

# 56. Answerable Accuracy

必须明确 denominator。

不要把：

正确拒答的不可回答题

混入普通 accuracy。

---

# 57. Unsafe Answer Release

定义：

Human = INCORRECT

+

系统 Decision = ANSWER / ANSWER_WITH_CAVEAT

+

Verifier 未阻断。

这是核心安全指标。

---

# 58. Pilot Sample Warning

如果：

GOLD < 100 cases

报告显著标注：

SMALL PILOT SAMPLE

不要泛化。

---

# PART J — Capability Report 2.0

# 59. Real Capability Report

生成：

JSON

+

Markdown。

---

# 60. Header

必须明确：

REAL CChO PILOT

或者：

NO REAL CChO DATA

不能把 synthetic 报告包装成真实结果。

---

# 61. Report Sections

至少：

Executive Summary

Data Provenance

Rights / Usage

Knowledge Corpus Size

Benchmark Composition

Coverage Matrix

Leakage Results

Overall GOLD Metrics

SILVER Metrics

Exploratory Findings

Confidence Calibration

Failure Analysis

Verifier Analysis

Retrieval Analysis

Tool Analysis

Human Review Summary

Known Limitations

Next Engineering Priorities

---

# 62. Sample Size

必须显示：

N。

例如：

GOLD N=32

而不是：

Accuracy = 84%

却不写样本量。

---

# PART K — Capability Gate

# 63. CapabilityGate

建立：

CapabilityGate。

不要让一次 benchmark 结束后：

自动宣布产品“可靠”。

---

# 64. Gate Status

NOT_EVALUATED

INSUFFICIENT_DATA

PILOT

LIMITED_CONFIDENCE

QUALIFIED

BLOCKED

---

# 65. Phase 06 Expected Status

即使 pilot 很好：

默认最多：

PILOT

或：

LIMITED_CONFIDENCE。

不要 Phase 06 就自动：

QUALIFIED。

---

# 66. Qualification

未来达到 QUALIFIED：

必须基于：

更大 benchmark

多领域覆盖

人工复核

低 verifier false pass

稳定版本

足够合法 verified corpus。

具体阈值：

不要本轮武断制定成行业标准。

---

# PART L — Real AI Provider Readiness

# 67. Current Solver

当前 Phase 04 Solver：

主要是 deterministic baseline。

Phase 06 可以建立：

RealAIProvider readiness。

但是：

# 不强制接入真实付费模型。

---

# 68. Provider Evaluation Interface

BenchmarkRunner 应允许比较：

deterministic baseline

vs

future real provider。

记录：

provider

model

prompt version

temperature/config。

---

# 69. No Silent Provider Change

同一个正式 benchmark run：

provider/model config 必须固定。

---

# PART M — Benchmark Execution Modes

# 70. Mode A

RETRIEVAL_ONLY

用于测试：

知识库和检索。

---

# 71. Mode B

SOLVER_ONLY_WITH_GOLD_EVIDENCE

直接提供人工选定 Evidence。

目的：

隔离 Solver 能力。

---

# 72. Mode C

END_TO_END

真实：

Question

→ Retrieval

→ Solver

→ Verifier

→ Tutor。

---

# 73. Why This Matters

例如：

END_TO_END = 50%

但：

SOLVER_WITH_GOLD_EVIDENCE = 85%

说明：

优先修 Retrieval / Corpus。

如果：

Gold Evidence 下仍 45%

说明：

Solver 本身才是瓶颈。

这是 Phase 06 最重要诊断能力之一。

---

# PART N — Ablation

# 74. Minimal Ablation

支持：

Retriever off / Gold Evidence

Verifier on/off

Tools on/off

仅限 evaluation environment。

---

# 75. Important

生产 Student API：

不能因为 ablation：

关闭安全 Verifier。

Ablation 只允许：

benchmark / test environment。

---

# PART O — Corpus & Benchmark CLI

# 76. Corpus CLI

建议：

```text
chemcoach corpus intake
chemcoach corpus validate
chemcoach corpus review-status
chemcoach corpus snapshot
chemcoach corpus coverage
```

适配现有 CLI。

---

# 77. Benchmark CLI

建议：

```text
chemcoach benchmark import-real
chemcoach benchmark leakage-check
chemcoach benchmark coverage
chemcoach benchmark run
chemcoach benchmark human-review
chemcoach benchmark calibrate
chemcoach benchmark capability-report
```

---

# PART P — Optional Internal Review UI

# 78. UI Priority

本轮：

UI 不是 P0。

如果现有 Next.js 很容易增加：

可以做极简 internal review page。

否则：

API + CLI 足够。

不要为了 UI：

拖慢 Phase 06。

---

# PART Q — Database

# 79. Migration

使用 Alembic 正式 migration。

需要支持：

IntakeManifest

CorpusUnit

Benchmark Gold Reference extension

HumanEvaluation

ReviewerDisagreement

Coverage

Calibration

CapabilityGate

以及合理的关联。

---

# 80. Backward Compatibility

不能破坏：

Phase 01–05 data。

必须：

upgrade

downgrade

test。

---

# PART R — Tests

# 81. Intake Tests

至少：

missing manifest

rights blocked

duplicate file

hash

quarantine

approved intake

---

# 82. Corpus Tests

至少：

review granularity

verified corpus

revoked exclusion

snapshot

coverage

---

# 83. Benchmark Tests

至少：

GOLD/SILVER/EXPLORATORY

GoldReference review

freeze

independent benchmark

leakage

source-level leakage

question-family overlap

---

# 84. Human Evaluation Tests

至少：

correct

partial

incorrect

reference issue

double review

disagreement

---

# 85. Calibration Tests

至少：

HIGH correct

HIGH wrong

MEDIUM correct

LOW wrong

HIGH_CONFIDENCE_ERROR。

---

# 86. Failure Attribution Tests

构造：

retrieval miss

corpus gap

solver reasoning error

tool error

verifier false pass

verifier false block

确认：

first meaningful failure。

---

# 87. Execution Mode Tests

验证：

RETRIEVAL_ONLY

SOLVER_ONLY_WITH_GOLD_EVIDENCE

END_TO_END。

---

# 88. Regression

运行：

所有 Phase 01–05 tests。

真实 PostgreSQL integration。

Next.js build。

TypeScript。

git diff --check。

---

# PART S — Pilot Execution

# 89. If Real Data Exists

如果用户已经将合法真实资料放入 intake：

执行：

真实 pilot。

---

# 90. Pilot Procedure

顺序必须：

Source Registration

→ Rights Validation

→ Intake

→ Quality Gate

→ ReviewTask

→ Human Approval

→ Corpus Snapshot

→ Benchmark Intake

→ Gold Review

→ Leakage Check

→ Freeze

→ Benchmark Run

→ Human Evaluation

→ Calibration

→ Capability Report

---

# 91. Human Review Cannot Be Faked

Codex：

不能冒充：

CHEMISTRY_EXPERT

然后把真实资料批量标记：

HUMAN_VERIFIED。

如果当前没有真实人工 reviewer 完成审核：

保持：

AWAITING_REVIEW。

---

# 92. Critical

AI 自动 review：

最多：

AI_REVIEWED。

不能：

HUMAN_VERIFIED。

---

# 93. If No Real Data Exists

执行完整 synthetic integration tests。

然后输出：

```text
REAL CChO CORPUS PILOT:
NOT STARTED

Reason:
No legally provided real corpus has been supplied.

Engineering readiness:
READY
```

这：

# 不算 Phase 06 工程失败。

---

# PART T — Human Review Workflow for Pilot

# 94. Review Packet

为了方便未来人工专家审核：

生成：

ReviewPacket。

每个 CorpusUnit / Benchmark Case：

包含：

source

provenance

rights

content

metadata

knowledge mapping

AI pre-check

review fields。

---

# 95. Export

支持导出：

JSON

Markdown

可选 CSV。

不要本轮强制 PDF。

---

# 96. Import Decisions

支持：

review decision import。

必须验证：

reviewer identity

target version

decision

timestamp

---

# PART U — No Benchmark Contamination

# 97. Critical Separation

Benchmark reference solution：

不得发送给：

Retriever

Solver

Tutor

除非：

SOLVER_ONLY_WITH_GOLD_EVIDENCE 模式明确使用的是：

Gold Evidence

而不是：

Gold Answer。

---

# 98. Gold Evidence vs Gold Answer

建立明确区别：

GoldEvidence

=

允许 Solver 使用的可信背景知识。

GoldAnswer

=

只用于评分。

绝对不能混淆。

---

# 99. Prompt Protection

BenchmarkRunner 必须确保：

expected_answer

reference_solution

forbidden_claims

grading notes

不进入 Solver prompt。

增加测试证明这一点。

---

# PART V — Metrics Integrity

# 100. No Cherry Picking

Capability Report：

必须记录：

all attempted GOLD cases。

不能只报告：

answered cases。

---

# 101. Failed Pipeline

如果 case 因：

system error

provider timeout

retrieval exception

失败，

必须记录：

SYSTEM_FAILURE。

不能从 denominator 偷删。

---

# 102. Report Versioning

Capability Report：

绑定：

BenchmarkRun ID

dataset version

knowledge snapshot

git commit。

---

# PART W — Phase 06 ADR

至少新增：

ADR-032
Why real corpus intake is quarantined before approval

ADR-033
Why benchmark gold answers never enter solver context

ADR-034
Why Gold/Silver/Exploratory benchmark tiers are separated

ADR-035
Why human evaluation is required for real benchmark calibration

ADR-036
Why confidence calibration tracks high-confidence errors

ADR-037
Why benchmark execution supports retrieval/gold-evidence/end-to-end modes

ADR-038
Why CapabilityGate prevents premature reliability claims

ADR-039
Why real copyrighted corpus files are gitignored by default

ADR-040
Why Codex/AI cannot self-assign HUMAN_VERIFIED

---

# PART X — Documentation

新增：

```text
docs/pilot/
real-corpus-intake.md
real-benchmark-intake.md
pilot-review-workflow.md
coverage-analysis.md
confidence-calibration.md
real-capability-report.md
```

更新：

README。

明确：

如何让用户把合法资料放入 intake。

---

# PART Y — Git

# 103. Before Work

确认：

checkpoint-phase-05

存在。

工作区：

clean。

---

# 104. Do Not Push

不要：

push

force push

rebase

reset --hard

git clean -fd。

---

# 105. Phase 06 Commit

工程实现和验证完成后：

```text
feat: add real corpus pilot and benchmark calibration
```

---

# 106. Tag

创建：

```text
checkpoint-phase-06
```

annotated tag。

---

# PART Z — Acceptance Tests

最终逐项回答：

1.

真实 corpus 是否必须经过 intake manifest？

必须：

是。

2.

没有明确 rights metadata 是否能直接进入 verified corpus？

必须：

不能。

3.

AI 是否可以把真实资料直接标记 HUMAN_VERIFIED？

必须：

不能。

4.

审核是否绑定具体 version？

必须：

是。

5.

Benchmark Gold Answer 是否与 Solver context 隔离？

必须：

是。

6.

Benchmark 是否检测 exact / near duplicate / source-level leakage？

必须：

是。

7.

正式 GOLD run 遇到严重 leakage 是否阻断？

必须：

是。

8.

是否存在 Corpus Coverage 和 Benchmark Coverage 对比？

必须：

是。

9.

是否能识别 CORPUS_COVERAGE_GAP？

必须：

是。

10.

是否支持 GOLD / SILVER / EXPLORATORY？

必须：

是。

11.

Gold Reference 是否需要 Human Review？

必须：

是。

12.

是否存在 HumanEvaluation？

必须：

是。

13.

是否支持 Reviewer disagreement？

必须：

是。

14.

是否单独统计 VERIFIER_FALSE_PASS？

必须：

是。

15.

是否单独统计 HIGH_CONFIDENCE_ERROR？

必须：

是。

16.

是否报告 Unsafe Answer Release Rate？

必须：

是。

17.

是否支持 RETRIEVAL_ONLY？

必须：

是。

18.

是否支持 SOLVER_ONLY_WITH_GOLD_EVIDENCE？

必须：

是。

19.

是否支持 END_TO_END？

必须：

是。

20.

Gold Answer 是否绝不会进入 Solver Prompt？

必须：

是。

21.

真实版权文件是否默认 gitignored？

必须：

是。

22.

没有真实合法资料时 Codex 是否不会自行上网抓取？

必须：

是。

23.

Synthetic 与 Real Benchmark 是否继续分开？

必须：

是。

24.

Capability Report 是否显示 N / sample size？

必须：

是。

25.

Phase 06 后是否自动宣称 ChemCoach 已达到可靠 CChO 水平？

必须：

不能。

---

# Phase 06 Final Report Format

完成后严格输出以下报告：

# A. Real Corpus Intake

Intake architecture：

Manifest：

Quarantine：

Rights gate：

---

# B. Corpus Governance

CorpusUnit：

Review：

Verified corpus：

Revocation：

---

# C. Real Data Status

必须精确报告：

Real CChO Sources：

Real CChO Documents：

Real CChO CorpusUnits：

Human Verified CorpusUnits：

Human Verified Chunks：

Knowledge Domains Covered：

不要把 synthetic 混进来。

---

# D. Benchmark Status

Real CChO Benchmark Datasets：

GOLD Cases：

SILVER Cases：

EXPLORATORY Cases：

Human Reviewed Cases：

Frozen Dataset：

---

# E. Leakage

Exact：

Near duplicate：

Source-level：

Question-family：

Blocked Cases：

---

# F. Coverage

Corpus Coverage：

Benchmark Coverage：

Coverage Gaps：

---

# G. Human Evaluation

Reviewed：

Second-reviewed：

Disagreements：

Resolved：

---

# H. Calibration

HIGH cases：

HIGH correct：

HIGH incorrect：

HIGH_CONFIDENCE_ERROR：

MEDIUM：

LOW：

---

# I. Core Metrics

仅真实 GOLD 数据存在时报告：

N：

Answerable Accuracy：

Final Answer Accuracy：

Partial Correct：

Refusal Accuracy：

False Refusal：

Unsafe Answer Release：

Verifier False Pass：

Verifier False Block：

Unsupported Claim：

Citation Validity：

Tool Accuracy：

---

# J. Execution Mode Comparison

RETRIEVAL_ONLY：

SOLVER_ONLY_WITH_GOLD_EVIDENCE：

END_TO_END：

说明瓶颈更可能在哪一层。

---

# K. Failure Analysis

Top Failure Categories：

First Meaningful Failure：

Corpus Gap：

Retrieval：

Solver：

Tools：

Verifier：

Policy：

Tutor：

---

# L. Capability Gate

Status：

必须是：

NOT_EVALUATED

INSUFFICIENT_DATA

PILOT

LIMITED_CONFIDENCE

QUALIFIED

BLOCKED

之一。

解释原因。

---

# M. Capability Report

JSON：

Markdown：

Run ID：

Knowledge Snapshot：

Dataset Version：

Git Commit：

---

# N. Tests

pytest：

PostgreSQL：

Alembic upgrade/downgrade：

TypeScript：

Next.js：

git diff --check：

---

# O. Git

Commit：

Tag：

Working tree：

Remote：

---

# P. Real vs Synthetic

明确分开：

REAL：

SYNTHETIC：

绝对不要混合 accuracy。

---

# Q. Technical Debt

列出真实剩余问题。

---

# R. Phase 07 Recommendation

根据真实 failure data 推荐下一阶段。

如果没有真实数据：

明确说：

Phase 07 engineering direction cannot yet be reliably selected from real failure data.

不要为了继续开发而随便选择。

---

# S. Final Judgment

最后回答：

1. Real corpus pipeline ready？
2. Rights gate ready？
3. Human review ready？
4. Real benchmark pipeline ready？
5. Leakage protection ready？
6. Human calibration ready？
7. Failure attribution ready？
8. Capability reporting ready？
9. Real CChO data actually evaluated？
10. Can ChemCoach claim reliable CChO capability？

如果没有真实数据：

第 9：

NO

第 10：

NO

最后输出：

如果工程完成但等待资料：

# PHASE 06 ENGINEERING COMPLETE — REAL DATA PILOT PENDING

如果真实 pilot 也完成：

# PHASE 06 REAL PILOT COMPLETE

如果关键机制缺失：

# PHASE 06 NOT COMPLETE

---

# Final Principle

Phase 01–05 解决的是：

“我们能不能造一个可靠系统？”

Phase 06 开始解决：

# “这个可靠系统，在真实化学竞赛问题上到底有多强？”

不要追求漂亮数字。

不要隐藏失败。

不要混合 synthetic 和 real。

不要让 benchmark 泄漏。

不要让 AI 冒充人工专家。

不要让 Gold Answer 进入 Solver。

不要把没有证据的成功包装成能力。

对于 Phase 06：

# 一个被正确诊断的失败，
# 比一个无法解释的高分更有价值。

现在开始执行 Phase 06。