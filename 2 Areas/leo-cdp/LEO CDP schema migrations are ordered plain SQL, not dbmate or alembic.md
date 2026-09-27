---
ai_hash: ee9af177cf9b47f8
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-26
entities:
- LEO CDP schema migrations
- ordered plain SQL
- dbmate
- alembic
- PostgreSQL schema
- deployments/postgres/run-sql.sh
- deployments/postgres/**/*.sql
- database-init/{database-schema, init-core-database, data-view-for-llm}.sql
- database-init/migrations/
- customer360-api
- alembic.ini
- versions tree
- alembic upgrade
- additive, numbered files
- forward-only migrations
- down-migrations
- postgres image
- /docker-entrypoint-initdb.d/
- fresh data volume
- ./dev-c360.sh reset
- 'LEO CDP environment version drift: local PG16/Redis8 vs managed PG15/Redis7'
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
- LEO CDP schema migrations — *are* — ordered plain SQL
- LEO CDP schema migrations — *are not* — dbmate
- LEO CDP schema migrations — *are not* — alembic
- LEO CDP schema migrations — *manage* — PostgreSQL schema
- ordered plain SQL — *applied by* — deployments/postgres/run-sql.sh
- deployments/postgres/run-sql.sh — *applies* — deployments/postgres/**/*.sql
- deployments/postgres/run-sql.sh — *applies* — database-init/{database-schema, init-core-database, data-view-for-llm}.sql
- deployments/postgres/run-sql.sh — *applies* — database-init/migrations/
- alembic — *is a declared dependency in* — customer360-api
- alembic — *is not the migration mechanism for* — LEO CDP schema migrations
- alembic — *lacks* — alembic.ini
- alembic — *lacks* — versions tree
- alembic upgrade — *does not work for* — LEO CDP schema migrations
- dbmate — *adoption was tried and reverted for* — LEO CDP schema migrations
- new schema changes — *must be* — additive, numbered files
- additive, numbered files — *are located under* — database-init/migrations/
- LEO CDP schema migrations — *are* — forward-only migrations
- forward-only migrations — *have no* — down-migrations
- postgres image — *bakes schema into* — /docker-entrypoint-initdb.d/
- /docker-entrypoint-initdb.d/ — *runs on* — fresh data volume
- ./dev-c360.sh reset — *re-initializes* — fresh data volume
- LEO CDP schema migrations — *is related to* — LEO CDP environment version drift: local PG16/Redis8 vs managed PG15/Redis7

%% ai-graph-end %%