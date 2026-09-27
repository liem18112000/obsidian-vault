---
ai_hash: 127a813698a4709d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities:
- test-agent-v2
- local-parity stack
- docker-compose.yaml
- prod dependency
- local container
- GCP
- Object store
- MinIO
- S3
- common/store/s3.py
- S3 generation-CAS via botocore If-Match conditional writes
- Task/session store
- hybrid recall
- Postgres
- pgvector
- pgvector schema
- Embeddings
- recall
- Vertex
- Ollama
- nomic-embed-text
- common/embed/ollama.py
- pg/embed.py
- Cache
- Redis
- Queue
- Pub/Sub emulator
- worker service
- workers.py
- GCS
- ObjectStore port
- google-cloud clients
- _EMULATOR_HOST
- LLM
- litellm provider
- llama3.2
- Pluggable LLM via the litellm ModelProvider backend
- Decisions
- JEV
- laya
- sidecar
- laya is JEV's local in-process decision-engine twin
- GCS emulator
- torch
- TPD assured decision gate
- decision-backend outage
- LLM judge
- Ollama models
- Run test-agent-v2 locally with docker-compose (no GCP)
source: session 2026-09-22
status: seedling
tags:
- test-agent
- docker-compose
- local-dev
- minio
- pgvector
- ollama
- pubsub
- laya
title: Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)
type: howto
---

# Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)

The `test-agent-v2/docker-compose.yaml` full-parity stack runs every prod dependency as a local container, no GCP. Each dep maps to a swappable port + a backend env switch:

- **Object store** -> MinIO (S3): `STORE_BACKEND=s3`, `S3_ENDPOINT_URL/S3_BUCKET/S3_ACCESS_KEY/S3_SECRET_KEY`. New `common/store/s3.py`. See [[S3 generation-CAS via botocore If-Match conditional writes]].
- **Task/session store + hybrid recall** -> Postgres `pgvector/pgvector:pg16`: `TASK_DB_URL=postgresql+asyncpg://...`, `MEMORY_BACKEND=hybrid`, `VECTOR_BACKEND=pgvector`. The pgvector schema self-creates (`CREATE EXTENSION IF NOT EXISTS vector`).
- **Embeddings** (for recall, no Vertex) -> Ollama: `EMBED_BACKEND=ollama`, `OLLAMA_URL`, `MEMORY_EMBED_MODEL=nomic-embed-text` (768-dim). New `common/embed/ollama.py`, selected in `pg/embed.py`.
- **Cache** -> Redis: `CACHE_BACKEND=redis`, `REDIS_HOST/PORT`.
- **Queue** -> Pub/Sub emulator: `TPD_GEN_MODE=workers`, `PUBSUB_EMULATOR_HOST`, `+worker` service. Needed refactoring `workers.py` off hardcoded GCS onto the ObjectStore port. See [[Route google-cloud clients to local emulators via _EMULATOR_HOST]].
- **LLM** -> Ollama via the pluggable `litellm` provider: `TESTAGENT_MODEL_BACKEND=litellm`, `LITELLM_MODEL=ollama/llama3.2`, `LITELLM_API_BASE`. See [[Pluggable LLM via the litellm ModelProvider backend]].
- **Decisions (JEV)** -> laya sidecar: `TPD_DECISION_BACKEND=laya`, `LAYA_URL`. See [[laya is JEV's local in-process decision-engine twin|laya is JEVs local in-process decision-engine twin]].

Design decisions: user chose MinIO (S3 API) over a GCS emulator, and full-parity ON. laya isolated as a sidecar (CPU-only torch via `--index-url .../whl/cpu`) so torch is in ONE image. One robustness fix: the TPD assured decision gate now catches a decision-backend outage and falls back to the LLM judge. Startup is slow (first run pulls ~2GB Ollama models + builds torch). Extends [[Run test-agent-v2 locally with docker-compose (no GCP)]]. UNCOMMITTED; end-to-end boot verification pending.

## Related

- [[Run test-agent-v2 locally with docker-compose (no GCP)]]
- [[Pluggable LLM via the litellm ModelProvider backend]]
- [[laya is JEV's local in-process decision-engine twin]]
- [[S3 generation-CAS via botocore If-Match conditional writes]]
- [[Route google-cloud clients to local emulators via _EMULATOR_HOST]]

%% ai-graph-start %%

**Related notes:**
- [[Low-disk CPU box drop laya, use JEV cloud API for decisions]]
- [[test-agent-v2 ran fully local end-to-end (LUZ-158390) — pipeline + quality gates proven]]
- [[One Postgres backs tasks, sessions, prompts AND pgvector in test-agent-v2]]
- [[Run test-agent-v2 locally with docker-compose (no GCP)]]
- [[Best fully-offline CPU config for test-agent-v2 (qwen2.53b + Turbo)]]

**Relations:**
- test-agent-v2 — *uses* — local-parity stack
- local-parity stack — *is defined by* — docker-compose.yaml
- local-parity stack — *runs* — prod dependency
- prod dependency — *runs as* — local container
- local-parity stack — *replaces* — GCP
- Object store — *implemented by* — MinIO
- MinIO — *provides* — S3
- common/store/s3.py — *supports* — MinIO
- S3 generation-CAS via botocore If-Match conditional writes — *relates to* — S3
- Task/session store — *implemented by* — Postgres
- hybrid recall — *uses* — Postgres
- Postgres — *uses* — pgvector
- pgvector schema — *managed by* — pgvector
- Embeddings — *provided by* — Ollama
- Embeddings — *for* — recall
- Embeddings — *does not use* — Vertex
- Ollama — *provides model* — nomic-embed-text
- common/embed/ollama.py — *supports* — Ollama
- common/embed/ollama.py — *selected in* — pg/embed.py
- Cache — *implemented by* — Redis
- Queue — *implemented by* — Pub/Sub emulator
- Pub/Sub emulator — *uses* — worker service
- workers.py — *refactored from* — GCS
- workers.py — *uses* — ObjectStore port
- Route google-cloud clients to local emulators via _EMULATOR_HOST — *relates to* — Pub/Sub emulator
- LLM — *provided by* — Ollama
- LLM — *accessed via* — litellm provider
- litellm provider — *uses model* — llama3.2
- Pluggable LLM via the litellm ModelProvider backend — *describes* — litellm provider
- Decisions — *handled by* — laya
- Decisions — *is also known as* — JEV
- laya — *is a type of* — sidecar
- laya — *is twin of* — JEV
- laya is JEV's local in-process decision-engine twin — *describes* — laya
- MinIO — *chosen over* — GCS emulator
- laya — *uses* — torch
- torch — *is in* — ONE image
- TPD assured decision gate — *catches* — decision-backend outage
- TPD assured decision gate — *falls back to* — LLM judge
- Ollama — *pulls* — Ollama models
- local-parity stack — *extends* — Run test-agent-v2 locally with docker-compose (no GCP)

%% ai-graph-end %%