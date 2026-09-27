---
title: "test-agent-v2 Cloud SQL Postgres holds app + ADK-session + A2A-task tables on one engine"
created: 2026-09-18
type: reference
status: seedling
source: "session 2026-09-18 building persistence-db-eer"
tags: [test-agent-v2, cloud-sql, postgres, pgvector, adk, a2a, schema]
---

# test-agent-v2 Cloud SQL Postgres holds app + ADK-session + A2A-task tables on one engine

The test-agent-v2 persistence database is a single Cloud SQL Postgres instance reached through one shared async SQLAlchemy engine (`common.db.get_engine`, pgvector extension enabled). It holds three groups of tables, and both `build_session_service()` and `build_task_store()` dial the *same* engine/pool.

**App-owned (we write the DDL):**
- `memory_node` — pgvector recall tier. `id` PK; `embedding vector(768)`; `tsv` is a GENERATED tsvector column; HNSW index on embedding + GIN on tsv; `meta jsonb`; carries `run_id`, `context_id`, `scope`, `status`, `confidence`.
- `memory_edge` — directed memory graph. PK `(source_id, target)`. Both columns reference `memory_node.id` but are plain `text` — an **app-level** reference, no DB foreign key.
- `prompt_template` — PK `key`; `current_version` points at the live version.
- `prompt_version` — PK `(key, version)`; append-only audit log; `key` FK -> `prompt_template`.

**ADK `DatabaseSessionService` (auto-created, we do not own the DDL):** `sessions` (PK `app_name,user_id,id`), `events` (PK `id,app_name,user_id,session_id`; FK -> `sessions`), `app_states` (PK `app_name`), `user_states` (PK `app_name,user_id`), `adk_internal_metadata` (PK `key`, schema-version marker).

**A2A `DatabaseTaskStore` (auto-created):** `tasks` — PK `id text(36)`; `context_id`; `status`/`artifacts`/`history`/`metadata` as `json`.

`context_id` is the app-level thread tying A2A tasks <-> ADK sessions <-> memory nodes into one pipeline context (no DB FK across the groups).

**Not in this DB:** code graphs and harvested lessons persist in **GCS** (object store), not Cloud SQL.

EER diagram: `test-agent-v2/docs/persistence-db-eer.excalidraw` (+ `.png`).

## Related

- [[test-agent-v2]]
