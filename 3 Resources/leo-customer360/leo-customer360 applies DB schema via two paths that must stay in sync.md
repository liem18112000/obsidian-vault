---
ai_hash: 1f47b487e420f4be
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-30
entities: []
source: session 2026-08-30 leo-customer360
status: seedling
tags:
- leo-customer360
- postgres
- schema
- docker
- deployment
title: leo-customer360 applies DB schema via two paths that must stay in sync
type: howto
---

# leo-customer360 applies DB schema via two paths that must stay in sync

In `leo-customer360`, database schema SQL is applied through **two independent paths that must be kept in sync** — adding or renaming a schema file means editing both.

## The two paths
1. **Image init** — `postgres/Dockerfile` does `COPY database-init/<file>.sql /docker-entrypoint-initdb.d/NN-<file>.sql`. Used by local `docker-compose` and the CI-built `postgres` image. Postgres runs these **only once, on an empty data dir** (first container start).
2. **Managed vDB** — `deployments/postgres/run-sql.sh <uat|prod>` pipes `database-init/*.sql` over SSH (via a bastion) into the managed VNG vDB. It has an explicit ordered list `APP_ORDER=(database-schema init-core-database persona360-schema data-view-for-llm)`, then any other `*.sql` alphabetically, then `migrations/*.sql`.

## Consequence
A new schema file (e.g. `persona360-schema.sql`) must be wired into **both**: a `COPY` line in the Dockerfile *and* `APP_ORDER` in `run-sql.sh`. Path 2 catches unlisted files via an alphabetical glob, but relying on that gives incidental, undocumented ordering — list it explicitly.

## Why re-running is safe
Both paths can re-apply the same SQL, so the schema files are written **idempotently**: `CREATE ... IF NOT EXISTS`, `CREATE OR REPLACE FUNCTION/VIEW`, triggers as `DROP TRIGGER IF EXISTS` + `CREATE TRIGGER`, and RLS policies dropped-then-created inside a function. `persona360` is also independent of the core `customer360` schema (no cross-references), so its apply order is flexible.

## Related
[[CI path-filter must mirror the Docker build context, not the service folder]]

## Related

- [[CI path-filter must mirror the Docker build context]]
- [[not the service folder]]

%% ai-graph-start %%

**Related notes:**
- [[LEO CDP schema changes must go in both database-schema.sql and a migrations file]]
- [[LEO CDP schema migrations are ordered plain SQL, not dbmate or alembic]]
- [[CI path-filter must mirror the Docker build context, not the service folder]]
- [[Never pipe a dbmate migration file through a raw psql replay — its down section is destructive]]
- [[Postgres docker-entrypoint-initdb.d runs only once on an empty volume]]

%% ai-graph-end %%