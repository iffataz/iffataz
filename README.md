# Iffat Abdul Azeez

**Software Engineer** · Melbourne, Australia

I build production software: backend services, API integrations, data pipelines, and AI systems. Currently a Junior Software Engineer at **IT Mate Solutions**, where I designed and built an internal operations platform now used daily across ~100 client organisations. Previously interned at **Origin Energy** on a data platform serving 500+ commercial clients.

Bachelor of Computer Science, **Monash University** (WAM 84.5).

The thing I care most about is software that holds up in production. Most of what's below has tests, CI, and a deployment story attached.

---

## Featured Projects

### 🤖 [Ask My Docs](https://github.com/iffataz/Ask-My-Docs) — self-correcting agentic RAG
A document Q&A system where the agent grades its own retrieval quality and retries when the context isn't good enough.

The interesting parts aren't the RAG basics, they're the guardrails. The retry loop is a **provably bounded invariant** — the cap is written in exactly one node and only read by routing, so termination is a property of the code rather than a convention, verified by an adversarial test that drives an always-insufficient grader. Three separate prompt paths mean exhausted retrieval returns *"the documents don't contain enough information"* instead of model-knowledge filler.

One pipeline behind three surfaces with zero duplicated logic: a FastAPI SSE-streaming API, an **MCP server** for Claude Desktop, and a Next.js UI, with the LLM, embedder and vector store behind swappable interfaces.

`Python` `FastAPI` `LangGraph` `ChromaDB` `MCP` `Next.js` `Docker` · 29 tests · `mypy --strict` and `ruff` clean

> Favourite bug: a chunking config mismatch where character-based splitting had inherited token-based defaults, quartering chunk size and shredding PDF headings from their tables. Every test stayed green. Found it by evaluating against real documents instead of fixtures.

### ⚡ [OctoPulse](https://github.com/iffataz) — batch + streaming data platform
A production-style medallion-architecture platform over GitHub's public event firehose, running at roughly **165,000 events/hour**.

Streaming path: rate-limit-aware producer → **Kafka** (KRaft) → **Spark Structured Streaming** → partitioned Parquet plus per-minute aggregates. Batch path: Dockerised **Airflow** hourly DAG plus a parameterised backfill DAG using dynamic task mapping, feeding a PySpark transform with a typed schema contract into **BigQuery**. Warehouse loads are idempotent via delete-then-append, so reruns and multi-day backfills never double-count. Infrastructure as code with **Terraform** under a least-privilege service account.

`Python` `PySpark` `Kafka` `Airflow` `BigQuery` `Terraform` `GCP` `Docker` · TDD, 60+ tests · verified live on millions of real events

### 📦 [ParcelTracker](https://github.com/iffataz) — real-time tracking, built test-first
A courier simulation moving parcels between real Australian depots, with every scan persisted and pushed live to a Leaflet map over **SignalR**.

Framework-free domain layer: an aggregate enforcing a status state machine that rejects illegal transitions, with encapsulated collections. The background simulator is unit-testable because its dependencies are inverted — a `TimeProvider` fake clock and a recording publisher make time-based behaviour deterministic.

`C#` `ASP.NET Core` `EF Core` `PostgreSQL` `SignalR` `xUnit` `Testcontainers` `Docker` `Azure`

> Testing earned its keep here: caught a pre-set client-side key making EF Core issue `UPDATE` instead of `INSERT`, and child collections loading in database order and scrambling route indexing after a round-trip.

### 🍽️ [Atlas](https://github.com/iffataz/Atlas) — voice-first LLM product · **[live demo](https://atlas-ten-peach.vercel.app)**
Speak your dietary preferences, get back a structured 7-day plan with an auto-aggregated shopping list, refinable by voice.

Built around an explicit UI state machine with optimistic updates that roll back on failure, an in-flight guard plus `AbortController` so a paid LLM call can't double-fire, and `zod` validating both the incoming request *and* the model's own response before anything persists. Accessible by design: `sr-only` labels, `role="alert"` live regions, keyboard-operable controls, and an identical typed fallback for browsers without the Web Speech API.

`Next.js` `TypeScript` `Groq` `MongoDB` `zod` `Upstash Redis` `Vitest`

### 🛋️ [Decora](https://github.com/iffataz/Decora) — multimodal vision pipeline · **[live demo](https://decora-sooty.vercel.app)** · 🏆 UniHack 2024 finalist
Upload a photo of a room, and Gemini vision reads its aesthetic and returns ranked furniture matches, each with a one-sentence reason it fits.

The engineering story is the two designs I got wrong first. Text-proxy scoring lost the visual signal and clustered every result at 7–9. Pairwise visual scoring effectively compared two rooms instead of ranking products. Batched comparative scoring — room photo plus all ten candidates in one numbered call — was the one that produced genuine ranking. Also cut 10–60s of latency by disabling unnecessary model reasoning, parallelising image fetches, and adding timeouts, backoff retry and a model fallback.

`Python` `Flask` `Gemini vision` `Pydantic` `Vercel`

### 🌪️ [Extreme Weather Australia](https://github.com/iffataz/extreme-weather-australia) — ETL & data quality
End-to-end pipeline ingesting Australian disaster and extreme-weather events, with a dedicated data-quality layer that canonicalises regions and overrides known-bad upstream records rather than trusting the source. Idempotent `ON CONFLICT DO UPDATE` upserts across PostgreSQL and SQLite, analytics via SQL window functions, tests in CI on every push.

`Python` `SQLAlchemy` `pandas` `PostgreSQL` `Typer` `GitHub Actions`

---

## Experience

| | | |
|---|---|---|
| **Junior Software Engineer** | IT Mate Solutions | Mar 2026 – Present |
| **Software Engineer (Intern)** | Origin Energy · Insights & Data Products | Jul – Nov 2025 |
| **Software Engineer** | Monash Deep Neuron · AI Division | Jan 2025 – Jun 2026 |

A few things I've shipped at work: an integration layer unifying five vendor APIs behind one pluggable interface with four different auth schemes; AES-256-GCM credential encryption with JWT and role-based access control; event-driven services on AWS Lambda/SQS/SNS with retry and dead-letter handling; and a Redshift query taken from over two minutes to under ten seconds by actually reading the execution plan. I also grew a test suite to 229 tests on a team with no separate QA function.

---

## Technical Skills

**Languages** · `Python` `TypeScript` `JavaScript` `Java` `C#` `SQL` `Bash`

**Backend** · `FastAPI` `Flask` `Node.js` `Express` `ASP.NET Core` `EF Core` · REST API design · SSE streaming · real-time (SignalR/WebSockets) · event-driven architecture

**Frontend** · `React` `Next.js` `TypeScript` `Tailwind CSS` · accessible & responsive UI · UI state machines

**AI / ML** · `LangGraph` `MCP` `PyTorch` · agentic workflows · RAG · LLM APIs (Anthropic, OpenAI, Gemini, Groq) · structured output & tool-calling · vector stores · prompt-injection defence · DQN, GANs

**Data** · `Kafka` `Spark` `Airflow` `BigQuery` `pandas` · ETL · medallion architecture · idempotent & incremental loading · backfills

**Databases** · `PostgreSQL` `MongoDB` `SQLite` `Redshift` `ChromaDB` · schema design · query optimisation · execution-plan tuning

**Cloud & DevOps** · `AWS` `GCP` `Azure` `Docker` `Terraform` `GitHub Actions` · CI/CD · structured logging · least-privilege IAM

**Testing & Quality** · `pytest` `Vitest` `xUnit` `Testcontainers` · TDD · integration testing · `mypy --strict` · `ruff`

---

## Get in touch

Open to software engineering roles in Melbourne or remote.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/iffat-abdul-azeez/)
[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:iffatazeez@gmail.com)
