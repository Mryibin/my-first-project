# ChemCoach CChO

ChemCoach CChO is a long-term engineering project for a reliable, personalized AI coach for learners of the Chinese Chemistry Olympiad. It is designed around an orchestrator, bounded specialist capabilities, a provenance-aware knowledge engine, deterministic tools, a student model, and evaluation—not around a chemistry prompt attached to a general chatbot.

> **CURRENT KNOWLEDGE STATUS: NO REAL CChO KNOWLEDGE INGESTED**

The current system does **not** have real CChO professional question-answering capability.

## Current phase

Phase 03 adds a working provenance-first ingestion, indexing, policy, hybrid retrieval, and evaluation slice. It can ingest controlled Markdown/text sources, version and chunk them, retrieve with PostgreSQL keyword + pgvector candidates, explain ranking, and refuse weak evidence. Its retrieval dataset is deliberately synthetic; there is still no real CChO corpus or professional answering capability.

Phase 04 adds an explicit evidence-grounded answering loop: structured question understanding,
retrieval planning, EvidencePack construction, claim-based solving, Calculator/molar-mass/equation
tools, independent verification, hard answer policy, Level 1–4 tutoring, and replayable AnswerRun
traces. The default is `STRICT_GROUNDED` plus Tutor `LEVEL_2`. Because the corpus remains
synthetic and unreviewed, this architecture is not a claim of real CChO answering quality.

Phase 05 adds source/copyright/usage governance, identified human review, version-specific and revocable approvals, knowledge quality gates, isolated frozen benchmark datasets, leakage checks, knowledge snapshots, reproducible runs, dimensional metrics, and JSON/Markdown capability reports. Real CChO source and benchmark counts remain zero by design.

Development answer endpoint:

```http
POST /api/v1/answer
Content-Type: application/json

{"question":"Calculate the molar mass of H2O","tutor_level":"level_2","grounding_policy":"strict_grounded","retrieval_policy":"strict","debug":false}
```

Set `debug=true` to receive structured `debug_info`; hidden chain-of-thought is never returned.

## Technology

- Web: Next.js 16, TypeScript, Tailwind CSS
- API: Python 3.12+, FastAPI, Pydantic
- Persistence foundation: SQLAlchemy async, PostgreSQL 16, pgvector
- Infrastructure: Docker Compose
- Tests: pytest

## Repository structure

```text
apps/
  api/                  FastAPI application and Docker image
  web/                  Next.js application and Docker image
database/
  init/                 PostgreSQL extension initialization
  migrations/           Alembic schema history
  seeds/                Future reviewed seed data
docs/
  architecture/         System design
  adr/                  Architecture decision records
knowledge_base/
  raw/ processed/ reviewed/ synthetic/
scripts/                Future operational scripts
tests/                  Backend contract and unit tests
docker-compose.yml
pyproject.toml
```

## Configuration

Copy `.env.example` to `.env`. All backend variables use the `CHEMCOACH_` prefix. Important settings are:

| Variable | Default purpose |
| --- | --- |
| `CHEMCOACH_ENVIRONMENT` | `development`, `test`, or `production` |
| `CHEMCOACH_LOG_LEVEL` | Structured application log level |
| `CHEMCOACH_DATABASE_URL` | SQLAlchemy async PostgreSQL URL |
| `CHEMCOACH_AI_PROVIDER` | Provider adapter selection; currently `mock` |
| `CHEMCOACH_AI_MODEL` | Configured model identifier |
| `CHEMCOACH_EMBEDDING_MODEL` | Provider-neutral embedding model identifier |
| `CHEMCOACH_EMBEDDING_DIMENSIONS` | Stored vector width; currently 64 |
| `CHEMCOACH_INGESTION_MAX_FILE_BYTES` | Intake size ceiling |
| `CHEMCOACH_RETRIEVAL_MINIMUM_SCORE` | Application retrieval threshold baseline |
| `NEXT_PUBLIC_API_BASE_URL` | Browser-visible API origin |

Do not commit secrets. Logging excludes common credential fields; adapters must additionally avoid putting secrets in messages or metadata.

## Local development

Backend (PowerShell):

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -e ".[dev]"
alembic upgrade head
uvicorn chemcoach.main:app --app-dir apps/api --reload
```

The API is at `http://localhost:8000`; health is `GET /api/v1/health`; OpenAPI is `/docs`.

Frontend:

```powershell
Set-Location apps/web
npm install
npm run dev
```

The web app is at `http://localhost:3000`.

## Docker

After copying `.env.example` to `.env`:

```powershell
docker compose up --build
```

This starts PostgreSQL with pgvector, the API, and the web app. Apply `alembic upgrade head` before using persistence-backed features.

### Local Docker setup

On Windows, install Docker Desktop from the official Docker package, enable its WSL 2 backend, and confirm that `docker info` returns a Linux server. Then start only the database while developing the knowledge engine:

```powershell
Copy-Item .env.example .env
docker compose up -d db
docker compose ps
alembic upgrade head
alembic current
```

Prepare deterministic synthetic source records and run the real PostgreSQL checks with:

```powershell
python scripts/postgres_integration_validation.py prepare
# Ingest at least three files with the source IDs printed above, using:
python -m chemcoach.knowledge.ingestion ingest <synthetic-file> <required-metadata>
python scripts/postgres_integration_validation.py verify
```

The verifier exercises PostgreSQL FTS, pgvector cosine search, hybrid ranking, metadata filters, strict-policy refusal, insufficient evidence, duplicate-run evidence, and full provenance. It expects migrated schema and previously ingested Synthetic fixtures; it is intentionally separate from ordinary unit tests.

Normal shutdown preserves the named database volume:

```powershell
docker compose down
```

Do not casually use `docker compose down -v`: the `-v` option deletes the local PostgreSQL data volume.

## Tests

```powershell
python -m pytest
```

Tests require neither an external AI API nor a CChO knowledge collection. They cover the earlier foundations plus governance, review/revocation, frozen benchmark workflows, leakage boundaries, knowledge snapshots, dimensional evaluation, reporting, and Alembic upgrade/downgrade.

## Governance and benchmark CLI

The internal `chemcoach` command supports source registration, review decisions, benchmark import/validation/freeze/run/report, and never imports benchmark references into the knowledge index. See [knowledge governance](docs/governance/knowledge-governance.md) and [benchmark architecture](docs/benchmark/benchmark-architecture.md).

```powershell
chemcoach source register source.json
chemcoach review list
chemcoach benchmark validate <dataset-id>
chemcoach benchmark run <dataset-id> --snapshot-id <snapshot-id> --git-commit <commit>
```

## Knowledge ingestion CLI

Run a safe offline preview first:

```powershell
python -m chemcoach.knowledge.ingestion ingest knowledge_base/synthetic/demo-redox.md `
  --source-id 00000000-0000-0000-0000-000000000001 `
  --document-type synthetic_demo --authority generated --copyright-status private_use `
  --review-status unreviewed --title "Synthetic redox demo" --dry-run
```

Without `--dry-run`, the command writes through the configured async database. Migrations and the referenced `KnowledgeSource` must already exist; source authority and copyright metadata must match exactly.

## Completed in Phase 03

- Source → document → immutable version → structured chunk persistence (26 total tables)
- Controlled Markdown/plain-text intake, chemistry-safe normalization, and exact deduplication
- Structure-aware and fixed-window chunking with validation
- Provider-neutral deterministic embeddings and pgvector storage/indexing
- PostgreSQL full-text, vector, and hybrid retrieval with typed metadata filters
- Strict, balanced, and exploratory retrieval policies
- Typed success, partial, no-result, and insufficient-evidence states
- Citation-ready provenance and score-component debug output
- Four explicitly generated/unreviewed synthetic documents and 17 evaluation cases
- Retrieval metrics, review transitions, migration coverage, documentation, and ADR-011–ADR-015

## Completed in Phase 02

- Twenty normalized domain and association tables
- Strict authority, review, copyright, relation, error, and content vocabularies
- Question versions, non-text asset references, and lifecycle/supersession fields
- Many-to-many Question relationships with relational metadata
- Ordered SolutionSteps with knowledge, method, and ErrorPattern links
- Aggregate-oriented Question, KnowledgePoint, and Solution repositories
- PostgreSQL-first Alembic initial migration, tested against SQLite
- One unreviewed `DEMO / SYNTHETIC DATA` relationship fixture
- Domain documentation, Mermaid diagrams, and ADR-005 through ADR-010

## Phase 01 foundation retained

- Monorepo structure and container definitions
- Versioned FastAPI health endpoint
- Environment-based typed configuration
- Async SQLAlchemy PostgreSQL connection/session foundation
- Provider-neutral AI interface and deterministic mock
- Provenance-bearing knowledge retrieval interface and in-memory test adapter
- Extensible tool protocol and AST-based calculator (never `eval`)
- JSON logging and component-run observation context
- Minimal Next.js status page
- Automated backend tests
- Architecture overview and four ADRs

## Phase 06 real-data pilot

Phase 06 adds rights-gated quarantined intake, fine-grained CorpusUnit review, tiered real benchmark governance, leakage checks, human evaluation, confidence calibration, three evaluation modes, and a claim-limiting CapabilityGate. Operator workflows are in `docs/pilot/`.

This repository contains no real CChO corpus or benchmark data. Intake files, reports, and review packets are ignored by default. Until legally usable material and independent human review are supplied, status is **REAL DATA INTAKE PENDING**, not demonstrated CChO reliability.

## CCHO_REAL_PILOT_V1 data workbench

The Phase 06 file/CLI workbench now provides blank manifests, review packets, a 30-case selection sheet, scoped status/readiness checks, separate GoldReference imports, snapshots, and centrally enforced Development/Calibration/Holdout execution. Start with `chemcoach pilot init CCHO_REAL_PILOT_V1` and follow the [operator guide](docs/pilot/workbench/README.md). No real data is created by initialization. Workbench readiness does not mean a real pilot has begun.

## Explicitly not implemented

- Real CChO source or benchmark content
- Reliable professional CChO capability, image recognition, or validated production tutoring
- Question generation, exam generation, grading, or error diagnosis
- Student accounts, mistake management, mastery profiles, or recommendations
- Autonomous multi-agent loops or production AI provider adapters
- Production deployment hardening and evaluation benchmark content

## Next phase direction

Phase 06 should acquire a very small legally usable real corpus and real benchmark sample, complete independent review, calibrate retrieval and refusal thresholds, and keep synthetic/real reporting separate before any production capability claim.

See [the system overview](docs/architecture/system-overview.md), [retrieval architecture](docs/knowledge/retrieval-architecture.md), [ingestion guide](docs/knowledge/knowledge-ingestion.md), [domain model](docs/domain/ccho-domain-model.md), and [architecture decisions](docs/adr/) for the constraints that guide future work.
