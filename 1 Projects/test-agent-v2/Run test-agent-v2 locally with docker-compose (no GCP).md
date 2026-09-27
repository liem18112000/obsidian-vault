---
ai_hash: ae3aadad28378ad3
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities:
- test-agent-v2
- docker-compose
- GCP
- .env.compose.example
- .env.compose
- model
- Atlassian creds
- MCP gateway
- :8080
- Claude Code
- GATEWAY_BEARER_TOKEN
- kga
- tpd
- tev
- admin
- STORE_BACKEND=local
- memory bank
- docker volume
- /data/memory
- pipeline state
- ObjectStore
- TESTAGENT_MODEL_BACKEND=litellm
- litellm
- ModelProvider backend
- Task state
- session state
- A2A state
- Cloud SQL
- GCS_BUCKET
- /readyz probe
- test-agent-v2/docker-compose.yaml
- test-agent-v2/.env.compose.example
- GcsArtifactService
- storage.Client
- Pluggable LLM
- Shared state
source: session 2026-09-22
status: seedling
tags:
- test-agent
- docker-compose
- local-dev
- adk
title: Run test-agent-v2 locally with docker-compose (no GCP)
type: howto
---

# Run test-agent-v2 locally with docker-compose (no GCP)

Copy \`.env.compose.example\` to \`.env.compose\`, set the model + Atlassian creds, then \`docker compose up --build\` from \`test-agent-v2/\`. It boots 5 containers — the MCP **gateway** (published on :8080, the endpoint Claude Code connects to with `GATEWAY_BEARER_TOKEN`) plus the four A2A agents **kga / tpd / tev / admin** (each `uvicorn main:app` selected by `$AGENT`). No GCP is required.

Two things make the no-GCP local run work end to end:
- **Shared state**: `STORE_BACKEND=local` puts the memory bank on a docker volume mounted at `/data/memory` in all four agents, so the pipeline state passes between containers. See [[ObjectStore is test-agent cross-service shared state]].
- **Pluggable model**: `TESTAGENT_MODEL_BACKEND=litellm` — see [[Pluggable LLM via the litellm ModelProvider backend]].

Task/session/A2A state is in-memory per container (fine for a single-instance local run; no Cloud SQL). `GCS_BUCKET` is set to a dummy value only to satisfy the `/readyz` probe — it is never dialed under `STORE_BACKEND=local`. Files: `test-agent-v2/docker-compose.yaml`, `test-agent-v2/.env.compose.example`.

## Related

- [[Pluggable LLM via the litellm ModelProvider backend]]
- [[ObjectStore is test-agent cross-service shared state]]
- [[GcsArtifactService dials storage.Client eagerly in its constructor]]

%% ai-graph-start %%

**Related notes:**
- [[ObjectStore is test-agent cross-service shared state]]
- [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]]
- [[Pluggable LLM via the litellm ModelProvider backend]]
- [[test-agent-v2 cloud resource and credential map (klara-nonprod)]]
- [[test-agent-v2 ran fully local end-to-end (LUZ-158390) — pipeline + quality gates proven]]

**Relations:**
- test-agent-v2 — *runs locally with* — docker-compose
- test-agent-v2 — *does not require* — GCP
- .env.compose.example — *is copied to* — .env.compose
- .env.compose — *configures* — model
- .env.compose — *configures* — Atlassian creds
- docker-compose — *starts* — MCP gateway
- docker-compose — *starts* — kga
- docker-compose — *starts* — tpd
- docker-compose — *starts* — tev
- docker-compose — *starts* — admin
- MCP gateway — *published on port* — :8080
- Claude Code — *connects to* — MCP gateway
- Claude Code — *uses* — GATEWAY_BEARER_TOKEN
- kga — *is an* — A2A agent
- tpd — *is an* — A2A agent
- tev — *is an* — A2A agent
- admin — *is an* — A2A agent
- STORE_BACKEND=local — *enables* — Shared state
- Shared state — *uses* — memory bank
- memory bank — *is stored on* — docker volume
- docker volume — *mounted at* — /data/memory
- pipeline state — *passes between* — containers
- ObjectStore — *is a type of* — Shared state
- ObjectStore — *is* — test-agent cross-service shared state
- STORE_BACKEND=local — *uses* — ObjectStore
- TESTAGENT_MODEL_BACKEND=litellm — *enables* — Pluggable LLM
- litellm — *is a* — ModelProvider backend
- Pluggable LLM — *is provided by* — litellm
- Task state — *is* — in-memory
- session state — *is* — in-memory
- A2A state — *is* — in-memory
- test-agent-v2 — *does not use* — Cloud SQL
- GCS_BUCKET — *satisfies* — /readyz probe
- /readyz probe — *is not dialed under* — STORE_BACKEND=local
- test-agent-v2/docker-compose.yaml — *is a* — file
- test-agent-v2/.env.compose.example — *is a* — file
- GcsArtifactService — *dials* — storage.Client
- storage.Client — *is dialed by* — GcsArtifactService
- test-agent-v2 — *uses* — litellm

%% ai-graph-end %%