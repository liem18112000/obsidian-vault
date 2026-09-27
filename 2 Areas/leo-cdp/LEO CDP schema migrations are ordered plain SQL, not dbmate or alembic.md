---
ai_hash: 8c814c1873950811
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-26
entities:
- LEO CDP
- LEO CDP schema migrations
- ordered plain SQL files
- dbmate
- alembic
- LEO Customer 360
- PostgreSQL schema
- deployments/postgres/run-sql.sh
- deployments/postgres/**/*.sql
- database-init/{database-schema, init-core-database, data-view-for-llm}.sql
- database-init/migrations/
- customer360-api
- alembic.ini
- versions tree
- new schema changes
- additive, numbered files
- 001_harden_tenant_rls_policies.sql
- down-migrations
- postgres image
- schema
- /docker-entrypoint-initdb.d/
- fresh data volume
- ./dev-c360.sh reset
- LEO CDP environment version drift
- PG16
- Redis8
- PG15
- Redis7
source: leo-customer360 release-doc work, session 2026-08-26
status: seedling
tags:
- leo-cdp
- postgres
- migrations
- gotcha
title: LEO CDP schema migrations are ordered plain SQL, not dbmate or alembic
type: lesson
---

# LEO CDP schema migrations are ordered plain SQL, not dbmate or alembic

LEO Customer 360 manages its PostgreSQL schema as **ordered plain SQL files**, applied in a fixed sequence by `deployments/postgres/run-sql.sh <uat|prod>` (over an SSH bastion, each with `ON_ERROR_STOP=1`): `deployments/postgres/**/*.sql` → `database-init/{database-schema, init-core-database, data-view-for-llm}.sql` → everything under `database-init/migrations/` alphabetically.

Gotcha: **`alembic` is a declared dependency in customer360-api but is NOT the migration mechanism** — there is no `alembic.ini` or versions tree, so `alembic upgrade` does nothing useful. A `dbmate` adoption was also tried and then **reverted**. Don't reach for either tool.

Rules that follow: new schema changes must be **additive, numbered files** under `database-init/migrations/` (e.g. `001_harden_tenant_rls_policies.sql`); never edit an already-applied file. Migrations are **forward-only** — there are no down-migrations, so plan changes backward-compatibly or recover from backup.

Locally the `postgres` image bakes schema into `/docker-entrypoint-initdb.d/`, which only runs on a fresh data volume (`./dev-c360.sh reset` to re-init).

## Related

- [[LEO CDP environment version drift local PG16Redis8 vs managed PG15Redis7|LEO CDP environment version drift: local PG16/Redis8 vs managed PG15/Redis7]]

%% ai-graph-start %%

**Related notes:**
- [[LEO CDP schema changes must go in both database-schema.sql and a migrations file]]
- [[leo-customer360 uses dbmate for Postgres migrations, not Alembic]]
- [[Never pipe a dbmate migration file through a raw psql replay — its down section is destructive]]
- [[leo-customer360 applies DB schema via two paths that must stay in sync]]
- [[LEO CDP environment version drift local PG16Redis8 vs managed PG15Redis7]]

**Relations:**
- LEO CDP schema migrations — *USES* — ordered plain SQL files
- LEO CDP schema migrations — *DOES_NOT_USE* — dbmate
- LEO CDP schema migrations — *DOES_NOT_USE* — alembic
- LEO Customer 360 — *MANAGES* — PostgreSQL schema
- PostgreSQL schema — *MANAGED_BY* — ordered plain SQL files
- ordered plain SQL files — *APPLIED_BY* — deployments/postgres/run-sql.sh
- deployments/postgres/run-sql.sh — *APPLIES_SQL_FROM* — deployments/postgres/**/*.sql
- deployments/postgres/run-sql.sh — *APPLIES_SQL_FROM* — database-init/{database-schema, init-core-database, data-view-for-llm}.sql
- deployments/postgres/run-sql.sh — *APPLIES_SQL_FROM* — database-init/migrations/
- alembic — *IS_DECLARED_DEPENDENCY_IN* — customer360-api
- alembic — *IS_NOT_MIGRATION_MECHANISM_FOR* — LEO CDP
- alembic — *LACKS* — alembic.ini
- alembic — *LACKS* — versions tree
- dbmate — *WAS_TRIED_AND_REVERTED_FOR* — LEO CDP schema migrations
- new schema changes — *ARE* — additive, numbered files
- additive, numbered files — *LOCATED_UNDER* — database-init/migrations/
- 001_harden_tenant_rls_policies.sql — *IS_EXAMPLE_OF* — additive, numbered files
- LEO CDP schema migrations — *ARE* — forward-only
- LEO CDP schema migrations — *HAVE_NO* — down-migrations
- postgres image — *BAKES* — schema
- schema — *BAKED_INTO* — /docker-entrypoint-initdb.d/
- /docker-entrypoint-initdb.d/ — *RUNS_ON* — fresh data volume
- ./dev-c360.sh reset — *RESETS* — fresh data volume
- LEO CDP — *HAS_RELATED_TOPIC* — LEO CDP environment version drift
- LEO CDP environment version drift — *INVOLVES* — PG16
- LEO CDP environment version drift — *INVOLVES* — Redis8
- LEO CDP environment version drift — *INVOLVES* — PG15
- LEO CDP environment version drift — *INVOLVES* — Redis7

%% ai-graph-end %%