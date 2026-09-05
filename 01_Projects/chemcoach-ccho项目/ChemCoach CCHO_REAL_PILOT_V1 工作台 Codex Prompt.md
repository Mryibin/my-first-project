# ChemCoach CChO — CCHO_REAL_PILOT_V1 Data Workbench

你正在继续开发现有项目：

ChemCoach CChO

当前已经完成：

- Phase 01–05
- Phase 06 Engineering
- commit: `850aeb5`
- tag: `checkpoint-phase-06`

当前真实状态：

- Real CChO Sources = 0
- Real CChO Benchmark = 0
- Human Verified Real Corpus = 0
- CapabilityGate = NOT_EVALUATED

现在不要继续开发新的 Solver、Generator、Grader、Student Profile 或 Agent。

本任务唯一目标：

# 为第一批真实 CChO Pilot 建立一个可以让我人工填资料、审核、导入和运行 Benchmark 的工作台。

Pilot 名称：

`CCHO_REAL_PILOT_V1`

---

# 1. Pilot Target

第一批计划：

- Real Benchmark：30 cases
- Development：10
- Calibration：10
- Holdout：10
- Human Reviewed Benchmark：100%
- Human Verified Knowledge：目标 60–100 chunks
- Knowledge Domains：先覆盖约 3 个领域

暂定领域：

1. Quantitative / Physical Chemistry
2. Inorganic / Redox
3. Organic / Structure / General Reasoning

这些只是 Pilot 标签，不得声明为官方 CChO 分类。

---

# 2. Create Pilot Directory Structure

在不破坏现有 Phase 06 架构的前提下，建立或确认以下逻辑目录。

真实版权文件必须继续 gitignored。

建议：

```text
data/
  intake/
    knowledge/
      open/
      authored/
      private/

    benchmark/
      real_ccho/
        source/
        extracted/

  pilot/
    ccho_real_pilot_v1/
      manifests/
      selection/
      review/
      gold/
      reports/
```

如果当前项目已经有等价目录，请复用，不要创建重复体系。

---

# 3. Git Safety

必须确认：

真实 CChO：

- PDF
- Word
- 图片
- 原题文本
- 参考答案
- 私有教材
- 培训资料

默认不能进入 Git。

更新 `.gitignore`，但不要误忽略：

- schema
- template
- empty README
- synthetic fixtures

执行测试证明真实 intake 文件不会被 Git track。

---

# 4. Create Knowledge Intake Manifest Template

创建模板：

`knowledge-intake-manifest.template.yaml`

字段至少包含：

```yaml
pilot_id: CCHO_REAL_PILOT_V1

file_name:

source_title:

source_type:

source_reference:

provided_by:

language: zh-CN

document_type:

authority_level:

copyright_status:

usage_scope:

usage_decision:

intended_usage:

notes:

file_size:

mime_type:

sha256:

review_status:
```

不得自动猜：

- copyright_status
- authority
- commercial permission
- HUMAN_VERIFIED

---

# 5. Create Benchmark Intake Manifest Template

创建：

`benchmark-intake-manifest.template.yaml`

至少：

```yaml
pilot_id: CCHO_REAL_PILOT_V1

source_title:

year:

exam_name:

question_number:

source_reference:

provided_by:

copyright_status:

usage_scope:
  - PRIVATE_STUDY
  - INTERNAL_EVALUATION

reference_answer_available:

reference_solution_available:

visual_dependency:

review_status:
```

---

# 6. Create Benchmark Selection Sheet

创建：

`ccho-real-pilot-v1-selection.csv`

字段：

```text
candidate_id
year
exam
question_number
domain
knowledge_points
difficulty
reasoning_depth
calculation_required
expected_tool
visual_dependency
evidence_dependency
reference_available
rights_checked
candidate_tier
pilot_split
selection_status
notes
```

枚举建议：

difficulty:

- EASY
- MEDIUM
- HARD

reasoning_depth:

- SINGLE_STEP
- MULTI_STEP
- LONG_CHAIN

visual_dependency:

- NONE
- LOW
- HIGH

candidate_tier:

- GOLD
- SILVER
- EXPLORATORY
- EXCLUDE

pilot_split:

- DEVELOPMENT
- CALIBRATION
- HOLDOUT
- UNASSIGNED

selection_status:

- CANDIDATE
- SELECTED
- REJECTED
- NEEDS_REVIEW

先生成空表，不得自行从互联网填真实题。

---

# 7. Create Knowledge Review Template

创建：

`knowledge-review.template.yaml`

人工 reviewer 至少确认：

```yaml
source_correct:
content_chemically_correct:
formula_correct:
conditions_complete:
knowledge_point_mapping_correct:
chunk_boundary_acceptable:
rights_acceptable:
major_issue:
minor_issue:
review_notes:
decision:
reviewer_id:
reviewed_at:
```

decision：

- APPROVE
- REJECT
- REQUEST_CHANGES

只有真实有效 Human Reviewer 可以产生最终 Human Verified 状态。

---

# 8. Create Benchmark Review Template

创建：

`benchmark-review.template.yaml`

至少：

```yaml
question_text_verified:
missing_image_or_table:
reference_answer_verified:
reference_solution_verified:
knowledge_points_verified:
accepted_alternative_answers:
accepted_methods:
numeric_tolerance:
unit_requirement:
equation_constraints:
difficulty_verified:
visual_dependency_verified:
leakage_reviewed:
review_notes:
decision:
reviewer_id:
reviewed_at:
```

---

# 9. Create Structured Benchmark Case Template

创建：

`benchmark-case.template.json`

结构至少：

```json
{
  "benchmark_id": "",
  "pilot_id": "CCHO_REAL_PILOT_V1",
  "benchmark_type": "REAL_CCHO",
  "tier": "GOLD",
  "split": "DEVELOPMENT",
  "year": null,
  "exam": "",
  "question_number": "",
  "domain": "",
  "difficulty": "",
  "reasoning_depth": "",
  "visual_dependency": "",
  "question_text": "",
  "knowledge_points": [],
  "expected_tools": [],
  "reference_status": "AWAITING_HUMAN_REVIEW"
}
```

Gold Answer / GoldReference 不得放在 Solver 可读取字段中。

---

# 10. Gold Reference Template

建立独立：

`gold-reference.template.json`

至少：

```json
{
  "benchmark_id": "",
  "expected_final_answer": "",
  "required_claims": [],
  "accepted_alternative_answers": [],
  "accepted_methods": [],
  "required_conditions": [],
  "numeric_tolerance": null,
  "unit_tolerance": null,
  "equation_constraints": [],
  "grading_notes": "",
  "review_status": "AWAITING_HUMAN_REVIEW"
}
```

必须继续保证：

GoldReference

不会进入：

Retriever

Solver Prompt

Tutor Prompt。

增加 regression test。

---

# 11. Pilot Split Guard

实现 Pilot Split 管理：

DEVELOPMENT

CALIBRATION

HOLDOUT

规则：

DEVELOPMENT：

允许反复运行和查看失败。

CALIBRATION：

允许用于 confidence / verifier / threshold calibration。

HOLDOUT：

默认禁止普通 development run。

只有明确指定：

`--allow-holdout`

且运行目的为：

FINAL_PILOT_EVALUATION

才允许执行。

记录：

holdout accessed at

run id

git commit。

目的：

防止开发过程中不断偷看 Holdout。

---

# 12. Candidate Selection Validation

提供 CLI：

```text
chemcoach pilot validate-selection CCHO_REAL_PILOT_V1
```

检查：

- 是否恰好或目标为 30 selected cases
- Development / Calibration / Holdout 是否约 10/10/10
- difficulty 分布
- domain 分布
- visual dependency
- tool requirement
- GoldReference availability
- rights metadata

在数据不足时只输出 warning，不得伪造数据。

---

# 13. Pilot Status Command

实现：

```text
chemcoach pilot status CCHO_REAL_PILOT_V1
```

输出：

Real sources

Knowledge documents

CorpusUnits

Human Verified CorpusUnits

Verified chunks

Benchmark candidates

Selected benchmark

Development

Calibration

Holdout

Gold reviewed

Leakage clear

Human reviewed

Ready for benchmark run

CapabilityGate

---

# 14. Pilot Import Commands

尽量复用 Phase 06 CLI。

目标体验：

```text
chemcoach pilot intake-knowledge CCHO_REAL_PILOT_V1

chemcoach pilot intake-benchmark CCHO_REAL_PILOT_V1

chemcoach pilot create-review-packets CCHO_REAL_PILOT_V1

chemcoach pilot import-review-decisions CCHO_REAL_PILOT_V1

chemcoach pilot leakage-check CCHO_REAL_PILOT_V1

chemcoach pilot create-snapshot CCHO_REAL_PILOT_V1
```

不要为了命令名字完全一致重构已有稳定代码。

---

# 15. Review Packet

生成适合人工审核的 Review Packet。

Knowledge packet：

- source
- rights
- content
- provenance
- knowledge points
- AI pre-check
- human fields

Benchmark packet：

- question
- source
- reference
- visual dependency
- proposed tags
- alternative answer
- tolerances
- leakage
- human fields

输出：

JSON

Markdown

可选 CSV。

---

# 16. Knowledge Card Template

创建：

`knowledge-card.template.md`

结构：

```markdown
# Knowledge Point

## Definition

## Core Principles

## Preconditions / Applicability

## Key Equations

## Reasoning / Solution Methods

## Common CChO Usage

## Common Errors

## Related Knowledge Points

## Provenance

## Review
```

AI 可以生成 draft。

AI draft：

只能：

UNREVIEWED / AI_REVIEWED

不能：

HUMAN_VERIFIED。

---

# 17. Pilot Readiness Gate

建立：

`PilotReadinessReport`

至少判断：

RIGHTS_READY

CORPUS_READY

BENCHMARK_READY

HUMAN_REVIEW_READY

LEAKAGE_READY

SNAPSHOT_READY

GOLD_READY

HOLDOUT_PROTECTED

最终：

NOT_READY

或：

READY_FOR_REAL_PILOT_RUN。

---

# 18. No Real Data Behavior

当前如果目录中没有真实资料：

不要上网搜索。

不要自动下载 CChO 题目。

不要生成假的 REAL_CCHO case。

完成工程后输出：

```text
CCHO_REAL_PILOT_V1

Workbench: READY
Real Corpus: 0
Real Benchmark: 0
Human Verified: 0

Status:
WAITING_FOR_USER_DATA
```

---

# 19. Tests

新增测试至少覆盖：

- real files gitignored
- missing manifest quarantine
- unknown rights cannot verified
- AI cannot Human Verified
- benchmark selection validation
- GoldAnswer prompt isolation
- Development allowed
- Calibration allowed
- Holdout blocked by default
- explicit final Holdout run
- Review packet creation
- Pilot readiness
- zero real data does not create fake cases

并运行所有现有 regression tests。

---

# 20. Validation

执行：

pytest

PostgreSQL integration

Alembic current

TypeScript

Next.js production build

git diff --check

确认 Phase 01–06 不被破坏。

---

# 21. Git

完成后：

commit：

`feat: add real CChO pilot data workbench`

tag：

`checkpoint-ccho-real-pilot-v1-workbench`

不要 push。

---

# 22. Final Report

输出：

## A. Workbench
created files and commands

## B. Git Safety
real file protection

## C. Knowledge Templates

## D. Benchmark Templates

## E. Selection Sheet

## F. Holdout Protection

## G. Review Workflow

## H. Pilot Readiness

## I. Tests

## J. Git

最后明确：

如果没有真实资料：

# CCHO_REAL_PILOT_V1 WORKBENCH READY — WAITING FOR USER DATA

不要宣称真实 Pilot 已开始。