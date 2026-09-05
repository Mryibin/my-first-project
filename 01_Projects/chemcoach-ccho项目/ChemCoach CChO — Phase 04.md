# ChemCoach CChO — Phase 04
# Evidence-Grounded Solver, Verifier & Socratic Tutor

你正在继续开发同一个长期项目：

# ChemCoach CChO

当前已经完成：

## Phase 01 — Engineering Foundation

包括：

- Next.js + TypeScript + Tailwind
- FastAPI
- PostgreSQL
- pgvector
- SQLAlchemy
- Alembic
- AIProvider abstraction
- KnowledgeRetriever abstraction
- Tool abstraction
- logging
- pytest
- Docker Compose

---

## Phase 02 — CChO Domain Model & Knowledge Ontology

已经建立：

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
- Authority / Review / Copyright
- Provenance
- Versioning

已经确认：

Question 支持多个 KnowledgePoint。

Solution 支持 step-level。

ErrorPattern 可以关联 SolutionStep。

Generated Solution 默认：

GENERATED
+
UNREVIEWED
+
not preferred

---

## Phase 03 — Knowledge Ingestion & Retrieval Engine

已经建立：

Source
→ Document
→ Version
→ Chunk

以及：

- Markdown / TXT ingestion
- normalization
- structure-aware chunking
- exact duplicate detection
- PostgreSQL FTS
- pgvector
- HNSW
- Hybrid Retrieval
- Metadata Filtering
- STRICT / BALANCED / EXPLORATORY policy
- Provenance
- Retrieval Evaluation
- NO_RESULTS
- INSUFFICIENT_EVIDENCE

真实 Docker + PostgreSQL + pgvector integration 已完成验证。

当前 checkpoint：

checkpoint-phase-03

以及：

checkpoint-pre-phase-04

当前 PostgreSQL / pgvector P0 integration debt 已消除。

---

# 1. 当前最重要限制

当前系统：

# 仍然没有真实、合法、人工审核的 CChO 专业知识库。

现有知识内容主要为：

SYNTHETIC / DEMO DATA。

因此 Phase 04：

可以建立完整 Solver / Verifier / Tutor 架构。

可以使用 synthetic chemistry cases 测试。

但是：

# 不得宣称已经具备真实 CChO 专业答疑能力。

不要偷偷生成所谓：

“官方 CChO 知识”

“官方竞赛答案”

“真实竞赛知识库”

---

# 2. Phase 04 核心目标

本轮建立 ChemCoach 第一个真正的 AI Answering Loop：

User Question
↓
Question Understanding
↓
Retrieval Planning
↓
Knowledge Retrieval
↓
Evidence Pack
↓
Solver
↓
Tool Execution
↓
Structured Solution
↓
Verifier
↓
Answer Decision
↓
Tutor
↓
Student-facing Response

核心目标不是：

# “让 LLM 回答化学问题。”

而是：

# “建立一个证据驱动、工具辅助、经过验证、可以拒答、可以分层教学的化学解题系统。”

---

# 3. 核心架构原则

必须严格遵守：

## Solver ≠ Tutor

Solver：

负责求解。

Tutor：

负责教学。

---

## Solver ≠ Verifier

Solver：

提出答案。

Verifier：

独立检查答案。

---

## Retrieval ≠ Answer

Retriever：

只负责提供证据。

不能直接把 RetrievalResult 当答案。

---

## Model Memory ≠ Ground Truth

LLM 自身预训练知识不能自动视为权威事实来源。

---

## Tool Result ≠ Model Guess

确定性计算结果必须与模型自然语言推导区分。

---

# 4. 建立 Answer Orchestrator

建立：

AnswerOrchestrator

负责整个回答生命周期。

建议：

QuestionUnderstanding
↓
RetrievalPlanner
↓
EvidenceBuilder
↓
Solver
↓
ToolExecutor
↓
Verifier
↓
AnswerPolicy
↓
Tutor

Orchestrator 不应自己承担具体化学推理。

它负责：

流程

状态

调用顺序

错误处理

日志

trace

---

# 5. Answer Run

建立正式：

AnswerRun

用于追踪一次完整回答。

至少包含：

id

question_input

question_id optional

started_at

completed_at

status

retrieval_policy

tutor_level

ai_provider

model_identifier

prompt_version

solver_version

verifier_version

confidence

final_decision

warnings

不要默认保存隐藏 chain-of-thought。

---

# 6. AnswerRun Status

建议：

CREATED

UNDERSTANDING

RETRIEVING

SOLVING

VERIFYING

TUTORING

COMPLETED

REFUSED

FAILED

必须支持：

partial failure。

例如：

retrieval 成功

solver 成功

verifier 拒绝

最终：

REFUSED

---

# 7. Question Understanding

建立：

QuestionUnderstandingService

输入：

raw user question

输出：

StructuredQuestionUnderstanding

至少包括：

normalized_question

question_type

detected_domain

detected_subdomains

candidate_knowledge_points

candidate_skills

quantities

units

chemical_entities

equations

constraints

requested_output

ambiguities

requires_calculation

requires_structure_reasoning

requires_external_evidence

confidence

warnings

注意：

这是 AI 对题目的“理解”。

不是 Ground Truth。

必须标记：

inferred / detected。

---

# 8. Question Type

建立受控 enum。

例如：

CONCEPTUAL

QUANTITATIVE

REACTION

EQUILIBRIUM

THERMODYNAMICS

ELECTROCHEMISTRY

STRUCTURE

ORGANIC_MECHANISM

INORGANIC_REASONING

ANALYTICAL

EXPERIMENTAL

MULTI_PART

MIXED

OTHER

这只是内部分类。

不要宣称这是 CChO 官方分类体系。

---

# 9. Ambiguity Detection

Question Understanding 必须能够表达：

题目不完整

条件缺失

单位缺失

图像缺失

表格缺失

反应式缺失

上下文缺失

例如：

“根据下图判断结构”

但是没有图。

系统必须识别：

MISSING_ASSET

而不是开始瞎猜。

---

# 10. Question Readiness

建立：

QuestionReadiness

READY

PARTIALLY_READY

INSUFFICIENT_INPUT

如果：

题目关键条件缺失

则：

禁止 Solver 构造伪完整答案。

---

# 11. Retrieval Planner

建立：

RetrievalPlanner

不要简单把原问题全文直接扔给 Retriever。

根据 QuestionUnderstanding 构造：

RetrievalPlan

至少包含：

primary_query

secondary_queries

knowledge_point_filters

subject_filters

method_filters

authority requirements

review requirements

retrieval_policy

top_k

minimum_evidence

---

# 12. Query Expansion

允许有限的：

query expansion。

例如：

题目：

“为什么加入少量酸会影响平衡？”

可能生成：

primary query

以及：

acid-base equilibrium

Le Chatelier

equilibrium constant

但是：

扩展 query 必须被记录。

不能隐藏。

---

# 13. Evidence Pack

这是 Phase 04 最重要的新对象之一。

建立：

EvidencePack

不要直接把 RetrievalResult[] 粗暴塞进 Prompt。

EvidencePack 至少包含：

question_understanding

retrieval_status

evidence_items

conflicts

coverage

missing_evidence

authority_summary

review_summary

warnings

---

# 14. Evidence Item

建立：

EvidenceItem

至少：

id

chunk_id

source_id

document_id

document_version

text

relevance_score

authority_level

review_status

knowledge_points

solution_methods

page

section

provenance

allowed_usage

---

# 15. Evidence Usage

建立：

EvidenceUsageType

例如：

FACT

DEFINITION

FORMULA

METHOD

CONSTRAINT

REFERENCE_SOLUTION

SUPPORTING_CONTEXT

注意：

EvidenceBuilder 可以推断 usage。

但是必须标记：

inferred classification。

---

# 16. Evidence Coverage

建立：

EvidenceCoverage

COMPLETE

PARTIAL

WEAK

NONE

不能只看 retrieval top score。

应综合：

question needs

knowledge points

required facts

method support

authority

review

---

# 17. Evidence Conflict

如果多个 EvidenceItem 出现：

定义冲突

公式冲突

条件冲突

版本冲突

必须记录：

EvidenceConflict

至少：

items

conflict_type

description

severity

resolution_status

不要让 Solver 静默选择一个。

---

# 18. Grounding Rules

正式建立：

GroundingPolicy

至少：

STRICT_GROUNDED

GUIDED_REASONING

EXPLORATORY

正式学生 Tutor 默认：

STRICT_GROUNDED

---

# 19. STRICT_GROUNDED

在 STRICT_GROUNDED 下：

关键外部事实必须有 Evidence 支撑。

允许：

逻辑推导

数学推导

确定性 Tool 计算

但是：

不能把 LLM 记忆中的专业事实偷偷作为 Ground Truth。

如果缺关键证据：

必须降低 confidence。

必要时：

REFUSE / REQUEST_MORE_INFORMATION。

---

# 20. GUIDED_REASONING

允许：

基于可靠 Evidence

+

模型进行合理推导。

但必须区分：

SUPPORTED_FACT

DERIVED_REASONING

TOOL_RESULT

MODEL_ASSUMPTION

---

# 21. Claim Model

建立：

Claim

这是可靠性核心。

每个重要结论可以表示：

id

text

claim_type

supporting_evidence_ids

supporting_tool_call_ids

depends_on_claim_ids

confidence

verification_status

warnings

---

# 22. Claim Type

至少：

SUPPORTED_FACT

DERIVED_REASONING

TOOL_RESULT

ASSUMPTION

FINAL_CONCLUSION

如果一个事实没有 Evidence：

不能标记：

SUPPORTED_FACT。

---

# 23. Solver

建立：

SolverService

输入：

StructuredQuestionUnderstanding

EvidencePack

available_tools

GroundingPolicy

输出：

StructuredSolution

---

# 24. StructuredSolution

至少包含：

solution_id

question_understanding_id

summary

steps

claims

final_answer

assumptions

evidence_used

tools_used

confidence

solver_status

warnings

---

# 25. Solution Step

Phase 04 Solver 输出必须复用/兼容 Phase 02 的 SolutionStep 思想。

每一步至少：

step_number

title

explanation

reasoning_type

knowledge_points

method

claims

evidence_ids

tool_calls

intermediate_result

confidence

warnings

不要输出隐藏 chain-of-thought。

这里的 explanation 是：

# 可展示的解题依据 / concise rationale

不是模型内部隐式推理 token。

---

# 26. Reasoning Type

例如：

FACT_APPLICATION

FORMULA_SELECTION

DERIVATION

CALCULATION

REACTION_REASONING

STRUCTURE_REASONING

CONSTRAINT_APPLICATION

CONCLUSION

---

# 27. Solver Status

SUCCESS

PARTIAL

INSUFFICIENT_EVIDENCE

INSUFFICIENT_INPUT

TOOL_ERROR

UNVERIFIED

FAILED

---

# 28. Tool Layer Upgrade

沿用 Phase 01 Tool abstraction。

本轮正式建立：

ToolRegistry

ToolExecutor

ToolCall

ToolResult

---

# 29. ToolCall

至少记录：

tool_name

tool_version

input

output

status

duration

deterministic

error

不要把工具结果混入模型文本后无法追踪。

---

# 30. Calculator

继续使用现有安全 Calculator。

必须：

禁止 eval

禁止任意代码执行。

---

# 31. 本轮增加 Chemistry Deterministic Tools

至少实现两个低风险、高确定性的 chemistry tools：

## MolarMassCalculator

输入：

chemical formula

输出：

molar mass

element breakdown

warnings

---

## EquationBalanceChecker

输入：

chemical equation

输出：

atom balance

charge balance

balanced yes/no

warnings

注意：

优先做：

checker

而不是自动万能 equation solver。

---

# 32. 可选 Tool

如果时间和架构允许：

UnitConverter

但不是必须。

不要本轮扩展成十几个 chemistry tools。

---

# 33. Tool Reliability

每个 deterministic tool：

必须有完整 unit tests。

尤其：

chemical formula parser

parentheses

stoichiometric coefficients

charges

hydrates

invalid formula

如果复杂情况暂不支持：

必须明确返回：

UNSUPPORTED

不能猜。

---

# 34. Solver Tool Use

Solver 如果遇到：

算术

摩尔质量

方程守恒检查

优先使用 deterministic tool。

不要让 LLM 自己“心算”后冒充工具结果。

---

# 35. Verifier

建立：

VerifierService

这是 Phase 04 最重要模块之一。

Verifier 必须与 Solver 逻辑分离。

输入：

QuestionUnderstanding

EvidencePack

StructuredSolution

ToolResults

GroundingPolicy

输出：

VerificationReport

---

# 36. Verifier 不等于“再问一次同一个模型”

Verifier 必须执行结构化检查。

至少包括：

Evidence Verification

Claim Verification

Calculation Verification

Tool Consistency

Step Consistency

Final Answer Consistency

Input Completeness

Grounding Policy Compliance

---

# 37. Verification Check

建立：

VerificationCheck

至少：

check_type

status

severity

target_id

message

evidence_ids

tool_call_ids

---

# 38. Verification Status

PASS

WARNING

FAIL

NOT_CHECKED

---

# 39. Verification Severity

INFO

LOW

MEDIUM

HIGH

CRITICAL

---

# 40. Evidence Verification

检查：

SUPPORTED_FACT 是否真的有 evidence_id。

Evidence 是否存在。

Evidence 是否被 policy 允许。

Evidence review / authority 是否满足策略。

如果：

SUPPORTED_FACT

没有 evidence：

HIGH 或 CRITICAL。

---

# 41. Calculation Verification

如果 Step 包含 calculation：

Verifier 应：

尽量通过 deterministic Calculator 重新计算。

检查：

expression

intermediate result

final result

是否一致。

---

# 42. Equation Verification

如果答案包含 chemical equation：

尽量调用：

EquationBalanceChecker。

如果不守恒：

FAIL。

---

# 43. Claim Dependency Verification

如果 Final Conclusion 依赖某个：

FAILED claim

则：

Final Conclusion 不能 PASS。

---

# 44. Assumption Verification

如果 Solver 引入了关键 assumption：

必须显式存在于：

StructuredSolution.assumptions

不能偷偷假设。

关键 assumption 如果无法支持：

Verifier 应 WARNING / FAIL。

---

# 45. Independent Verification

如果使用 AI 做语义层验证：

建立独立：

Verifier Prompt

不要复用 Solver Prompt。

如果 AIProvider 支持配置不同模型：

架构应允许：

solver_model

verifier_model

不同。

但本轮测试仍可使用 deterministic/mock provider。

不要写死某一家模型。

---

# 46. Verification Report

至少：

overall_status

checks

unsupported_claims

calculation_errors

evidence_issues

conflicts

critical_failures

confidence_adjustment

recommended_action

---

# 47. Recommended Action

APPROVE

APPROVE_WITH_WARNINGS

REQUEST_MORE_INFORMATION

RETRY_SOLVER

REFUSE

HUMAN_REVIEW

---

# 48. Answer Policy

建立：

AnswerPolicyEngine

根据：

QuestionReadiness

RetrievalStatus

EvidenceCoverage

SolverStatus

VerificationReport

GroundingPolicy

决定：

AnswerDecision

---

# 49. Answer Decision

ANSWER

ANSWER_WITH_CAVEAT

HINT_ONLY

REQUEST_CLARIFICATION

REFUSE

HUMAN_REVIEW

---

# 50. Hard Blocking Rules

至少实现以下硬规则：

## Rule A

QuestionReadiness = INSUFFICIENT_INPUT

→

REQUEST_CLARIFICATION

---

## Rule B

STRICT_GROUNDED

+

RetrievalStatus = INSUFFICIENT_EVIDENCE

且问题需要外部专业事实

→

REFUSE

或者

HINT_ONLY

不得输出伪完整专业答案。

---

## Rule C

VerificationReport 存在 CRITICAL FAIL

→

禁止 ANSWER。

---

## Rule D

Final Answer calculation verification failed

→

禁止 ANSWER。

---

## Rule E

关键 chemical equation balance failed

→

禁止作为 VERIFIED ANSWER 输出。

---

# 51. Confidence

建立统一：

ConfidenceLevel

HIGH

MEDIUM

LOW

UNKNOWN

不要使用虚假的：

97.3%

除非真的存在经过校准的概率模型。

---

# 52. Confidence 计算原则

综合：

input completeness

evidence coverage

authority

review status

tool verification

claim verification

conflicts

solver confidence

但不要让 Solver 自己决定最终 confidence。

最终 confidence 应由：

AnswerPolicy / Verifier

综合产生。

---

# 53. Tutor

建立：

TutorService

Tutor 输入：

StructuredSolution

VerificationReport

AnswerDecision

StudentRequest

TutorLevel

Tutor 不重新解题。

Tutor 负责：

# 将已验证 Solution 转换为教学表达。

---

# 54. Tutor Levels

正式实现：

LEVEL_1

只给提示。

不透露关键步骤和最终答案。

---

LEVEL_2

知识点

+

方向

+

关键切入点

仍然尽量不直接给最终答案。

---

LEVEL_3

给主要解题步骤。

保留部分学生自己完成的空间。

---

LEVEL_4

完整讲解：

知识点

方法选择

步骤

计算

最终答案

易错点

---

# 55. 默认 Tutor Level

默认：

LEVEL_2

不要默认 LEVEL_4。

因为产品目标是：

教会学生

而不是：

代替学生做题。

---

# 56. Socratic Tutor

LEVEL_1 / LEVEL_2 应优先采用：

问题引导

例如：

“这一步你觉得应该先守恒哪一种量？”

而不是：

“答案是……”

但是：

不要强行每句话都变成问题。

要自然。

---

# 57. Tutor Output

建立：

TutorResponse

至少：

decision

tutor_level

message

knowledge_points

method

hints

common_errors

next_question

citations

confidence

warnings

---

# 58. Citation

LEVEL_4 完整解答应该能够附：

ProvenanceReference。

LEVEL_1/2 可以减少 citation 展示密度。

但内部仍应保留：

evidence → claim → solution step

映射。

---

# 59. Student-Facing Transparency

如果：

evidence weak

或者：

verification warning

Tutor 应使用自然语言表达：

“现有资料不足以可靠确认这一点。”

而不是输出内部术语：

INSUFFICIENT_EVIDENCE

给学生。

内部状态与学生语言分离。

---

# 60. Clarification

如果题目缺失：

图片

条件

数值

单位

选项

要求

Tutor 应主动请求：

具体缺失信息。

例如：

“这道题引用了一个结构图，但当前没有收到图片，请补充题图。”

---

# 61. Prompt Architecture

建立版本化 Prompt：

question_understanding

retrieval_planner

solver

verifier

tutor

每个 prompt：

有 version。

不要把所有逻辑写成一个超级 prompt。

---

# 62. Prompt Storage

建议：

prompts/

question_understanding/

solver/

verifier/

tutor/

或者当前项目合理目录。

Prompt 必须可测试、可版本化。

---

# 63. Structured Output

AIProvider 应优先使用：

generate_structured

结合 Pydantic schema。

不要依赖：

“请严格返回 JSON”

然后手写 fragile parser。

如果当前 Mock Provider 需要升级：

保持 backward compatibility。

---

# 64. Model Provider Independence

不要写死：

OpenAI

Claude

Gemini

Qwen

DeepSeek

具体模型。

继续使用：

AIProvider abstraction。

配置：

solver provider/model

verifier provider/model

tutor provider/model

允许未来不同。

---

# 65. Answer Trace

建立：

AnswerTrace

至少记录：

run_id

question understanding

retrieval plan

retrieval result IDs

evidence pack

tool calls

solver output

verification report

answer decision

tutor output

prompt versions

provider/model identifiers

timings

---

# 66. 不保存隐藏 Chain-of-Thought

禁止设计：

raw_chain_of_thought

hidden_reasoning

internal_monologue

字段。

只保存：

structured rationale

claims

solution steps

evidence mapping

tool results

verification checks

---

# 67. Persistence

Phase 04 至少考虑 persistence：

AnswerRun

ToolCall

VerificationReport

最终 TutorResponse

是否需要完整 ORM persistence：

由现有架构决定。

但必须保证：

未来可以复盘一次错误回答：

它理解了什么？

检索了什么？

用了哪些 evidence？

调用了什么工具？

Solver 输出了什么？

Verifier 为什么通过/拒绝？

最终给学生什么？

---

# 68. API

建立开发 API：

POST /api/v1/answer

输入至少：

question

tutor_level

grounding_policy

retrieval_policy

可选：

question_id

---

# 69. API Response

至少：

run_id

decision

tutor_response

confidence

citations

warnings

debug_info optional

默认：

不要暴露完整 debug trace。

---

# 70. Debug Mode

开发模式允许：

debug=true

查看：

QuestionUnderstanding

RetrievalPlan

EvidencePack summary

Solver structured output

ToolCalls

VerificationReport

AnswerDecision

但：

不要返回 hidden chain-of-thought。

---

# 71. Safety / Failure Handling

如果：

AI Provider timeout

retrieval error

tool error

verifier error

系统必须明确失败状态。

不要因为某一层失败：

直接绕过 Verifier 返回 Solver 答案。

特别禁止：

Verifier failed
→ automatically trust Solver

正确行为：

降低可信度 / retry / refuse。

---

# 72. Retry

允许有限 retry。

例如：

Solver structured output invalid

可以 retry 1 次。

Verifier 要求：

RETRY_SOLVER

可以 retry。

必须设置：

max_retries

避免 agent loop。

不要建立无限 autonomous agent。

---

# 73. Synthetic Test Knowledge

继续使用 Phase 03 synthetic documents。

可以增加少量 synthetic cases。

但必须清晰标记：

SYNTHETIC_DATA

不要导入真实 CChO 内容。

---

# 74. Phase 04 Synthetic Question Set

建立至少：

25 个 synthetic question cases。

覆盖：

基础概念

简单定量

摩尔质量

方程守恒

酸碱

平衡

氧化还原

多步骤

条件缺失

知识库缺失

矛盾 evidence

generated + unreviewed evidence

calculator use

molar mass tool

equation checker

---

# 75. 必须包含 Refusal Cases

至少：

5 个应该拒答 / 请求补充信息的 case。

例如：

知识库无资料

只有 generated unreviewed evidence 且 STRICT

缺图

缺关键数值

Evidence conflict 无法解决

---

# 76. 必须包含 Verification Failure Cases

至少：

5 个故意错误 Solver output。

例如：

错误加法

错误摩尔质量

错误方程系数

Unsupported Fact

Final Answer 与步骤不一致

Verifier 必须抓住。

---

# 77. Solver Evaluation

建立：

SolverEvaluationCase

至少：

question

expected_status

expected_knowledge_points

expected_tools

expected_key_claims

expected_final_answer optional

grounding_policy

should_answer

---

# 78. Verifier Evaluation

建立：

VerifierEvaluationCase

至少：

structured_solution

expected_failures

expected_action

---

# 79. Tutor Evaluation

至少验证：

LEVEL_1 不泄露 final answer。

LEVEL_2 不直接完整解题。

LEVEL_3 包含主要步骤。

LEVEL_4 包含完整 verified solution。

---

# 80. Evaluation Metrics

至少考虑：

Question understanding accuracy

Retrieval adequacy

Tool selection accuracy

Final answer correctness

Unsupported claim rate

Verification catch rate

False rejection rate

Tutor level compliance

Citation coverage

Refusal accuracy

本轮 synthetic 数据可以小。

重点是：

建立 framework。

---

# 81. 关键指标

特别记录：

# Unsupported Claim Rate

这是 Phase 04 核心指标之一。

定义：

重要 claims 中：

没有 evidence

没有 tool

没有 valid derivation

却被当成可靠事实输出的比例。

目标：

STRICT_GROUNDED 下尽量接近 0。

---

# 82. Verification Catch Rate

故意给 Verifier 错误 Solver outputs。

看能抓住多少。

不要只测试：

正确答案 → PASS。

---

# 83. False Refusal

系统不能为了安全：

什么都拒绝。

因此记录：

False Refusal Rate。

如果 Evidence 足够：

应该正常回答。

---

# 84. Tutor Leakage

建立：

Tutor Leakage Test。

LEVEL_1：

不得直接出现 final_answer。

LEVEL_2：

不得完整泄露所有关键步骤 + final_answer。

---

# 85. Tests

Phase 04 至少增加：

QuestionUnderstanding tests

missing asset tests

retrieval planner tests

EvidencePack tests

evidence conflict tests

Claim validation tests

Solver structured output tests

Calculator integration tests

MolarMassCalculator tests

EquationBalanceChecker tests

ToolRegistry tests

ToolExecutor tests

Verifier evidence tests

Verifier calculation tests

Verifier equation tests

Verifier dependency tests

AnswerPolicy tests

STRICT grounding tests

refusal tests

Tutor level tests

Tutor leakage tests

citation tests

trace tests

API tests

retry tests

provider failure tests

Phase 01–03 regression tests

---

# 86. PostgreSQL Integration

由于 Phase 03 已经完成真实 PostgreSQL + pgvector validation：

Phase 04 至少执行一次：

真实 DB

→ retrieval

→ EvidencePack

→ Solver mock

→ Verifier

→ Tutor

的 integration path。

不需要真实付费 LLM 才能完成架构验证。

可以使用 deterministic/mock AI provider。

---

# 87. Documentation

新增：

docs/answering/

至少：

answer-orchestration.md

question-understanding.md

evidence-pack.md

solver-architecture.md

tool-execution.md

verification.md

answer-policy.md

tutor-levels.md

answer-evaluation.md

---

# 88. Architecture Diagram

使用 Mermaid 描述：

User Question
↓
Question Understanding
↓
Question Readiness
↓
Retrieval Planner
↓
Knowledge Engine
↓
Evidence Builder
↓
Evidence Pack
↓
Solver
↕
Tool Executor
↓
Structured Solution
↓
Verifier
↕
Deterministic Tools
↓
Answer Policy
↓
Tutor
↓
Student Response

旁边连接：

Answer Trace

---

# 89. ADR

至少新增：

ADR-016
Why Solver and Tutor are separated

ADR-017
Why Verifier is independent from Solver

ADR-018
Why model memory is not Ground Truth

ADR-019
Why important answers are represented as Claims

ADR-020
Why deterministic tools override model arithmetic

ADR-021
Why we do not store chain-of-thought

ADR-022
Why Tutor defaults to LEVEL_2

ADR-023
Why verification failure blocks final answer

---

# 90. 本轮不要做

不要：

Question Generator

Automatic Exam Generator

Grader

Paper OCR

Student Profile

Adaptive Recommendation

Mistake Notebook

Knowledge Graph Reasoner

Multi-Agent Autonomous Loop

真实 CChO 大规模资料导入

互联网爬虫

模型 Fine-tuning

不要引入：

LangChain

LangGraph

除非存在不可替代的技术理由。

当前优先保持：

explicit orchestration。

---

# 91. Git

从：

checkpoint-pre-phase-04

之后继续开发。

不要修改已有：

checkpoint-phase-03

checkpoint-pre-phase-04

tag。

完成 Phase 04 后：

运行完整验证。

然后 commit：

feat: add evidence-grounded solver verifier and tutor

如果所有验收通过：

创建：

checkpoint-phase-04

annotated tag。

---

# 92. Phase 04 最关键 Acceptance Test

必须能够演示：

## Demo A — 正常回答

输入一个 synthetic knowledge 已覆盖的问题。

系统：

理解题目

→ 检索

→ EvidencePack

→ Solver

→ Tool if needed

→ Verifier PASS

→ Tutor LEVEL_2

→ 返回教学提示。

---

## Demo B — LEVEL 4

同一道题。

Tutor LEVEL_4。

输出：

完整步骤

+

final answer

+

citations

+

confidence。

---

## Demo C — Calculator

一道需要计算的问题。

Solver 调用 Calculator。

Verifier 重新检查。

答案一致。

---

## Demo D — Molar Mass

输入需要摩尔质量的问题。

必须调用：

MolarMassCalculator。

---

## Demo E — Equation Balance

Solver 输出 chemical equation。

EquationBalanceChecker 检查。

错误 equation：

Verifier FAIL。

---

## Demo F — Unsupported Claim

人为让 Solver 加入一个 Evidence 没有支持的专业事实。

Verifier：

必须发现。

---

## Demo G — Missing Image

输入：

“根据下图判断该分子的结构。”

但没有图片。

系统：

INSUFFICIENT_INPUT

→

REQUEST_CLARIFICATION

而不是猜结构。

---

## Demo H — Insufficient Evidence

知识库没有相关资料。

STRICT_GROUNDED。

系统：

REFUSE / HINT_ONLY

不能完整作答。

---

## Demo I — Generated Evidence

只有：

GENERATED + UNREVIEWED

资料。

STRICT_GROUNDED。

不得把它当 Ground Truth。

---

## Demo J — Wrong Calculation

人为构造：

Solver calculation wrong。

Verifier：

必须发现

→

禁止 ANSWER。

---

# 93. 最终验收问题

完成 Phase 04 后必须逐项回答：

1.

Solver 是否和 Tutor 解耦？

必须：

是。

2.

Verifier 是否和 Solver 解耦？

必须：

是。

3.

Question Understanding 是否支持：

missing input / missing asset？

必须：

是。

4.

是否存在 EvidencePack？

必须：

是。

5.

重要 Claim 是否可以追溯到：

Evidence

Tool

或 Derived Reasoning？

必须：

是。

6.

LLM model memory 是否可以自动成为 Ground Truth？

必须：

不能。

7.

Solver 是否可以调用 deterministic tools？

必须：

可以。

8.

Verifier 是否会重新检查 calculation？

必须：

会。

9.

错误 chemical equation 是否可以被检测？

必须：

可以。

10.

Critical verification failure 后是否仍然允许直接 ANSWER？

必须：

不能。

11.

STRICT_GROUNDED + INSUFFICIENT_EVIDENCE 是否会拒绝伪完整答案？

必须：

会。

12.

Tutor 是否支持 LEVEL_1–4？

必须：

支持。

13.

默认 Tutor Level 是否为 LEVEL_2？

必须：

是。

14.

LEVEL_1 是否防止 final answer leakage？

必须：

是。

15.

是否保存 hidden chain-of-thought？

必须：

不保存。

16.

是否有 AnswerTrace 可以复盘回答过程？

必须：

有。

17.

当前是否已经具备真正可靠的 CChO 专业答疑能力？

正确答案仍然应该是：

# 还不能这样宣称。

原因：

当前虽然已经建立：

Retrieval
+
Solver
+
Verifier
+
Tutor

完整架构，

但是：

尚未导入足够规模、合法、经过人工审核的真实 CChO 专业知识，

也尚未使用真实 CChO Benchmark 进行系统性评估。

---

# 94. 完成报告格式

完成后输出：

# A. Phase 04 Architecture

说明完整 answering loop。

---

# B. Question Understanding

字段、状态、missing input 处理。

---

# C. Retrieval Planning

如何生成 retrieval plan。

---

# D. Evidence Pack

结构、coverage、conflict。

---

# E. Solver

StructuredSolution / Step / Claim。

---

# F. Tool Layer

列出：

Calculator

MolarMassCalculator

EquationBalanceChecker

以及测试结果。

---

# G. Verifier

检查类型

blocking rules

VerificationReport。

---

# H. Answer Policy

列出：

ANSWER

ANSWER_WITH_CAVEAT

HINT_ONLY

REQUEST_CLARIFICATION

REFUSE

HUMAN_REVIEW

的决策规则。

---

# I. Tutor

LEVEL_1–4 行为。

---

# J. Trace & Observability

如何复盘一次回答。

---

# K. Evaluation

给出：

总 case 数

Unsupported Claim Rate

Verification Catch Rate

False Refusal Rate

Refusal Accuracy

Tutor Leakage

Citation Coverage

---

# L. Demo A–J

逐个展示：

输入

关键内部状态

最终 decision

结果。

不要输出 hidden chain-of-thought。

---

# M. Tests

Backend tests：

PostgreSQL integration：

TypeScript：

Next.js Build：

git diff --check：

---

# N. Git

Commit：

Tag：

Working tree：

---

# O. Technical Debt

列出真实剩余问题。

不要隐藏。

---

# P. Phase 05 Recommendation

说明下一阶段建议。

但：

不要直接实现 Phase 05。

---

# 95. 最终工程原则

Phase 04 的成功标准不是：

# “AI终于能回答问题了。”

而是：

# “系统知道自己为什么能够回答这个问题。”

对于一个答案，我们必须能够追问：

题目是怎么理解的？

检索了什么？

证据从哪里来？

哪些是事实？

哪些是推导？

哪些是工具计算？

哪些是假设？

Verifier 检查了什么？

为什么允许回答？

为什么 confidence 是这个等级？

为什么 Tutor 只给这些内容？

如果这些问题无法回答：

Phase 04 就没有真正完成。

---

# 96. 最后可靠性原则

ChemCoach 的优先级：

Correctness
>
Grounding
>
Verification
>
Pedagogy
>
Fluency

而不是：

Fluency
>
Everything Else

宁可输出：

“现有资料不足，我还不能可靠确认。”

也不要输出：

一个结构完整、语言漂亮、实际上没有可靠依据的化学答案。

现在开始执行 Phase 04。