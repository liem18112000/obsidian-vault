---
title: "Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)"
created: 2026-09-22
type: howto
status: seedling
source: "session 2026-09-22"
tags: [test-agent, docker-compose, local-dev, minio, pgvector, ollama, pubsub, laya]
---

# Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)

The `test-agent-v2/docker-compose.yaml` full-parity stack runs every prod dependency as a local container, no GCP. Each dep maps to a swappable port + a backend env switch:

- **Object store** -> MinIO (S3): `STORE_BACKEND=s3`, `S3_ENDPOINT_URL/S3_BUCKET/S3_ACCESS_KEY/S3_SECRET_KEY`. New `common/store/s3.py`. See [[S3 generation-CAS via botocore If-Match conditional writes]].
- **Task/session store + hybrid recall** -> Postgres `pgvector/pgvector:pg16`: `TASK_DB_URL=postgresql+asyncpg://...`, `MEMORY_BACKEND=hybrid`, `VECTOR_BACKEND=pgvector`. The pgvector schema self-creates (`CREATE EXTENSION IF NOT EXISTS vector`).
- **Embeddings** (for recall, no Vertex) -> Ollama: `EMBED_BACKEND=ollama`, `OLLAMA_URL`, `MEMORY_EMBED_MODEL=nomic-embed-text` (768-dim). New `common/embed/ollama.py`, selected in `pg/embed.py`.
- **Cache** -> Redis: `CACHE_BACKEND=redis`, `REDIS_HOST/PORT`.
- **Queue** -> Pub/Sub emulator: `TPD_GEN_MODE=workers`, `PUBSUB_EMULATOR_HOST`, `+worker` service. Needed refactoring `workers.py` off hardcoded GCS onto the ObjectStore port. See [[Route google-cloud clients to local emulators via _EMULATOR_HOST]].
- **LLM** -> Ollama via the pluggable `litellm` provider: `TESTAGENT_MODEL_BACKEND=litellm`, `LITELLM_MODEL=ollama/llama3.2`, `LITELLM_API_BASE`. See [[Pluggable LLM via the litellm ModelProvider backend]].
- **Decisions (JEV)** -> laya sidecar: `TPD_DECISION_BACKEND=laya`, `LAYA_URL`. See [[laya is JEVs local in-process decision-engine twin]].

Design decisions: user chose MinIO (S3 API) over a GCS emulator, and full-parity ON. laya isolated as a sidecar (CPU-only torch via `--index-url .../whl/cpu`) so torch is in ONE image. One robustness fix: the TPD assured decision gate now catches a decision-backend outage and falls back to the LLM judge. Startup is slow (first run pulls ~2GB Ollama models + builds torch). Extends [[Run test-agent-v2 locally with docker-compose (no GCP)]]. UNCOMMITTED; end-to-end boot verification pending.

## Related

- [[Run test-agent-v2 locally with docker-compose (no GCP)]]
- [[Pluggable LLM via the litellm ModelProvider backend]]
- [[laya is JEV's local in-process decision-engine twin]]
- [[S3 generation-CAS via botocore If-Match conditional writes]]
- [[Route google-cloud clients to local emulators via _EMULATOR_HOST]]
