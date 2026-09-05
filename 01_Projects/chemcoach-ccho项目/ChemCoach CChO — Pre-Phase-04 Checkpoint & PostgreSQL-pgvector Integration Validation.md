# ChemCoach CChO — Pre-Phase-04 Checkpoint & PostgreSQL/pgvector Integration Validation

你现在位于同一个长期项目：

# chemcoach-ccho

当前已完成：

- Phase 01：项目工程骨架
- Phase 02：Domain Model & Knowledge Ontology
- Phase 03：Knowledge Ingestion & Retrieval Engine

当前已知状态：

- Phase 02 和 Phase 03 尚未提交 Git
- 当前应位于 `main` 分支
- Phase 03 后端测试曾达到 `46 passed`
- TypeScript 类型检查通过
- Next.js production build 通过
- Alembic PostgreSQL offline SQL 生成通过
- 已包含 pgvector / PostgreSQL FTS / GIN / HNSW 相关实现
- 当前没有真实 CChO 专业资料
- 只有明确标记的 Synthetic / Demo data
- 当前机器此前没有 Docker CLI，因此 PostgreSQL + pgvector 尚未进行真实 runtime integration validation

本轮任务是进入 Phase 04 前的最后一次工程收尾。

本轮一次性完成两个目标：

# Goal A
创建 Phase 02 + Phase 03 的安全 Git checkpoint。

# Goal B
解决 P0 integration debt：

真正安装/启用 Docker 环境，并完成：

PostgreSQL
+
pgvector
+
Alembic
+
Knowledge Ingestion
+
FTS
+
Vector Retrieval
+
Hybrid Retrieval

的真实端到端数据库验收。

---

# 1. 本轮原则

本轮不是开发 Phase 04。

不要实现：

Solver
Tutor
Verifier
Question Generator
Grader
Student Profile

不要重构现有架构。

不要升级无关依赖。

不要顺手修改产品功能。

本轮只做：

检查
→ Git checkpoint
→ Docker 环境
→ PostgreSQL/pgvector runtime validation
→ 最终验证

---

# PART A — Git Checkpoint

# 2. 检查 Git 状态

首先运行：

```bash
git status
git branch --show-current
git log --oneline --decorate -10
```

确认：

1. 当前位于 `chemcoach-ccho`
2. 当前 branch 是 `main`
3. Phase 02 / Phase 03 尚未提交
4. 没有异常的非项目文件

---

# 3. Security Check

在 staging 前检查是否存在不应该进入 Git 的内容。

重点：

```text
.env
.env.local
.env.production
API Key
OPENAI_API_KEY
DATABASE PASSWORD
token
secret
credential
private key
真实个人数据
```

检查 `.gitignore` 至少覆盖：

```text
.env
.env.*
!.env.example

__pycache__/
*.pyc
.pytest_cache/
.venv/
venv/

node_modules/
.next/

coverage/
htmlcov/

.DS_Store
Thumbs.db

*.log
```

如果发现 secret：

不要提交。

不要在输出中泄露 secret 值。

---

# 4. Synthetic Data Check

检查现有 demo / synthetic knowledge。

确认它们清晰标记：

```text
SYNTHETIC
DEMO
TEST DATA
```

确认不存在：

- 冒充 CChO 官方真题
- 冒充官方答案
- 意外加入版权材料
- 私人资料
- 未授权教材全文

---

# 5. 提交前测试

重新运行：

```bash
python -m pytest
```

然后进入：

```text
apps/web
```

运行项目已有 TypeScript 检查命令。

例如：

```bash
npm run typecheck
```

如果没有，则使用仓库当前实际可用的 equivalent command。

再运行：

```bash
npm run build
```

然后返回仓库根目录。

运行：

```bash
git diff --stat
git diff --check
```

确认没有：

- whitespace error
- cache
- 临时文件
- build output
- `.env`
- 大型无关 binary

---

# 6. Git Commit

优先做一个安全完整 checkpoint。

Commit：

```text
feat: establish domain model and knowledge retrieval engine
```

Commit body：

```text
Complete Phase 02 domain modeling and Phase 03 knowledge engine.

Phase 02:
- add CChO-oriented domain model and ontology foundations
- add question, knowledge point, skill and solution relationships
- add step-level solutions and error patterns
- add source provenance and review status
- add Alembic domain migrations

Phase 03:
- add source/document/version/chunk knowledge model
- add ingestion and structure-aware chunking pipeline
- add keyword, vector and hybrid retrieval
- add retrieval policies and provenance
- add insufficient-evidence handling
- add retrieval evaluation framework and synthetic fixtures
- add ADR-005 through ADR-015

No real CChO knowledge has been ingested.
```

执行：

```bash
git add -A
git status
git diff --cached --stat
git diff --cached --check
```

确认安全后 commit。

然后：

```bash
git status
git log -1 --stat
git rev-parse --short HEAD
```

目标：

```text
working tree clean
```

---

# 7. 创建本地 Tag

创建：

```bash
git tag -a checkpoint-phase-03 -m "ChemCoach checkpoint after Phase 02 and Phase 03"
```

验证：

```bash
git tag --list
git show checkpoint-phase-03 --stat --oneline
```

本轮：

不要 push。

---

# PART B — Docker Environment

# 8. 检查 Docker 是否存在

现在检查：

```bash
docker --version
docker compose version
docker info
```

如果 Docker 已安装并运行：

直接进入 PostgreSQL runtime validation。

如果 Docker CLI 不存在：

进入安装流程。

---

# 9. Windows 环境识别

当前项目路径为类似：

```text
D:/chemcoach-ccho
```

因此优先按 Windows 开发环境处理。

检查：

```powershell
winget --version
wsl --status
wsl --version
```

记录：

- Windows 版本基本信息
- WSL 是否存在
- WSL2 是否可用
- winget 是否存在
- Docker Desktop 是否已安装但未启动

不要修改与本项目无关的系统设置。

---

# 10. Docker 安装策略

如果 Docker 未安装：

优先使用官方可信安装方式。

推荐顺序：

## Option A — winget

如果 winget 可用：

查找 Docker 官方包：

```powershell
winget search Docker.DockerDesktop
```

确认 package identity 后安装：

```powershell
winget install Docker.DockerDesktop
```

或使用 winget 返回的官方准确 ID。

不要使用来历不明的第三方安装包。

---

## Option B — 官方 Docker Desktop

如果 winget 不可用：

从 Docker 官方渠道下载安装 Docker Desktop。

只允许：

官方 Docker 下载来源。

不要使用第三方镜像站或未知安装程序。

---

# 11. 管理员权限 / 重启处理

Docker Desktop 安装可能需要：

- UAC 管理员授权
- WSL2
- Windows Virtual Machine Platform
- Hyper-V（视环境）
- 系统注销
- 系统重启

如果出现必须由用户交互完成的：

管理员授权
系统重启
登录操作

不要假装已经完成。

执行所有能够安全自动完成的步骤。

然后明确输出：

```text
MANUAL ACTION REQUIRED
```

说明用户需要做什么。

完成后能继续则继续。

如果确实必须重启导致当前任务无法继续：

保留 Git checkpoint 已完成状态，并明确数据库 runtime validation 尚未完成。

不要破坏仓库。

---

# 12. 启动 Docker

如果安装完成且无需重启：

启动 Docker Desktop。

等待 Docker Engine 可用。

验证：

```bash
docker --version
docker compose version
docker info
```

必须确认：

Docker Engine 真正响应。

不是只确认 CLI 存在。

---

# PART C — PostgreSQL + pgvector Runtime Validation

# 13. 检查 Compose 配置

检查：

```text
docker-compose.yml
.env.example
database/
database/migrations/
database/init/
```

特别确认：

PostgreSQL image

pgvector support

port

database

user

password

volume

healthcheck

不要泄露真实 credential。

如果 `.env` 不存在：

基于 `.env.example` 创建本地 `.env`。

`.env` 必须被 Git ignore。

只使用本地开发 credential。

不要把它提交 Git。

---

# 14. 启动数据库

优先只启动必要数据库服务。

例如：

```bash
docker compose up -d db
```

如果服务名不是 `db`：

使用当前 compose 的真实 service 名。

然后：

```bash
docker compose ps
docker compose logs <postgres-service>
```

确认：

容器 running

healthcheck healthy

数据库接受连接。

---

# 15. 验证 pgvector Extension

进入 PostgreSQL 或通过 SQLAlchemy 执行：

```sql
SELECT extname, extversion
FROM pg_extension
WHERE extname = 'vector';
```

如果 extension 尚未创建：

检查项目已有 migration / init SQL。

使用项目正式机制启用。

不要手工做一个无法复现的修补。

必须最终确认：

```text
vector extension exists
```

---

# 16. Alembic Runtime Migration

在真实 PostgreSQL 上运行：

```bash
alembic upgrade head
```

或者使用当前项目已有的正式 Alembic command/path。

必须确认：

migration 真正在 PostgreSQL 上成功执行。

不是 offline SQL。

检查：

```bash
alembic current
alembic heads
```

确认 current == head。

---

# 17. 验证数据库 Schema

检查关键表是否真实存在。

重点：

Source
Document
DocumentVersion
Chunk

以及 Phase 02 domain tables。

确认：

vector column

FTS index

GIN index

HNSW / vector index

实际存在。

可以查询：

```sql
SELECT tablename
FROM pg_tables
WHERE schemaname = 'public';
```

和 PostgreSQL catalog。

不要仅根据 migration 文件推断成功。

---

# 18. 真实 Knowledge Ingestion

使用 Phase 03 已有 synthetic/demo documents。

不要下载真实 CChO 内容。

选择至少 3 份 synthetic 文档。

通过正式 ingestion CLI 执行真实写库。

不要绕过 pipeline 直接 INSERT。

例如：

```bash
python -m chemcoach.knowledge.ingestion ingest ...
```

使用项目真实 CLI 参数。

记录：

document

version

chunk count

duplicate count

warnings

index status

review status

---

# 19. Duplicate Test

对其中一份 synthetic document 再次 ingest。

验证：

exact content hash duplicate detection

真实数据库中有效。

确认不会生成不必要重复内容。

---

# 20. PostgreSQL FTS Real Query

执行一个真实 keyword retrieval。

例如 synthetic 数据中存在：

```text
电子守恒
```

或项目 demo 数据中实际存在的关键词。

必须走：

PostgreSQL full text search

而不是 InMemoryRetriever。

验证：

结果数量

ranking

source

document

version

chunk

review status

authority

---

# 21. pgvector Real Query

执行真实 vector retrieval。

必须确认：

query embedding

→ vector column

→ pgvector similarity search

→ top-k

整个路径真实运行。

测试 provider 可以使用 deterministic embedding provider。

不要求真实 OpenAI。

重点是：

pgvector SQL 真正执行。

---

# 22. Hybrid Retrieval Real Query

执行：

HybridRetriever

真实组合：

keyword
+
vector
+
metadata filters
+
authority adjustment
+
review adjustment

至少跑 3 个案例：

## Case 1 — 正常成功

知识库存在对应资料。

期望：

SUCCESS

---

## Case 2 — Metadata Filter

同一个 query。

增加：

review_status
或
authority_level
或
knowledge_point

确认结果发生合理变化。

---

## Case 3 — Insufficient Evidence

使用知识库明确不存在的问题。

期望：

```text
INSUFFICIENT_EVIDENCE
```

或：

```text
NO_RESULTS
```

禁止为了有结果返回无关 chunk。

---

# 23. STRICT Policy Runtime Test

专门测试：

```text
GENERATED + UNREVIEWED
```

内容不能在：

STRICT policy

下自动成为高优先级 Ground Truth。

如果只有这种内容：

应该拒绝或 evidence insufficient。

---

# 24. Provenance Runtime Test

从真实 retrieval result 检查：

Chunk
→ DocumentVersion
→ Document
→ Source

必须全部可以追溯。

输出一个 demo provenance：

```text
source
document
version
chunk
section/page
authority
review status
```

---

# 25. Index Runtime Verification

确认实际 PostgreSQL 中存在：

FTS / GIN index

vector / HNSW index

不要只检查 migration SQL。

从 PostgreSQL 系统 catalog 查询。

---

# 26. Database Restart Test

执行一次：

```bash
docker compose restart <postgres-service>
```

数据库恢复后：

再次执行一个 retrieval。

确认：

data persistent

migration state retained

vector index usable

retrieval functional

---

# 27. Container Recreate / Persistence Safety

如果当前 compose 使用 named volume：

可以安全执行：

```bash
docker compose down
docker compose up -d
```

注意：

不要使用：

```bash
docker compose down -v
```

不要删除 volume。

重新启动后确认 synthetic 数据仍然存在。

---

# 28. Runtime Evaluation

如果 Phase 03 evaluation framework 可以直接指向 PostgreSQL retriever：

运行 evaluation。

至少报告：

Recall@K

MRR

Refusal accuracy

Authority compliance

Review compliance

如果 evaluation 当前只支持 deterministic/in-memory fixture：

不要为了这轮大改架构。

至少执行若干真实 PostgreSQL integration cases。

明确区分：

unit/synthetic evaluation

与：

PostgreSQL runtime integration validation。

---

# 29. Integration Test

如果合理：

新增一个最小 PostgreSQL integration test marker，例如：

```text
@pytest.mark.integration
```

保证：

普通：

```bash
pytest
```

不会强依赖 Docker。

但在 Docker 环境存在时，可以运行：

```bash
pytest -m integration
```

测试至少覆盖：

DB connect

pgvector

migration

ingestion

keyword retrieval

vector retrieval

hybrid retrieval

insufficient evidence

如果当前架构不适合在本轮添加：

可以建立 integration script。

不要为了测试框架重构项目。

---

# 30. Docker Documentation

更新 README / docs。

新增：

## Local Docker Setup

包括：

Windows Docker Desktop

启动方式

```bash
docker compose up -d
```

migration

ingestion

integration test

shutdown

并特别写：

正常停止：

```bash
docker compose down
```

会保留 named volume。

不要轻易使用：

```bash
docker compose down -v
```

因为会删除本地数据库数据。

---

# PART D — Git After Docker Validation

# 31. Docker 验证产生的代码处理

注意：

Phase 02 + Phase 03 checkpoint 已经在前面创建。

如果 Docker runtime validation 过程中：

没有修改代码：

不要再 commit。

如果为了修复真实 integration bug 必须修改：

migration
SQL
compose
retriever
ingestion
docs
integration tests

那么：

先运行完整回归测试。

然后创建第二个独立 commit：

```text
fix: validate PostgreSQL pgvector integration
```

Commit body 说明：

```text
- validate Dockerized PostgreSQL runtime
- validate pgvector extension
- validate Alembic migrations
- validate ingestion persistence
- validate FTS, vector and hybrid retrieval
- validate insufficient-evidence behavior
- add/fix integration tests and Docker documentation
```

不要修改：

`checkpoint-phase-03`

这个 tag。

该 tag 应继续指向最初 Phase 02 + Phase 03 checkpoint。

如果 integration fixes 产生新 commit：

额外创建：

```text
checkpoint-pre-phase-04
```

annotated tag：

```bash
git tag -a checkpoint-pre-phase-04 -m "ChemCoach validated before Phase 04"
```

---

# 32. 最终完整回归

完成 PostgreSQL runtime validation 后：

再次执行：

```bash
python -m pytest
```

以及前端：

TypeScript check

Next.js build

并检查：

```bash
git status
git diff --check
```

如果有 integration test：

执行。

---

# 33. 最终成功标准

进入 Phase 04 前必须尽量达到：

## Git

Phase 02 + Phase 03 已提交

存在：

```text
checkpoint-phase-03
```

working tree clean

---

## Docker

Docker CLI 可用

Docker Engine 正常

Docker Compose 可用

---

## PostgreSQL

真实容器启动成功

Alembic upgrade head 成功

---

## pgvector

vector extension 存在

vector similarity query 实际成功

HNSW/vector index 实际存在

---

## Knowledge Engine

synthetic document 实际写入 PostgreSQL

duplicate detection 实际成功

FTS retrieval 实际成功

vector retrieval 实际成功

hybrid retrieval 实际成功

metadata filter 实际成功

---

## Reliability

STRICT policy 实际工作

GENERATED + UNREVIEWED 不会自动成为 Ground Truth

INSUFFICIENT_EVIDENCE / NO_RESULTS 实际工作

---

## Provenance

Source
→ Document
→ Version
→ Chunk

真实数据库记录可完整追溯。

---

# 34. 如果 Docker 无法自动安装完成

如果受限于：

管理员权限

BIOS virtualization

WSL2

系统重启

企业设备策略

网络限制

则：

不要强行绕过系统限制。

不要安装未知第三方软件。

不要关闭安全功能。

不要修改企业安全策略。

输出：

```text
DOCKER RUNTIME VALIDATION BLOCKED
```

并准确说明：

1. 已经完成什么
2. 阻塞在哪里
3. 用户只需要做哪一个人工动作
4. Git checkpoint 是否已经安全完成
5. 哪些 PostgreSQL integration 项目仍然待验证

---

# 35. 禁止操作

本轮禁止：

```text
git reset --hard
git clean -fd
git push --force
docker compose down -v
删除数据库 volume
删除 Git history
删除 migration history
```

不要自动创建远程 GitHub 仓库。

不要 push。

---

# 36. 最终报告格式

完成后输出：

# A. Git Checkpoint

Branch：

Phase 02/03 Commit Hash：

Commit Message：

`checkpoint-phase-03`：

Working Tree：

---

# B. Docker Environment

Docker Version：

Docker Compose Version：

Docker Engine：

安装方式：

是否需要人工操作：

---

# C. PostgreSQL Runtime

Container：

Health：

Database Connection：

Alembic Current：

Alembic Head：

---

# D. pgvector

Extension：

Version：

Vector Column：

Vector Index：

真实 Vector Query：

---

# E. Ingestion Runtime

写入文档数：

写入 Chunk 数：

Duplicate Test：

Versioning：

---

# F. Retrieval Runtime

Keyword：

Vector：

Hybrid：

Metadata Filter：

STRICT Policy：

Insufficient Evidence：

---

# G. Provenance

展示一条实际：

Source
→ Document
→ Version
→ Chunk

---

# H. Tests

Backend pytest：

Integration Test：

TypeScript：

Next.js Build：

git diff --check：

---

# I. 修复项

真实 PostgreSQL 验证是否暴露 bug：

如果有：

列出 bug 和修复。

是否产生第二个 commit：

Commit Hash：

---

# J. Final Tag

如果没有 integration code fix：

`checkpoint-phase-03` 即可。

如果产生 integration fix：

确认：

```text
checkpoint-pre-phase-04
```

---

# K. Remaining Technical Debt

列出仍未解决问题。

不要隐藏。

---

# L. 最终判断

逐项回答：

1.

Phase 02 + Phase 03 是否已经形成安全 Git checkpoint？

2.

Docker 是否真实可用？

3.

PostgreSQL 是否真实运行成功？

4.

pgvector 是否真实执行过 similarity query？

5.

Alembic 是否在真实 PostgreSQL 上执行成功？

6.

Knowledge ingestion 是否真实写入 PostgreSQL？

7.

Keyword / Vector / Hybrid retrieval 是否都真实运行成功？

8.

INSUFFICIENT_EVIDENCE 是否真实验证？

9.

Source → Document → Version → Chunk 是否真实数据库可追溯？

10.

是否已经消除：

```text
P0 integration debt:
PostgreSQL + pgvector has not been runtime validated
```

如果以上关键项全部成功：

最后输出：

# READY FOR PHASE 04

如果 Docker 因人工操作被阻塞：

输出：

# NOT YET READY FOR FULL PHASE 04 INTEGRATION VALIDATION

并说明唯一剩余动作。

---

# 37. 最后原则

本轮真正的目标不是：

“安装 Docker。”

而是：

# 把 Phase 01–03 从“代码和测试看起来正确”，推进到“真实 PostgreSQL + pgvector 环境里已经跑通过”。

最终我们希望在进入 Phase 04 前明确知道：

数据库是真的能启动，

migration 是真的能执行，

资料是真的能摄取，

FTS 是真的能查，

pgvector 是真的能查，

Hybrid Retrieval 是真的能工作，

证据不足是真的会拒绝。

完成这些之后再进入 Phase 04。

现在开始执行。