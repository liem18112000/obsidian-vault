---
ai_hash: 60020a3708182129
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-24
entities:
- leo-customer360
- dbmate
- Postgres migrations
- Alembic
- Atlas
- migrate lint
- CI
- Python
- FastAPI
- SQLAlchemy 2.0
- queries
- schema
- raw SQL
- database-init/database-schema.sql
- RLS tenant policies
- PostGIS
- pgvector
- DO $$ blocks
- customer360 schema
- Go binary
- plain .sql up/down files
- VNG Cloud VM
- SQLAlchemy ORM models
- CREATE EXTENSION
- Flyway
- Liquibase
- JVM
- VNG footprint goal
- Docker Compose
- one-shot migrate Compose service
- API
- k8s Job
- initContainer
- VKS
- docs/research-tech/sql-migration-tool-recommendation.md
- Postgres docker-entrypoint-initdb.d runs only once on an empty volume
source: session 2026-08-24 migration tooling research
status: seedling
tags:
- leo-customer360
- postgres
- migrations
- dbmate
- decision
title: leo-customer360 uses dbmate for Postgres migrations, not Alembic
type: argument
---

# leo-customer360 uses dbmate for Postgres migrations, not Alembic

For `leo-customer360` we run PostgreSQL schema migrations with **dbmate** (a single static Go binary, run-and-exit, plain `.sql` up/down files) plus **Atlas `migrate lint` in CI only** as a destructive-change/lock guardrail. This holds even though every service is Python/FastAPI + SQLAlchemy 2.0 — because SQLAlchemy is used for *queries*, and the schema itself is **raw SQL** (a hand-written `database-init/database-schema.sql` with RLS tenant policies, PostGIS, pgvector, `DO $$` blocks, custom `customer360` schema).

**Why dbmate:** raw-SQL migrations run verbatim (no DDL subset to fight), it maps onto the `NNN_*.sql` convention already started in the repo, and its ~8 MB footprint fits the VNG Cloud VM least-RAM/CPU goal.

**Why NOT Alembic** (the obvious Python choice, deliberately declined): its value is `--autogenerate` diffing SQLAlchemy *ORM models* — but our schema isn't in ORM models, and Alembic can't model RLS / PostGIS / pgvector / `CREATE EXTENSION` / DO-blocks. You'd write all of it as `op.execute("<raw sql>")` inside Python files — paying Python+Alembic overhead to run raw SQL, strictly worse than dbmate. Revisit only if the schema ever moves into ORM models.

**Why NOT Flyway/Liquibase:** JVM (200–400 MB RAM/run, ~300 MB image) contradicts the VNG footprint goal; their best features are paid tiers anyway.

**Deploy target:** the project runs on **Docker Compose today** (not K8s), so dbmate runs as a one-shot `migrate` Compose service gated by `depends_on: { migrate: { condition: service_completed_successfully } }` before the API. The same image runs as a k8s `Job`/initContainer *if* we later move to VKS — migration files don't change.

The full research write-up lives at `docs/research-tech/sql-migration-tool-recommendation.md`. Core problem it solves: see [[Postgres docker-entrypoint-initdb.d runs only once on an empty volume]].

## Related

- [[Postgres docker-entrypoint-initdb.d runs only once on an empty volume]]

%% ai-graph-start %%

**Related notes:**
- [[LEO CDP schema migrations are ordered plain SQL, not dbmate or alembic]]
- [[Never pipe a dbmate migration file through a raw psql replay — its down section is destructive]]
- [[A self-contained migration parity test is only useful during the cutover]]
- [[Postgres docker-entrypoint-initdb.d runs only once on an empty volume]]
- [[leo-customer360 applies DB schema via two paths that must stay in sync]]

**Relations:**
- leo-customer360 — *uses* — dbmate
- dbmate — *for* — Postgres migrations
- leo-customer360 — *does not use* — Alembic
- leo-customer360 — *runs* — Postgres migrations
- leo-customer360 — *uses* — Atlas
- Atlas — *uses* — migrate lint
- migrate lint — *used in* — CI
- migrate lint — *is a* — destructive-change/lock guardrail
- leo-customer360 — *services are* — Python
- leo-customer360 — *services use* — FastAPI
- leo-customer360 — *services use* — SQLAlchemy 2.0
- SQLAlchemy 2.0 — *used for* — queries
- schema — *is* — raw SQL
- raw SQL — *is in* — database-init/database-schema.sql
- database-init/database-schema.sql — *includes* — RLS tenant policies
- database-init/database-schema.sql — *includes* — PostGIS
- database-init/database-schema.sql — *includes* — pgvector
- database-init/database-schema.sql — *includes* — DO $$ blocks
- database-init/database-schema.sql — *includes* — customer360 schema
- dbmate — *is a* — Go binary
- dbmate — *uses* — plain .sql up/down files
- dbmate — *runs* — raw SQL
- dbmate — *has* — ~8 MB footprint
- ~8 MB footprint — *fits* — VNG Cloud VM
- Alembic — *value is* — --autogenerate diffing SQLAlchemy ORM models
- schema — *is not in* — SQLAlchemy ORM models
- Alembic — *cannot model* — RLS tenant policies
- Alembic — *cannot model* — PostGIS
- Alembic — *cannot model* — pgvector
- Alembic — *cannot model* — CREATE EXTENSION
- Alembic — *cannot model* — DO $$ blocks
- Flyway — *is* — JVM
- Liquibase — *is* — JVM
- JVM — *contradicts* — VNG footprint goal
- leo-customer360 — *runs on* — Docker Compose
- dbmate — *runs as* — one-shot migrate Compose service
- dbmate — *runs before* — API
- dbmate — *image runs as* — k8s Job
- dbmate — *image runs as* — initContainer
- k8s Job — *for* — VKS
- initContainer — *for* — VKS
- research write-up — *lives at* — docs/research-tech/sql-migration-tool-recommendation.md
- docs/research-tech/sql-migration-tool-recommendation.md — *solves* — Postgres docker-entrypoint-initdb.d runs only once on an empty volume

%% ai-graph-end %%