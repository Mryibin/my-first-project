你是一名资深 AI 产品架构师、Python 后端工程师和全栈工程师。

现在请帮我从零搭建一个项目：

# ChemCoach CChO

这是一个面向中国化学奥林匹克竞赛（CChO）学习者的 AI 化学竞赛教练。

## 一、产品目标

第一阶段不是做完整商业产品，而是搭建一个可持续迭代的技术骨架。

核心目标：

用户输入一道 CChO 化学题，可以：

1. 输入文字题目
2. 上传题目图片
3. AI 识别题目
4. 判断题目所属知识领域
5. 从专业知识库检索相关知识
6. 调用 Solver Agent 分析解题路径
7. 调用 Tutor Agent 生成适合学生理解的讲解
8. 返回：
   - 题目理解
   - 考察知识点
   - 解题思路
   - 分步引导
   - 最终答案
   - 易错点
   - 推荐继续学习的知识点

注意：

这是一个“AI 教练”，不是简单的 ChatGPT Wrapper。

AI 必须尽可能基于专业知识库回答，不能把大模型自身记忆当作唯一知识来源。

---

# 二、技术栈

请使用：

Frontend:
- Next.js
- TypeScript
- Tailwind CSS

Backend:
- Python 3.12+
- FastAPI
- Pydantic
- SQLAlchemy

Database:
- PostgreSQL
- pgvector

AI:
- OpenAI API
- 使用环境变量配置 API Key
- 模型名称不要硬编码，统一通过配置读取

Infrastructure:
- Docker
- docker-compose

Testing:
- pytest
- frontend 使用 TypeScript 类型检查和基础测试

代码管理：
- Git

---

# 三、第一阶段严格限制

这一阶段不要实现：

- 用户注册
- 支付
- 社区
- 排行榜
- 微信登录
- 原生 Android
- 原生 iOS
- 商业化功能

暂时也不要实现复杂的多 Agent 自主循环。

先搭建稳定的 Agent Orchestrator 架构。

---

# 四、项目目录

请按照下面结构创建项目：

chemcoach-ccho/

frontend/

backend/
  app/
    api/
    agents/
      orchestrator/
      tutor/
      solver/
      question/
      grader/
      profile/
    knowledge/
    models/
    schemas/
    services/
    tools/
      chemistry/
      calculator/
      vision/
    core/
    main.py
  tests/

knowledge_base/
  ccho/
  textbooks/
  solutions/
  methods/

database/
  migrations/
  seeds/

docs/

---

# 五、Agent 架构

设计以下 Agent：

## 1. Orchestrator Agent

负责整个请求的调度。

流程：

用户问题
↓
题目解析
↓
知识检索
↓
Solver Agent
↓
答案验证
↓
Tutor Agent
↓
最终响应

Orchestrator 不直接承担复杂化学推理。

---

## 2. Solver Agent

负责：

- 分析题目
- 识别题型
- 识别知识点
- 制定解题路径
- 进行化学推理
- 必要时调用计算工具
- 输出结构化结果

要求 Solver 输出 Pydantic Schema，而不是自由文本。

建议结构：

{
  "problem_understanding": "",
  "knowledge_points": [],
  "question_type": "",
  "difficulty": 1,
  "solution_strategy": [],
  "calculation_required": false,
  "calculation_expression": "",
  "final_answer": "",
  "common_mistakes": []
}

---

# 六、Tutor Agent

Tutor Agent 的目标不是直接给答案，而是帮助学生建立解题能力。

默认教学模式：

Socratic / 引导式教学。

分成四个等级：

LEVEL_1:
只给提示

LEVEL_2:
给知识点和解题方向

LEVEL_3:
给关键步骤

LEVEL_4:
完整解析

默认使用 LEVEL_1 或 LEVEL_2。

只有用户明确要求“完整解析”“直接告诉我答案”等情况下，才进入 LEVEL_4。

Tutor 输出：

{
  "teaching_mode": "",
  "opening_hint": "",
  "guidance_steps": [],
  "knowledge_explanation": "",
  "full_solution": "",
  "common_mistakes": [],
  "next_question_suggestion": ""
}

---

# 七、Knowledge Engine

先不要实现复杂的知识图谱。

但是架构必须为未来知识图谱预留接口。

Knowledge Engine 需要支持：

1. keyword search
2. vector search
3. metadata filtering

知识资料未来包含：

- CChO 官方竞赛要求
- CChO 历年真题
- 官方参考答案
- 合法获得的教材
- 解题方法
- 易错点
- 典型题

每一个知识文档必须带 Metadata。

建议：

{
  "source": "",
  "source_type": "",
  "year": null,
  "competition_stage": "",
  "subject_area": "",
  "topic": "",
  "difficulty": null,
  "version": "",
  "authority_level": ""
}

其中 authority_level 可以：

official
textbook
expert
curated
generated

原则：

official > textbook > expert > curated > generated

未来回答时应该优先使用高权威来源。

---

# 八、严禁把知识库和普通聊天混在一起

设计一个 KnowledgeRetriever 接口：

retrieve(
    query,
    filters=None,
    top_k=5
)

返回：

KnowledgeChunk[]

未来可以替换：

PostgreSQL + pgvector
OpenAI File Search
其他 Vector DB

而不影响 Agent。

---

# 九、AI Provider 抽象层

不要在 Agent 中直接大量调用 OpenAI SDK。

创建：

AIProvider

接口，例如：

generate()
generate_structured()
vision()
embedding()

然后创建：

OpenAIProvider

未来可以增加：

GeminiProvider
ClaudeProvider
QwenProvider
DeepSeekProvider

这样可以做模型横向评测。

---

# 十、计算工具

建立 ChemistryCalculator 接口。

第一阶段至少支持：

calculate(expression)

并预留：

- SymPy
- NumPy
- SciPy
- RDKit

未来可以增加：

- 化学方程式配平
- 摩尔质量计算
- 酸碱平衡
- 化学平衡
- 电化学
- 热力学
- 分子结构分析

重要原则：

对于高风险计算，不允许 LLM 直接声称自己完成了精确计算。

应该：

LLM 判断需要计算
↓
调用 Calculator
↓
返回结果
↓
LLM 解释

---

# 十一、可靠性设计

这是整个项目最重要的要求。

建立 AnswerVerification 层。

每一次回答至少记录：

- retrieved_sources
- reasoning_status
- calculation_used
- calculation_result
- verification_status
- confidence
- warnings

confidence 分为：

HIGH
MEDIUM
LOW

如果没有找到足够权威的知识资料：

不要让模型编造。

返回：

“当前知识库中没有找到足够可靠的资料支持这个结论。”

而不是让模型自由发挥。

---

# 十二、数据库设计

第一阶段建立这些表：

students

questions

knowledge_points

knowledge_documents

solution_methods

mistakes

learning_records

student_knowledge_mastery

agent_runs

暂时可以不做复杂用户系统，但数据库模型需要先建立。

---

# 十三、API

至少创建：

POST /api/v1/tutor/ask

输入：

{
  "question": "...",
  "image_url": null,
  "teaching_level": 1
}

输出：

{
  "question_understanding": {},
  "knowledge_points": [],
  "retrieved_sources": [],
  "solution": {},
  "teaching": {},
  "verification": {},
  "next_step": {}
}

再创建：

GET /api/v1/health

用于健康检查。

---

# 十四、前端第一版

首页只需要一个非常简单的 AI Tutor 页面。

页面包含：

顶部：

ChemCoach CChO
你的专属 AI 化学奥赛教练

中间：

聊天区域

底部：

输入框

“发送”

“上传图片”

“完整解析”

按钮。

同时显示：

- 考察知识点
- 解题思路
- 当前提示
- 相关知识

不要做复杂 UI。

重点是让整个调用链跑通。

---

# 十五、Demo

请内置一个 Demo Question。

例如：

“某化学平衡体系中……（请使用一个简单的示例题，不要虚构成 CChO 真题）”

启动项目后：

用户输入问题

↓

Frontend

↓

FastAPI

↓

Orchestrator

↓

KnowledgeRetriever

↓

Solver

↓

Verifier

↓

Tutor

↓

Frontend

整个链路必须能够跑通。

---

# 十六、配置

创建：

.env.example

至少包含：

OPENAI_API_KEY=
OPENAI_MODEL=
OPENAI_VISION_MODEL=
OPENAI_EMBEDDING_MODEL=
DATABASE_URL=

不要把 API Key 写入代码。

.gitignore 必须包含：

.env
__pycache__
node_modules
.next
*.pyc

---

# 十七、日志

所有 Agent 执行都要记录：

agent_name
run_id
input
output
duration
status
error

但不要记录敏感信息。

未来这些数据用于：

Agent Evaluation。

---

# 十八、测试

至少写：

1. Health API 测试
2. Tutor API 测试
3. KnowledgeRetriever 单元测试
4. Solver Schema 测试
5. Tutor Schema 测试
6. Calculator 测试
7. Orchestrator 流程测试

AI 调用必须支持 mock。

不要让测试依赖真实 OpenAI API。

---

# 十九、README

README 必须包含：

1. 项目介绍
2. 系统架构
3. 目录结构
4. 环境变量
5. 本地启动方式
6. Docker 启动方式
7. API 示例
8. Agent 工作流程
9. 如何添加新的知识文档
10. 如何替换 AI Provider
11. 如何运行测试

---

# 二十、开发原则

请遵循：

- Clean Architecture 思想
- 高内聚低耦合
- Agent 与业务逻辑分离
- AI Provider 与 Agent 分离
- Knowledge Engine 与 Agent 分离
- 数据库与业务逻辑分离
- 所有核心结构使用 Pydantic
- 类型完整
- 函数保持小而清晰
- 不要创建巨型文件
- 不要为了“看起来高级”引入不必要的框架

---

# 二十一、非常重要

不要假装系统已经具备 CChO 专业知识。

目前知识库只是空骨架。

请明确标记：

KNOWLEDGE_BASE_STATUS = EMPTY / DEMO

以后我们会单独建立真正的 CChO 专业知识库。

不要虚构：

- CChO 真题
- 官方答案
- 官方知识点
- 专家解析

如果需要 Demo 数据，请明确标记为：

DEMO_DATA

---

# 二十二、最终交付

完成后请：

1. 创建完整项目目录
2. 创建所有核心代码
3. 创建数据库模型
4. 创建 API
5. 创建前端页面
6. 创建 Docker 配置
7. 创建测试
8. 创建 README
9. 运行测试
10. 修复所有明显错误

最后不要只告诉我“完成了”。

请给我：

A. 项目目录树

B. 已完成的功能

C. 尚未完成的功能

D. 如何启动

E. 测试结果

F. 下一阶段建议

注意：

**不要在这一轮实现自动出题、试卷扫描批改、学习画像等完整功能。**

这一轮唯一目标：

> **把 ChemCoach CChO 的 AI Tutor 最小闭环跑通，并为后续 CChO 专业知识库、自动出题、批改和学习画像留下干净的扩展接口。**