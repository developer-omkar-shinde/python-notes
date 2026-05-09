# Backend Engineering Roadmap — Trivelta Backend Services

> **Personal learning plan for ramping up on `trivelta-backend-services`.**
> Starting point: comfortable with Python + OOP, new to backend.
> Target: confidently shipping PRs to this codebase within ~6 weeks, fluent in ~3 months.

---

## How to use this document

- **Don't try to read it all at once.** Work through it phase by phase.
- **Tick off items (`- [x]`)** as you complete them — keeps you honest about progress.
- **Always pair learning with reading real code in this repo.** Theory without practice fades fast.
- **Resist the urge to learn every technology.** Learn enough to ship, then deepen by need.
- **When stuck, trace one real request end-to-end before reading more theory.**

---

## Table of Contents

1. [Phase 0 — Prerequisites to solidify](#phase-0--prerequisites-to-solidify)
2. [Phase 1 — Backend fundamentals (framework-agnostic)](#phase-1--backend-fundamentals-framework-agnostic)
3. [Phase 2 — The actual tech stack of this repo](#phase-2--the-actual-tech-stack-of-this-repo)
4. [Phase 3 — Patterns and architecture this team uses](#phase-3--patterns-and-architecture-this-team-uses)
5. [Phase 4 — Hands-on inside this codebase](#phase-4--hands-on-inside-this-codebase)
6. [The 6-week schedule](#the-6-week-schedule)
7. [Reading list](#reading-list)
8. [Common pitfalls to avoid](#common-pitfalls-to-avoid)
9. [Mindset and habits](#mindset-and-habits)
10. [Quick reference cheat sheet](#quick-reference-cheat-sheet)

---

## Phase 0 — Prerequisites to solidify

These come up on day one. If anything feels fuzzy, fix it before moving on.

### Topics

- [ ] **HTTP fundamentals**
  - Methods: `GET`, `POST`, `PUT`, `PATCH`, `DELETE`
  - Status codes: 2xx, 3xx, 4xx, 5xx (memorize: 200, 201, 204, 400, 401, 403, 404, 409, 422, 500, 503)
  - Headers: `Content-Type`, `Authorization`, `Accept`, `User-Agent`
  - Request anatomy: method + URL + headers + body
  - Query params vs path params vs body — when to use each

- [ ] **JSON**
  - Serialization / deserialization
  - Nested structures
  - `dict` ↔ JSON in Python
  - Common gotchas: `Decimal`, `datetime`, `None` vs missing key

- [ ] **REST principles**
  - Resources are nouns (plural), not verbs
  - Statelessness
  - Versioning (`/v1`, `/v2`)
  - Idempotency (which methods are safe/idempotent and why)
  - **Read `AGENTS.md` section 7 — your reference**

- [ ] **Git essentials**
  - `branch`, `checkout`, `commit`, `push`, `pull`, `rebase`, `merge`
  - Resolving conflicts
  - Reading a `git log` and understanding history
  - **This repo enforces `feat/TICKET-123-description` branch naming** — read `AGENTS.md` section 1

- [ ] **Terminal basics**
  - Navigation: `cd`, `ls`, `pwd`
  - File operations: `cat`, `less`, `grep`, `find`
  - Environment variables: `export FOO=bar`, `echo $FOO`
  - Running scripts and commands

### Resources
- MDN: [HTTP overview](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)
- [JSON.org](https://www.json.org/)
- Atlassian: [Git tutorials](https://www.atlassian.com/git/tutorials)
- The repo's own `AGENTS.md` (sections 1, 2, 7)

**Estimated time:** 3-7 days

---

## Phase 1 — Backend fundamentals (framework-agnostic)

Universal concepts you'll use no matter which job you're at.

### 1.1 The client-server model
- [ ] How a request travels: client → DNS → load balancer → server → database → back
- [ ] What "stateless" means and why it matters for scaling
- [ ] Synchronous vs asynchronous communication

### 1.2 Authentication & Authorization
- [ ] Difference between authn (who you are) and authz (what you can do)
- [ ] **JWT (JSON Web Tokens)** — structure, signing, expiration
- [ ] Sessions vs tokens
- [ ] API keys (used by admin endpoints in this repo)
- [ ] **AWS Cognito basics** — user pools, identity pools, tokens
- [ ] OAuth 2.0 flow (high level)

### 1.3 Databases — SQL vs NoSQL
- [ ] What relational databases are good at (joins, transactions, ad-hoc queries)
- [ ] What NoSQL is good at (scale, flexible schema, predictable performance)
- [ ] CAP theorem (high level — pick two of consistency, availability, partition tolerance)
- [ ] When to use which
- [ ] Indexes — what they are and why they matter

### 1.4 Asynchronous architecture
- [ ] Why we don't do everything in the request/response cycle
- [ ] Message queues — what they solve (decoupling, retries, buffering)
- [ ] Pub/sub vs queues
- [ ] Event-driven systems (this repo is heavily event-driven)
- [ ] At-least-once vs exactly-once delivery; idempotency

### 1.5 Logging, error handling, observability
- [ ] Structured logging (key=value or JSON, not free text)
- [ ] Log levels: DEBUG, INFO, WARNING, ERROR, CRITICAL
- [ ] Why you never log secrets/PII
- [ ] Metrics vs logs vs traces (the three pillars of observability)
- [ ] How to write actionable error messages

### 1.6 Configuration and secrets
- [ ] Environment variables vs config files
- [ ] Why secrets never go in code or git
- [ ] **AWS Secrets Manager** (used everywhere in this repo)
- [ ] [12-factor app principles](https://12factor.net/)

### Resources
- Free book: [The Architecture of Open Source Applications](https://aosabook.org/) (skim relevant chapters)
- YouTube: "What is a message queue?" (any 10-min intro)
- jwt.io — paste a token, see what's inside
- AWS docs: [Cognito User Pools — getting started](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html)

**Estimated time:** 2-3 weeks (in parallel with Phase 2)

---

## Phase 2 — The actual tech stack of this repo

Tier ordering = priority. Don't move to Tier 2 before being comfortable with Tier 1.

### Tier 1 — Use on day one

#### Python typing
- [ ] Type hints: `int`, `str`, `bool`, `list[str]`, `dict[str, Any]`
- [ ] `Optional[X]` and `X | None`
- [ ] `Protocol` (used for repository interfaces in v2 code)
- [ ] `TypedDict` (used in `helper/api_gateway_resolver.py`)
- [ ] `dataclass` and `@dataclass(frozen=True)`
- [ ] When to use `cast` (rarely)

#### Pydantic v2
- [ ] `BaseModel`, fields with defaults, `Optional` fields
- [ ] `Field(...)` constraints (`min_length`, `gt`, `regex`)
- [ ] Validators: `@field_validator`, `@model_validator`
- [ ] `model_config = ConfigDict(extra="ignore" | "forbid")`
- [ ] `.model_dump()` / `.model_validate()`
- [ ] **Read in repo:** `services/onboarding_service/onboarding/v2/schemas/requests/signup_request.py`
- [ ] **Read in repo:** `services/bonus_platform_service/bonus_service_v2/modules/referral/schemas/commands.py`

#### AWS — what each service does (just the *what*, not the *how*)
- [ ] **Lambda** — runs code per-event, no servers
- [ ] **API Gateway** — turns HTTPS into Lambda events
- [ ] **DynamoDB** — NoSQL key-value/document database
- [ ] **SQS** — message queue
- [ ] **SNS** — pub/sub topics
- [ ] **S3** — object storage (files)
- [ ] **Secrets Manager** — credential storage
- [ ] **Cognito** — user authentication
- [ ] **CloudWatch** — logs, metrics, alarms
- [ ] **EKS** — managed Kubernetes
- [ ] **ECS** — managed Docker (used by some workers)
- [ ] **Kinesis** — real-time streams (used in odds feed)

#### `boto3` — AWS Python SDK
- [ ] `boto3.client("...")` vs `boto3.resource("...")`
- [ ] DynamoDB:
  - [ ] `Table.get_item()` — fetch by key
  - [ ] `Table.query()` — fetch by partition key + condition
  - [ ] `Table.scan()` — full-table scan (avoid this in prod)
  - [ ] `Table.put_item()` — insert/replace
  - [ ] `Table.update_item()` — partial update (`UpdateExpression`)
  - [ ] `Table.delete_item()`
  - [ ] `Key()` and `Attr()` from `boto3.dynamodb.conditions`
  - [ ] `KeyConditionExpression` vs `FilterExpression` (read this 3 times)
  - [ ] `ConditionExpression` (used for atomic writes)
- [ ] SQS: `send_message`, `receive_message`, `delete_message`
- [ ] S3: `get_object`, `put_object`, `presigned URLs` (basic)

#### DynamoDB modeling (this trips everyone up coming from SQL)
- [ ] Partition key vs sort key
- [ ] Composite primary keys
- [ ] **Global Secondary Indexes (GSI)** — alternate query patterns
- [ ] **Local Secondary Indexes (LSI)** — same partition, different sort
- [ ] **`KeyConditionExpression` reads from the index. `FilterExpression` runs after.** Filter does NOT save read capacity.
- [ ] Pagination with `LastEvaluatedKey`
- [ ] Hot partition problem
- [ ] Single-table design (the AWS-recommended pattern)
- [ ] **MUST WATCH:** Rick Houlihan's AWS re:Invent talk on DynamoDB (search YouTube — any year, all are gold)

### Tier 2 — Pick up in weeks 2-4

#### FastAPI (used as a thin EKS wrapper here)
- [ ] Path operations: `@app.get`, `@app.post`, etc.
- [ ] Path parameters and query parameters
- [ ] Pydantic-based request bodies and response models
- [ ] Dependencies (`Depends(...)`)
- [ ] Middleware
- [ ] [Official FastAPI tutorial](https://fastapi.tiangolo.com/tutorial/) — first half is enough

#### `pytest`
- [ ] Test discovery and naming conventions
- [ ] `assert` statements
- [ ] **Fixtures** — `@pytest.fixture`, scope (`function`, `module`, `session`)
- [ ] `conftest.py` — shared fixtures
- [ ] `parametrize` — table-driven tests
- [ ] `pytest.raises` for expected exceptions
- [ ] `monkeypatch` and `unittest.mock`
- [ ] Markers (`@pytest.mark.unit`, `@pytest.mark.integration`)
- [ ] Coverage with `pytest-cov`
- [ ] **Read in repo:** any test under `tests/test_services/test_bonus_platform_service/`

#### `moto` — mocks AWS in tests
- [ ] `@mock_aws` decorator
- [ ] Setting up a fake DynamoDB table in a fixture
- [ ] Why we never hit real AWS in unit tests
- [ ] **Read in repo:** `tests/test_services/test_casino_external_service/conftest.py`

#### Tooling
- [ ] **`uv`** — package manager
  - `uv sync --dev`
  - `uv add <pkg>` and `uv add --dev <pkg>`
  - `uv run <cmd>`
  - `uv lock`
- [ ] **`ruff`** — linter + formatter
  - `uvx ruff check <files>`
  - `uvx ruff format <files>`
  - `uvx ruff check --fix <files>`
  - Read `pyproject.toml` `[tool.ruff.lint]` section to see what rules are enabled
- [ ] **`basedpyright`** — type checker
- [ ] **`pre-push hooks`** — they run lint + tests on every push

### Tier 3 — Pick up over months 2-3

#### Observability
- [ ] **OpenTelemetry** basics — spans, traces, exporters
- [ ] CloudWatch Logs Insights queries
- [ ] How to find a request in logs given a `request_token`

#### Messaging
- [ ] **Apache Kafka** basics — topics, partitions, consumer groups (used in odds feed)
- [ ] **RabbitMQ / AMQP** basics (`pika`)
- [ ] **Apache Avro** schema (used for Kafka messages)

#### SQL & ORM (only if you work in casino_service)
- [ ] PostgreSQL basics — DDL, DML, JOIN, indexes
- [ ] **SQLAlchemy 2.x** — models, sessions, relationships
- [ ] **Alembic** — migrations
- [ ] `asyncpg` for async Postgres

#### Containers & K8s
- [ ] Docker basics: `Dockerfile`, `docker build`, `docker run`, layers, image caching
- [ ] Kubernetes basics: pod, deployment, service, ingress, configmap, secret
- [ ] Helm charts (only enough to read the gitops repo when needed)

#### Other libraries you'll see
- [ ] `httpx` and `requests` — HTTP clients
- [ ] `pyjwt` and `python-jose` — JWT handling
- [ ] `cryptography` and `pycryptodome` — encryption
- [ ] `pynamodb` — DynamoDB ORM (used in some services instead of raw boto3)
- [ ] `msgspec` — fast serialization (alternative to Pydantic for hot paths)

**Estimated time:** Tier 1 = 2 weeks, Tier 2 = 2 weeks, Tier 3 = ongoing as needed

---

## Phase 3 — Patterns and architecture this team uses

This is what turns "I can write Python" into "I can write *this team's* Python."

### 3.1 Read these documents end-to-end (mandatory)
- [ ] **`AGENTS.md`** — the team's engineering bible. Sections 1-10, no skipping.
  - Section 1: Branch naming
  - Section 2: Trunk-based development & linting
  - Section 3: Feature flags (mandatory for ALL net-new code)
  - Section 4: Domain-Driven Design layers
  - Section 5: DRY (and what NOT to abstract)
  - Section 6: SOLID principles
  - Section 7: REST API design
  - Section 8: Test coverage (80% minimum)
  - Section 9: Code hygiene
  - Section 10: AWS Secrets Manager review checklist
- [ ] **`CLAUDE.md`** — short PR review checklist
- [ ] **`README.md`** — local setup
- [ ] **`tests/TESTING.md`** — testing workflow
- [ ] **`services/bonus_platform_service/bonus_service_v2/ARCHITECTURE.md`** — concrete DDD example
- [ ] **`docs/openapi/README.md`** — API spec

### 3.2 Architectural concepts to internalize

#### Domain-Driven Design (DDD)
- [ ] What "domain", "ubiquitous language", "bounded context" mean
- [ ] **Entity** — object with identity (e.g. `User`, `MtsTicket`)
- [ ] **Value Object** — immutable, identity-free (e.g. `Money`, `TicketStatus`)
- [ ] **Aggregate** — cluster of entities with consistency boundary (e.g. `TicketLifecycle`)
- [ ] **Repository** — abstraction over persistence
- [ ] **Service / Use Case** — orchestrates the domain to fulfill a workflow
- [ ] **Domain Event** — something meaningful that happened
- [ ] The 4-layer structure: `domain/` → `repository/` → `service/` → `handler.py` → `lambda_function.py`
- [ ] Dependencies point inward — domain knows nothing about AWS

#### SOLID principles (with examples in `AGENTS.md`)
- [ ] **S**ingle Responsibility
- [ ] **O**pen/Closed
- [ ] **L**iskov Substitution
- [ ] **I**nterface Segregation
- [ ] **D**ependency Inversion

#### Other patterns used
- [ ] **Repository pattern**
- [ ] **Strategy pattern** (e.g. payment providers, casino aggregators)
- [ ] **Dependency Injection** via container (`bonus_service_v2/dependencies.py`)
- [ ] **Decorator pattern** (`@require_feature_flag`, `@require_whitelist`)
- [ ] **Adapter / Port** (the v2 `BrazePort`, `FeatureFlagPort`, `TCMStringsPort`)
- [ ] **Event-driven architecture** (SQS, DynamoDB Streams, Kafka)

### 3.3 Feature flags as a discipline
- [ ] **Every** new endpoint, screen, or background job ships behind a flag
- [ ] Two decorators: `@require_feature_flag` (fail-open) and `@require_whitelist` (fail-closed)
- [ ] Flag key naming: `<domain>.<feature_name>` (e.g. `sportsbook.parlay_builder`)
- [ ] Two tests required: flag-on path AND flag-off path
- [ ] Add the key to `platform-configs` DynamoDB table set to `false` before merging
- [ ] Remove dead flags after rollout — they become landmines

### 3.4 REST API conventions in this repo
- [ ] URLs are plural nouns (`/v1/users`, not `/v1/getUser`)
- [ ] Methods follow semantics (GET = safe, PUT = idempotent replace, etc.)
- [ ] Status codes are correct (don't return 200 for errors)
- [ ] Path versioning (`/v1`, `/v2`)
- [ ] Standard response envelope: `{ "success": bool, "message": str, "data": ... }`
- [ ] Errors never expose stack traces or internal paths to clients
- [ ] Validate input at the boundary, before business logic runs

**Estimated time:** 1-2 weeks of dedicated reading + months of internalizing while writing code

---

## Phase 4 — Hands-on inside this codebase

This is the most important phase. Reading > theory. Doing > reading.

### Step 1 — Get the repo running locally
- [ ] Clone the repo (already done)
- [ ] Install `uv`: `curl -LsSf https://astral.sh/uv/install.sh | sh`
- [ ] Run `uv sync --dev`
- [ ] Set up the pre-push hook: `git config core.hooksPath .githooks`
- [ ] Run the test suite: `PYTHONPATH=. uv run pytest --import-mode=importlib`
- [ ] Verify tests pass; fix any local env issues

### Step 2 — Trace a v1 endpoint end-to-end (critical exercise)
Pick `GET /v1/bonus/promotional`. Open these files in order, write down what each does:

- [ ] `services/user_service/user_management/lambda_function.py` — Lambda entry
- [ ] `services/user_service/user_management/routes.py` — route registration
- [ ] `helper/api_gateway_resolver.py` — `APIGatewayRestResolver.resolve()`
- [ ] `helper/feature_flag_decorator.py` — how the flag gate works
- [ ] `services/user_service/user_management/user/bonus.py` — `get_user_promotional_bonuses`
- [ ] `helper/responses.py` — response envelope
- [ ] DynamoDB query → response → JSON → client

**Goal:** be able to draw the full request flow on a whiteboard from memory.

### Step 3 — Trace a v2 use case end-to-end (the contrast)
Pick the referral-bonus award flow:

- [ ] `services/bonus_platform_service/bonus_service_v2/lambda_function.py` — entry router
- [ ] `services/bonus_platform_service/bonus_service_v2/handlers/bonus_processor_events/handler.py`
- [ ] `services/bonus_platform_service/bonus_service_v2/registry.py` — event registration
- [ ] `services/bonus_platform_service/bonus_service_v2/modules/referral/handlers/referral_handler.py`
- [ ] `services/bonus_platform_service/bonus_service_v2/modules/referral/use_cases/check_and_award_bonus.py`
- [ ] `services/bonus_platform_service/bonus_service_v2/modules/referral/use_cases/award_referral_bonus.py`
- [ ] `services/bonus_platform_service/bonus_service_v2/domain/models/referral.py`
- [ ] `services/bonus_platform_service/bonus_service_v2/domain/interfaces/promotion_repository.py`
- [ ] `services/bonus_platform_service/bonus_service_v2/infrastructure/persistence/referral_repository.py`
- [ ] `services/bonus_platform_service/bonus_service_v2/dependencies.py` — DI container

**Goal:** see how each layer has a single responsibility and depends only on abstractions below it.

### Step 4 — Read tests for both
- [ ] `tests/test_services/test_user_service/test_user_management/` — v1 style
- [ ] `tests/test_services/test_bonus_platform_service/test_bonus_service_v2/modules/test_referral/` — v2 style

Notice how v2 tests are smaller, more focused, and don't need to mock AWS.

### Step 5 — Run a test under the debugger
- [ ] Pick a small test, run it with `--pdb` or via your IDE's debugger
- [ ] Step through line by line
- [ ] Inspect fixture injection, see how `moto` mocks DynamoDB

### Step 6 — Find and pick a tiny first ticket
Good first-issue candidates:
- A typo in an error message
- A missing log line
- A missing test for an existing function
- A small validation tweak
- Updating an outdated comment

Goals for your first PR:
- [ ] Branch named correctly: `fix/TICKET-XYZ-description`
- [ ] Lint passes locally before push
- [ ] All tests pass
- [ ] PR fits in <400 lines of logic
- [ ] You can explain the change in one sentence

### Step 7 — Ship a small feature
After your first merged PR, take on something small but real:
- A new field on an existing endpoint
- A new query parameter
- A new event handler

Make sure it goes in **behind a feature flag** (per `AGENTS.md`).

---

## The 6-week schedule

A concrete week-by-week plan. Adjust as needed.

| Week | Focus | Deliverable |
|---|---|---|
| **1** | HTTP, REST, JSON, Git workflow, Pydantic basics; set up repo locally; read `AGENTS.md` sections 1-2-7 | Repo runs, tests pass on your machine |
| **2** | AWS service overview (Lambda, DynamoDB, SQS, S3, Cognito), `boto3` basics; trace `GET /v1/bonus/promotional` end-to-end | A whiteboard sketch of the v1 request flow |
| **3** | DynamoDB modeling deep dive (partition keys, GSIs, query vs scan); read 5 v1 handlers; finish reading `AGENTS.md` | Notes on DynamoDB access patterns in this repo |
| **4** | DDD concepts; read `bonus_service_v2/ARCHITECTURE.md`; trace one v2 use case end-to-end; SOLID principles | Whiteboard sketch comparing v1 vs v2 layouts |
| **5** | `pytest` + `moto`; read existing tests; write a test for an existing untested function (under guidance) | One test PR merged |
| **6** | Pick up a small ticket; ship a PR through CI; iterate based on review feedback | First feature PR merged |

After week 6, the curve flattens. From there: depth in your team's services, breadth as new tickets demand it.

---

## Reading list

### Books (priority order)
1. **"Architecture Patterns with Python"** by Percival & Gregory — DDD + clean architecture in Python.
   📖 Free online: https://www.cosmicpython.com/
   *Read first. Single most relevant book for this codebase.*

2. **"The DynamoDB Book"** by Alex DeBrie — the only DynamoDB book that matters.
   *Read second. Will fix all your DynamoDB confusion.*

3. **"Designing Data-Intensive Applications"** by Martin Kleppmann — best backend book ever written.
   *Read at 1 chapter every 1-2 weeks over months. Foundational.*

4. **"Domain-Driven Design Distilled"** by Vaughn Vernon — short, readable DDD primer.
   *~150 pages. Good complement to Cosmic Python.*

5. **"Fluent Python"** (2nd ed) by Luciano Ramalho — Python deep dive.
   *Reference book; dip in as you encounter advanced patterns.*

### Online courses & tutorials
- [FastAPI official tutorial](https://fastapi.tiangolo.com/tutorial/)
- [Pydantic docs](https://docs.pydantic.dev/latest/)
- [boto3 docs](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)
- [pytest docs](https://docs.pytest.org/)
- [12-factor app](https://12factor.net/)

### Videos / talks
- **Rick Houlihan — DynamoDB Advanced Design Patterns** (any AWS re:Invent year). YouTube.
- **Martin Fowler — DDD talks** on YouTube
- **ArjanCodes — Python design patterns** YouTube channel

### Repo-internal docs
- [`AGENTS.md`](./AGENTS.md)
- [`CLAUDE.md`](./CLAUDE.md)
- [`README.md`](./README.md)
- [`tests/TESTING.md`](./tests/TESTING.md)
- [`docs/openapi/README.md`](./docs/openapi/README.md)
- [`services/bonus_platform_service/bonus_service_v2/ARCHITECTURE.md`](./services/bonus_platform_service/bonus_service_v2/ARCHITECTURE.md)
- [`docs/service_code_refactors/`](./docs/service_code_refactors/) — refactor write-ups

---

## Common pitfalls to avoid

### Learning pitfalls
- ❌ **Trying to learn every AWS service.** There are 200+. You need ~10 deeply, the rest by name only.
- ❌ **Memorizing instead of understanding.** Concepts > syntax. You can always look up syntax.
- ❌ **Tutorial hell.** After 2-3 tutorials on a topic, start writing real code. More tutorials won't help.
- ❌ **Comparing yourself to seniors.** They have 10+ years of context. You're 6 weeks in.
- ❌ **Over-engineering early code.** Get something working first. Refactor later.

### Coding pitfalls (specific to this repo)
- ❌ **Using `Table.scan()`** — almost always wrong, expensive, slow.
- ❌ **Putting business logic in route handlers** — extract to a service.
- ❌ **Importing `boto3` in domain code** — violates DDD layering.
- ❌ **Skipping feature flags on new code** — PR will be rejected.
- ❌ **Using `FilterExpression` to "save reads"** — it doesn't; it just trims the response.
- ❌ **Hardcoded magic strings** — use enums or const objects.
- ❌ **Catching `Exception` and `pass`-ing** — silent failures = production landmines.
- ❌ **Missing tests for both flag-on and flag-off paths.**
- ❌ **Branch named `myname/feature-x`** — must be `feat/TICKET-123-description`.
- ❌ **Pushing without running `ruff` and tests locally** — CI will reject.

### Soft-skill pitfalls
- ❌ **Asking "how do I do X?" before trying yourself for 30 mins.**
- ❌ **Not asking after struggling for 2 hours.** Time-box your stuck-ness.
- ❌ **Skipping PR feedback because it stings.** That's where you learn fastest.
- ❌ **Refusing to read existing code.** It's the cheapest, highest-yield learning resource you have.

---

## Mindset and habits

### Daily habits
- **Read code before writing it.** Spend the first 30 mins of each task reading what's already there.
- **Write down what confused you.** Build a personal glossary as you go.
- **Use a notebook.** Sketch architecture, draw request flows, list questions.
- **Run small experiments.** Open a Python REPL, poke at boto3 with `moto`, see what happens.

### Weekly habits
- **Review your own merged PRs.** Re-read them after a week. You'll spot improvements.
- **Read one PR by a senior on the team.** Understand *why* they made the choices they did.
- **Pick one technology to go deeper on.** This week DynamoDB, next week Pydantic, etc.

### Mental models worth adopting
- **"It's not magic, it's just code I haven't read yet."** Everything in this repo can be traced step by step.
- **"The framework doesn't matter as much as the patterns."** FastAPI vs in-house resolver — both serve the same purpose. Patterns transfer; frameworks don't.
- **"Optimize for readability, not cleverness."** Boring code is good code.
- **"If it's hard to test, the design is wrong."** Untestable code = leaky abstractions.
- **"Senior engineers are juniors who never stopped reading code."** That's literally the difference.

### When you're stuck
1. Re-read the error message carefully.
2. Read the docs for the function/library involved.
3. Search the codebase for similar patterns.
4. Read the test file — it often shows expected usage.
5. Take a break. Walk away. Come back fresh.
6. Ask in Slack (after the above) — include what you tried.

---

## Quick reference cheat sheet

### Common commands

```bash
# Setup
uv sync --dev
git config core.hooksPath .githooks

# Run tests
PYTHONPATH=. uv run pytest --import-mode=importlib tests/ -v

# Run specific test
PYTHONPATH=. uv run pytest tests/test_services/test_xxx/test_yyy.py::test_specific -v

# With coverage
PYTHONPATH=. uv run pytest --cov=services --cov-report=html tests/ -v

# Lint
uvx ruff check <changed-files>
uvx ruff format <changed-files>
uvx ruff check --fix <changed-files>

# Skip pre-push checks (use sparingly)
SKIP_RUFF=1 git push
SKIP_TESTS=1 git push
```

### File locations
| What | Where |
|---|---|
| Engineering standards | `AGENTS.md` |
| Local setup | `README.md` |
| Testing guide | `tests/TESTING.md` |
| Routing helper | `helper/api_gateway_resolver.py` |
| Response envelope | `helper/responses.py` |
| Feature flag decorator | `helper/feature_flag_decorator.py` |
| OpenAPI spec | `openapi.yaml`, `docs/openapi/sportsbook-openapi.yaml` |
| v1 example handler | `services/user_service/user_management/user/bonus.py` |
| v2 example architecture | `services/bonus_platform_service/bonus_service_v2/` |

### Branch naming (mandatory)
| Type | Pattern |
|---|---|
| Feature | `feat/TICKET-123-description` |
| Bug fix | `fix/TICKET-123-description` |
| Chore | `chore/description` |

### HTTP status codes (memorize)
| Code | Use |
|---|---|
| 200 | OK (GET, PATCH, PUT, DELETE success) |
| 201 | Created (successful POST) |
| 204 | No content |
| 400 | Bad request |
| 401 | Not authenticated |
| 403 | Forbidden / feature-flag block |
| 404 | Not found |
| 409 | Conflict |
| 422 | Validation error |
| 500 | Server error |
| 503 | Dependency unavailable |

### DynamoDB cheat sheet
- `Key("user_id").eq(x)` → KeyConditionExpression (uses index)
- `Attr("status").eq("active")` → FilterExpression (post-filter)
- `query()` → use partition key
- `scan()` → reads everything (avoid)
- `update_item()` with `UpdateExpression` → atomic update
- `ConditionExpression` → optimistic concurrency
- GSI → query by non-primary key

---

## Progress tracker

Keep this honest. Tick off as you go.

### Phase milestones
- [ ] Phase 0 complete — fundamentals solid
- [ ] Phase 1 complete — backend concepts understood
- [ ] Phase 2 Tier 1 complete — can read any v1 handler
- [ ] Phase 2 Tier 2 complete — can write tests
- [ ] Phase 3 complete — `AGENTS.md` internalized
- [ ] Phase 4 Step 2 complete — traced v1 endpoint
- [ ] Phase 4 Step 3 complete — traced v2 use case
- [ ] Phase 4 Step 6 complete — first PR merged
- [ ] Phase 4 Step 7 complete — first feature shipped

### After 3 months you should be able to
- [ ] Read any handler in this repo and explain what it does
- [ ] Write a new endpoint following v2 (DDD) patterns
- [ ] Write tests with `pytest` + `moto` for new code
- [ ] Add a feature flag and test both paths
- [ ] Open a well-structured PR that passes CI
- [ ] Review someone else's PR and leave useful comments
- [ ] Debug a production issue using CloudWatch logs

### After 6 months you should be able to
- [ ] Refactor v1 procedural code into v2 DDD layout
- [ ] Design a new DynamoDB table with the right access patterns
- [ ] Lead a small feature from spec → design → implementation → ship
- [ ] Mentor someone else through their first weeks

---

## Final words

You'll feel overwhelmed in the first month. That's normal. Everyone did. The volume of unfamiliar acronyms (Lambda, EKS, GSI, DDD, SQS, JWT, IAM, OTEL...) is genuinely huge.

Don't try to learn it all. Learn what's in front of you. The next ticket teaches you the next thing.

In 6 weeks you'll be shipping.
In 6 months you'll forget that any of this once felt hard.

Good luck. The repo is well-structured for someone willing to read it carefully.

---

*Last updated: roadmap created during ramp-up. Update as your understanding evolves.*
