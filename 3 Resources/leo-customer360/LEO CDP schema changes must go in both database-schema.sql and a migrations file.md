---
ai_hash: 70350303ed3257b6
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-09
entities: []
source: SCRUM-93 session 2026-09-09
status: seedling
tags:
- leo-customer360
- postgres
- migrations
- gotcha
title: LEO CDP schema changes must go in both database-schema.sql and a migrations
  file
type: lesson
---

# LEO CDP schema changes must go in both database-schema.sql and a migrations file

In leo-customer360, the Postgres schema is applied by **two different paths**, so any schema change must be written in **both** places or one environment silently misses it:

- **Fresh databases** (local/dev via docker-compose) only run the files copied into `/docker-entrypoint-initdb.d/` by `postgres/Dockerfile` — namely `database-init/database-schema.sql` and `database-init/init-core-database.sql`. These run once, on an empty data dir. The `database-init/migrations/` folder is **not** applied here.
- **Existing deployed databases** (UAT/prod) are bootstrapped by `deployments/postgres/run-sql.sh`, which re-runs the base files *and* every `database-init/migrations/*.sql` in filename order. Because the base uses `CREATE TABLE IF NOT EXISTS`, re-running it never ALTERs an existing table — so new columns on existing tables reach deployed DBs **only** through a migration file.

Practical rule for a schema task: (1) add the new tables/columns to `database-schema.sql` (for fresh DBs), and (2) add an idempotent `migrations/NNN_*.sql` (for existing DBs). Make both idempotent (`ADD COLUMN IF NOT EXISTS`, guard `ADD CONSTRAINT` with a `pg_constraint` existence check inside a `DO $$` block, `CREATE INDEX IF NOT EXISTS`, `DROP POLICY IF EXISTS` before `CREATE POLICY`) since `run-sql.sh` re-applies everything on each deploy.

Rollback convention I introduced: pair `migrations/NNN_name.sql` with `migrations/NNN_name.down.sql`, and teach `run-sql.sh` to skip `! -iname "*.down.sql"` so rollbacks never auto-apply on deploy.

First hit: SCRUM-93 / SUBTASK-01 (agentic email-marketing schema foundation).

## Related

- [[Run a throwaway PostgreSQL on Windows via the pgserver pip package]]

%% ai-graph-start %%

**Related notes:**
- [[leo-customer360 applies DB schema via two paths that must stay in sync]]
- [[LEO CDP schema migrations are ordered plain SQL, not dbmate or alembic]]
- [[Never pipe a dbmate migration file through a raw psql replay — its down section is destructive]]
- [[Postgres docker-entrypoint-initdb.d runs only once on an empty volume]]
- [[CREATE TABLE IF NOT EXISTS cannot express a rename]]

%% ai-graph-end %%