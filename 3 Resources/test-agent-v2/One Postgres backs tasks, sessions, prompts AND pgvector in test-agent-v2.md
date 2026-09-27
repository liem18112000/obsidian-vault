---
ai_hash: 3b4dbb6fe9333384
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities: []
source: session 2026-09-22
status: seedling
tags:
- test-agent
- postgres
- pgvector
- prompt-store
- task-store
title: One Postgres backs tasks, sessions, prompts AND pgvector in test-agent-v2
type: lesson
---

# One Postgres backs tasks, sessions, prompts AND pgvector in test-agent-v2

In test-agent-v2 a SINGLE Postgres (the `db` container / `TASK_DB_URL`) is both the "normal" relational DB and the vector DB — everything reuses one `common.db.get_engine()` async engine/pool in the one `testagent` database:

- A2A **tasks** -> `DatabaseTaskStore` (common/adk/services.py)
- ADK **sessions** -> `DatabaseSessionService` (same)
- **prompts** (`prompt_*` MCP tools) -> `PgPromptStore` (common/prompts/stores.py): `PgPromptStore(py) if get_engine() is not None else py`
- **vector recall** -> memory_node/memory_edge (common/memory/pg)

All tables self-create lazily (`CREATE TABLE IF NOT EXISTS ...`; pgvector also `CREATE EXTENSION IF NOT EXISTS vector`) — no manual migration on a fresh DB. GOTCHA: the switch is simply *presence of TASK_DB_URL* (get_engine() not None). Unset it and the task store + prompt store silently fall back to in-memory — so a missing TASK_DB_URL loses task/prompt persistence, not just vectors. See [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]].

## Related

- [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]]

%% ai-graph-start %%

**Related notes:**
- [[test-agent-v2 Cloud SQL Postgres holds app + ADK-session + A2A-task tables on one engine]]
- [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]]
- [[A2A to_a2a task_store and runner are separate persistence params]]
- [[ObjectStore is test-agent cross-service shared state]]
- [[a2a-sdk DatabaseTaskStore makes A2A tasks survive Cloud Run restarts]]

%% ai-graph-end %%