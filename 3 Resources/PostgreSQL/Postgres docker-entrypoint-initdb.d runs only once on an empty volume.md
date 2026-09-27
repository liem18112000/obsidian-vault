---
ai_hash: fac3ccf87efdbceb
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-24
entities: []
source: session 2026-08-24 migration tooling research
status: seedling
tags:
- postgres
- docker
- gotcha
- migrations
title: Postgres docker-entrypoint-initdb.d runs only once on an empty volume
type: lesson
---

# Postgres docker-entrypoint-initdb.d runs only once on an empty volume

Scripts placed in a Postgres container's `/docker-entrypoint-initdb.d/` (`.sql`/`.sh`) run **exactly once — only when the data directory is empty**, i.e. on the first-ever container start against a fresh volume. They are **never** re-run on subsequent starts once the volume has data.

**The gotcha:** using this as your only schema bootstrap means any later change to `schema.sql` is **silently ignored on every existing database** (dev, UAT, prod all already booted). The change works on fresh volumes and 'mysteriously' does nothing on existing ones — no error, no warning. Symptom: teammates start hand-writing idempotent `DO $$` guard blocks to patch live DBs, which is re-inventing a migration runner by hand.

**Fix:** don't use `initdb` for evolving schema. Keep only truly-once bootstrap there (e.g. `CREATE EXTENSION`), and drive all schema changes through a real migration tool that tracks applied versions (e.g. [[leo-customer360 uses dbmate for Postgres migrations, not Alembic|dbmate]]).

Ref: Docker Hub `postgres` → 'Initialization scripts'.

## Related

- [[leo-customer360 uses dbmate for Postgres migrations, not Alembic]]

%% ai-graph-start %%

**Related notes:**
- [[LEO CDP schema changes must go in both database-schema.sql and a migrations file]]
- [[leo-customer360 uses dbmate for Postgres migrations, not Alembic]]
- [[Never pipe a dbmate migration file through a raw psql replay — its down section is destructive]]
- [[leo-customer360 applies DB schema via two paths that must stay in sync]]
- [[LEO CDP schema migrations are ordered plain SQL, not dbmate or alembic]]

%% ai-graph-end %%