# Example Output Walkthrough

This file shows what the skill produces when run against [sample-jd.md](sample-jd.md) for a hypothetical mid-level backend candidate whose resume includes ~2.5 years of Python/PostgreSQL work but limited exposure to async workers (Celery), Kubernetes, and AWS.

## What gets created

```
backend-labs/
├── PLATFORM.md                 # one-line file: "linux-debian"
├── theory/
│   ├── 01-celery-fundamentals.md     # 5-section theory write-up
│   ├── 02-postgres-indexing.md
│   ├── 03-rest-api-design.md
│   ├── 04-redis-patterns.md
│   ├── 05-docker-for-backend.md
│   ├── 06-kubernetes-basics.md
│   └── 07-observability-sentry-prometheus.md
├── labs/
│   ├── lab01-celery-basics/         # hands-on Celery lab
│   │   ├── README.md                # commands tailored to Linux Debian/Ubuntu
│   │   ├── docker-compose.yml       # Redis + worker + Flower
│   │   ├── app/
│   │   │   ├── tasks.py
│   │   │   └── main.py
│   │   └── requirements.txt
│   ├── lab02-postgres-explain/      # query optimisation lab
│   ├── lab03-fastapi-openapi/       # API design + OpenAPI generation
│   ├── lab04-redis-caching/         # cache-aside, pub/sub
│   ├── lab05-docker-compose/        # multi-service local stack
│   ├── lab06-kubernetes-basics/     # kubectl + Helm intro on kind
│   └── lab07-observability-stack/   # Sentry + Prometheus + Grafana locally
├── THEORY-CHEATSHEET.md             # ~200 lines, all 7 topics summarised
└── QUICKQA.md                       # ~180 one-liner Q&A entries

interview-prep/
└── northstar-backend-2026-06-15.md   # ~250-line prep file
```

## What the prep file looks like (excerpt)

```markdown
# NorthStar SaaS — Backend Engineer interview 2026-06-15

## TL;DR of the company

60-person B2B startup, customer onboarding platform for mid-market SaaS.
Engineering team of 12, three squads. The role is on Onboarding Core
(workflow engine + customer-facing API).

What this means for the interview:
- Small company → expect breadth over depth; the engineer will touch
  multiple parts of the stack
- B2B SaaS → multi-tenancy, customer-facing reliability matter
- 45 min, no live coding → conversation about past experience and
  technical reasoning

## Interview behaviour

- **Format**: 45 min total, 10 min about company, 25 min experience +
  technical, 10 min for your questions
- **Take-home assignment** follows if both sides want to proceed
- **Tactic**: bring specific examples from your last 2-3 production
  features. Mention the trade-offs you considered, not just what you built.
- **Main thing**: show backend judgment — when to add a cache, when to
  add an index, when to extract a service vs keep a monolith

## Tell-me-about-yourself (75-90 seconds)

> Full delivery cycle has to be spelled out

**Script:**

"My name is [Name], I'm a backend engineer with 2.5 years of production
experience in Python. For the last 18 months at [Company A] — a logistics
SaaS — I've been building features end-to-end: API design with FastAPI,
PostgreSQL schema and queries, background workers, and the deployment
side through our GitHub Actions pipeline. Before that, 1 year at
[Company B] working on an internal admin tool — Django, smaller scale,
but where I learned to write code that other engineers can read and
extend. Outside of work I keep a few side projects in Python and Rust,
nothing big. Looking for now: a team where the backend matters to the
product, not just plumbing — somewhere I can grow into a senior role over
the next year or two."

(~140 words, ~75 seconds at normal reading pace)

## JD → experience mapping

| JD requirement | My experience | Where specifically |
|----------------|---------------|---------------------|
| 2+ years production Python | 2.5 years, FastAPI primary | Company A (current) |
| PostgreSQL — queries, indexes, migrations | Daily — schema design, slow query investigation, alembic | Company A |
| REST API + OpenAPI | FastAPI auto-generated OpenAPI; designed 30+ endpoints | Company A |
| Docker for local dev | Yes, docker-compose for every project | Company A + side projects |
| Redis (caching, queues) | Caching yes (cache-aside), queues only through FastAPI background tasks | Company A — concept solid, no Celery yet |
| Git, PR workflow | Daily, including code review | Company A + B |
| Celery / task queue | Background tasks via FastAPI only, no Celery | Gap — see weak spots |
| Kubernetes basics | Read about it; not used in prod | Gap — see weak spots |
| AWS — ECS, RDS, S3, CloudWatch | S3 yes (file uploads), ECS/RDS/CloudWatch no — our stack is GCP | Partial |
| Sentry / Datadog | Sentry yes (integrated in current project) | Company A |
| Type hints / mypy | Type hints yes; mypy strict mode in current codebase | Company A |

## Top-10 likely questions

### 1. "Walk me through a feature you shipped end-to-end"

**Answer skeleton:**
- Pick one feature where the trade-off conversation was interesting
  (e.g. "we needed search across 50M records — chose Postgres full-text
  over Elasticsearch because the cost/maintenance wasn't worth it yet")
- Talk through: requirement → design → migration → API → tests → rollout
- Mention what you'd do differently in hindsight

### 2. "How do you decide when to add an index vs rewrite a query?"

**Answer skeleton:**
- First measure: `EXPLAIN ANALYZE` on representative data
- Index when the query is read-heavy and the column has good selectivity
- Rewrite when the plan shows a fundamental issue (Cartesian, missing
  join condition, N+1 from ORM)
- Always check `pg_stat_user_indexes` to find unused indexes before adding more

[8 more questions with similar structure]

## 2-3 STAR+R stories under this interview

**Main**: Slow query investigation at [Company A]
- When: questions about Postgres, debugging, performance
- Anchor: 30-second p95 → 200ms p95 by changing a single composite index

**Backup**: Multi-tenant data isolation
- When: questions about multi-tenancy or data leaks
- Anchor: row-level security in Postgres + tenant_id check in every query

## Weak spots — preventive answers

### "Have you used Celery in production?"

> "Not Celery specifically — at [Company A] we use FastAPI's BackgroundTasks
> for the lightweight stuff. I know the concept of a broker + worker + result
> backend, and I ran a Celery lab last week — happy to walk through how I'd
> set it up for a new service if that's helpful."

### "What about Kubernetes — have you operated it?"

> "Not in production. Our team uses Cloud Run on GCP. I've done the basics —
> kubectl, deployments, services, ingress — on a local kind cluster while
> studying for this interview. I can talk through how I'd structure
> manifests, but I'm honest that day-1 I'd be learning."

### "AWS experience — ECS, RDS, CloudWatch?"

> "Our infra is on GCP — Cloud Run, Cloud SQL, Cloud Logging. The concepts
> map: ECS ≈ Cloud Run, RDS ≈ Cloud SQL, CloudWatch ≈ Cloud Logging. I'd
> need a week to get fluent in the AWS console; the mental model is there."

## 5 questions to ask them

1. **What does a typical week look like for someone on Onboarding Core?**
   Trying to understand if it's mostly feature work, mostly maintenance,
   or roughly split.
2. **How is the on-call rotation structured?** What's a typical alert
   that wakes someone up, and how often does it happen?
3. **Tech debt** — what's the worst piece of the codebase right now, and
   is there time allocated to fix it?
4. **The take-home assignment** — what does it look like and how is it
   evaluated? Just so I can plan my time.
5. **Why are you hiring** — is this a new headcount or a backfill?

## Morning-of checklist

- [ ] Re-read this prep file once
- [ ] Run "tell me about yourself" out loud twice, time it (target: 75-90 sec)
- [ ] Open backend-labs/THEORY-CHEATSHEET.md — 20-minute skim
- [ ] Open backend-labs/QUICKQA.md — let the one-liners pass by
- [ ] Pick 3-4 questions to ask them (from list above)
- [ ] Camera/mic check 15 min before
- [ ] Coffee + water within reach

## After the interview

- [ ] Write down impressions and any questions they actually asked
- [ ] Update practice/PROGRESS.md
- [ ] Anything new they asked → add to backend-labs/QUICKQA.md
```

## What the QUICKQA one-liner format looks like

```markdown
# Quick Q&A — Backend interview

> Format: question → answer on one line. Open in your editor right
> before the call, let your eyes pass by to refresh associations.

## 1. Celery fundamentals

- **What is Celery?** — A distributed task queue for Python. Producers
  send tasks to a broker (Redis/RabbitMQ); workers consume and execute;
  results optionally stored in a result backend.
- **Broker vs result backend?** — Broker is the queue (Redis, RabbitMQ);
  result backend stores task outcomes (Redis, DB) so callers can poll.
- **`@app.task` decorator does what?** — Registers a function as a Celery
  task. Adds `.delay()` and `.apply_async()` methods.
- **`delay()` vs `apply_async()`?** — `delay()` is shorthand for
  `apply_async()` with positional args only. Use `apply_async()` when
  you need countdown, eta, queue, or retry options.

[10-15 more one-liners per topic]
```

---

## Reading order before the interview

1. Read each `theory/NN-topic.md` once (1-2 hours total, the night before)
2. Run each `labs/labNN-topic/` hands-on (2-4 hours total, the night before
   — labs are shorter for a backend role than for a DevOps one)
3. Morning of: `THEORY-CHEATSHEET.md` (20 minutes)
4. 5 minutes before: `QUICKQA.md` in your editor of choice
5. During the call: the prep file open in a second monitor (if remote)

---

> The names, companies, and stack choices in this example are synthetic.
> Replace with your own when you run the skill against a real JD.
