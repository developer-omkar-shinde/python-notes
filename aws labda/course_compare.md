# AWS Lambda Course Recommendation for `trivelta-backend-services`

A comparison of two Udemy AWS Lambda courses against the actual stack used in this repo (specifically `bonus_platform_service`), to decide which to buy.

---

## What This Repo Actually Uses

| Need | Used Where |
|------|-----------|
| Python Lambda handlers | Every `handlers/*/handler.py` |
| `boto3` (Client + Resource) | All repositories (`infrastructure/persistence/`) |
| API Gateway events | `bonus_user_api`, `bonus_admin_api` |
| **SQS event processing** | `main.py` SQSConsumer for 4 queues (CRITICAL) |
| **DynamoDB** | Main DB, also DynamoDB streams |
| EventBridge | Campaign events relay |
| CloudWatch logs | All services |
| Environment variables | Config + secrets |
| IAM roles | Lambda execution role |
| Secrets Manager | `helper/integrations` |

**Less relevant for daily work:**

- CloudFormation, CDK (CI/CD handles deploys)
- Cognito Authorizers (auth is handled centrally)
- VPC config (infra team)
- Bedrock / GenAI

---

## Course #1 — AWS Lambda, Python (Boto3) & Serverless — Beginner to Advanced

**Instructor:** Rahul Trisal
**Length:** 9 hours
**Verdict:** 🥇 Best primary course for this repo.

### Strong matches

- Python + Boto3 (Client and Resource) — exactly what this repo uses
- Lambda handler, events & context — core knowledge
- Lambda + DynamoDB hands-on — directly applicable
- Lambda + S3 + DynamoDB — similar pattern to this repo
- EventBridge — used here for campaign events
- API Gateway deep dive — used by user/admin APIs
- CloudWatch logs/metrics — used in production
- Environment variables — used heavily here
- Lambda invocation models, concurrency, limits — important for prod

### Less relevant (~30% of course)

- CloudFormation (CI handles it)
- CDK (CI handles it)
- Cognito + Lambda Authorizers (auth is centralized)
- Bedrock / GenAI (not used here)

### Missing

- **No SQS section** — this is a gap for this repo
- No Lambda + SQS event handling

### Sections to skip

- CloudFormation (Section 13)
- CDK (Section 12)
- Cognito (parts of Section 9)
- Bedrock/GenAI (Section 10)

**Covers ~65% of what you'll actually use.** Python + Boto3 + DynamoDB + EventBridge + CloudWatch focus is exactly right.

---

## Course #2 — AWS Lambda: A Practical Guide

**Instructor:** Daniel Galati
**Length:** 7.5 hours
**Verdict:** 🥈 Excellent complement to Course #1.

### Strong matches

- **Has SQS hands-on (47 min)** — huge for this repo
- Synchronous vs Async invocations — important
- Concurrency, throttling, reserved/provisioned concurrency — production-critical
- DLQ (Dead Letter Queues) — used here
- CloudWatch logs, metric filters — used here
- Versions, aliases, env vars
- API Gateway REST hands-on
- X-Ray tracing
- Lambda Insights

### Less relevant

- Heavy CDK focus
- Lambda Layers (not used much here)
- Docker integration

### Missing / Weak

- Not Python-specific (language-agnostic)
- No deep Boto3 coverage
- No DynamoDB Boto3 patterns
- Less hands-on with services

### Sections to skip (optional)

- Lambda Layers
- Docker
- Database Proxies
- X-Ray (optional but useful)

**Has the SQS coverage Course #1 lacks. More "how Lambda actually works in production" — concurrency, throttling, monitoring.**

---

## Combined Coverage

| Topic | Course #1 | Course #2 |
|-------|----------|----------|
| Python + Boto3 | ✅ Strong | ❌ Weak |
| DynamoDB hands-on | ✅ Strong | ⚠️ Light |
| API Gateway | ✅ Strong | ✅ Good |
| **SQS** | ❌ Missing | ✅ **Strong** |
| EventBridge | ✅ Yes | ✅ Yes |
| CloudWatch | ✅ Yes | ✅ Strong |
| Concurrency/Scaling | ⚠️ Light | ✅ **Strong** |
| DLQ | ❌ No | ✅ Yes |
| Production tips | ⚠️ Light | ✅ Strong |

Together, Course #1 + Course #2 cover ~90% of what you need.

---

## Recommended Order

1. **Course #1** (Rahul Trisal) — 9 hrs — Foundation: Python + Boto3 + Lambda + DynamoDB + API Gateway + EventBridge
2. **Course #2** (Daniel Galati) — 7.5 hrs — Production focus: SQS + concurrency + monitoring + DLQ

Skipping non-relevant sections saves ~5–6 hours.

### After the Lambda courses

- FastAPI course (e.g. *FastAPI - The Complete Course 2026 (Beginner + Advanced)*)
- Pydantic V2 docs — read directly while exploring this repo's schemas
- Pytest practice — write tests in this repo

**Total time investment:** ~25–30 hours of video, then start contributing.

---

## Repo Files to Read While Learning

- `services/bonus_platform_service/bonus_service_v2/main.py` — FastAPI app + SQS consumers
- `services/bonus_platform_service/bonus_service_v2/ARCHITECTURE.md` — Service design
- `services/bonus_platform_service/bonus_service_v2/handlers/bonus_user_api/routes.py` — API routes
- `services/bonus_platform_service/bonus_service_v2/lambda_function.py` — Lambda entry point
- `helper/feature_flag_decorator.py` — Feature flag pattern
- `AGENTS.md` — Engineering standards (DDD, feature flags, testing)
