---
ai_hash: 021be3e18aa4654b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities:
- test-agent-v2
- MinIO
- Postgres
- Redis
- Pub/Sub emulator
- Ollama
- laya
- docker-compose.yaml
- GCP
- Object store
- S3
- pgvector
- litellm
- Embeddings
- Cache
- Queue
- LLM
- Decisions (JEV)
- torch
- LLM judge
- TPD assured decision gate
- nomic-embed-text
- llama3.2
- Run test-agent-v2 locally with docker-compose (no GCP)
- Pluggable LLM via the litellm ModelProvider backend
- laya is JEVs local in-process decision-engine twin
- S3 generation-CAS via botocore If-Match conditional writes
- Route google-cloud clients to local emulators via _EMULATOR_HOST
- full-parity stack
- prod dependency
- local container
- GCS emulator
- decision-backend outage
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
- test-agent-v2 — *is a* — full-parity stack
- test-agent-v2 — *is configured by* — docker-compose.yaml
- test-agent-v2 — *does not use* — GCP
- test-agent-v2 — *uses* — MinIO
- test-agent-v2 — *uses* — Postgres
- test-agent-v2 — *uses* — Redis
- test-agent-v2 — *uses* — Pub/Sub emulator
- test-agent-v2 — *uses* — Ollama
- test-agent-v2 — *uses* — laya
- test-agent-v2 — *extends* — Run test-agent-v2 locally with docker-compose (no GCP)
- test-agent-v2 — *has related note* — Run test-agent-v2 locally with docker-compose (no GCP)
- full-parity stack — *runs* — prod dependency
- prod dependency — *as* — local container
- MinIO — *provides* — Object store
- MinIO — *implements* — S3
- MinIO — *is chosen over* — GCS emulator
- MinIO — *has related note* — S3 generation-CAS via botocore If-Match conditional writes
- Postgres — *provides* — Task/session store
- Postgres — *provides* — hybrid recall
- Postgres — *uses* — pgvector
- Redis — *provides* — Cache
- Pub/Sub emulator — *provides* — Queue
- Pub/Sub emulator — *has related note* — Route google-cloud clients to local emulators via _EMULATOR_HOST
- Ollama — *provides* — Embeddings
- Ollama — *provides* — LLM
- Ollama — *hosts* — nomic-embed-text
- Ollama — *hosts* — llama3.2
- Ollama — *is accessed via* — litellm
- litellm — *is a* — LLM provider
- litellm — *has related note* — Pluggable LLM via the litellm ModelProvider backend
- laya — *provides* — Decisions (JEV)
- laya — *is a* — sidecar
- laya — *uses* — torch
- laya — *has related note* — laya is JEVs local in-process decision-engine twin
- TPD assured decision gate — *catches* — decision-backend outage
- TPD assured decision gate — *falls back to* — LLM judge

%% ai-graph-end %%