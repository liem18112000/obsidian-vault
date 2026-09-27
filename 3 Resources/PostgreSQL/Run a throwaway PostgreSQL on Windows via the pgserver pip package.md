---
ai_hash: 0ee2f020fbdf56d6
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-09
entities: []
source: SCRUM-93 session 2026-09-09
status: seedling
tags:
- postgres
- windows
- testing
- migrations
- pgserver
title: Run a throwaway PostgreSQL on Windows via the pgserver pip package
type: howto
---

# Run a throwaway PostgreSQL on Windows via the pgserver pip package

To verify a PostgreSQL migration end-to-end on a Windows box with **no Docker and no server install**, install the pip package **`pgserver`** (`pip install pgserver`). It bundles a complete PostgreSQL 16 (its own `initdb`, `postgres`, `psql`, and the `share/postgresql/postgres.bki` bootstrap files) under `site-packages/pgserver/pginstall/bin`.

Usage from Python:
```python
import pgserver
server = pgserver.get_server(pgdata_path, cleanup_mode="stop")  # initdb + start
uri = server.get_uri()   # postgresql://postgres:@127.0.0.1:<port>/postgres
server.cleanup()         # stop + (with cleanup_mode) tidy
```
Run SQL files with meta-commands (`\echo`, `\set ON_ERROR_STOP`, `DO` blocks) through the bundled psql: `psql.exe -X -w -d <uri> -v ON_ERROR_STOP=1 -f file.sql`. Always pass `-w` (never prompt) or a bad connection **hangs** on a password prompt instead of erroring; pass the connection as `-d <uri>`, not as a bare positional (a positional URI makes psql treat later `-v`/`-f` as ignored extra args).

Gotcha: the PostgreSQL 18 in `C:\Program Files\PostgreSQL\18` on this machine is a **client-tools-only** install — it has `bin\psql.exe` etc. but **no `share/` and no `postgres.bki`**, so its `initdb` cannot bootstrap a cluster (`file "...postgres.bki" does not exist`). Use pgserver instead. If a prior run was killed mid-test, a stray `postgres.exe` from `pginstall` can lock the data dir (`initdb: directory ... not empty`) — kill it (`Get-Process postgres | where Path -like *pginstall*`) and use a fresh data dir.

Caveat: pgserver PG16 has **no pgvector/postgis**, so the full `database-schema.sql` (needs `vector`/`postgis`) will not load as-is; test a migration against **minimal stub prerequisite tables** (just the PK + tenant_id + FK-target columns the migration references).

## Related

- [[LEO CDP schema changes must go in both database-schema.sql and a migrations file]]

%% ai-graph-start %%

**Related notes:**
- [[LEO CDP schema changes must go in both database-schema.sql and a migrations file]]
- [[Local Cloud SQL admin without psql use the Python connector + asyncpg]]
- [[Idempotent CREATE DATABASE needs psql gexec since it cannot run in a transaction]]
- [[Never pipe a dbmate migration file through a raw psql replay — its down section is destructive]]
- [[LEO CDP schema migrations are ordered plain SQL, not dbmate or alembic]]

%% ai-graph-end %%