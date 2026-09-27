---
title: "Run test-agent-v2 locally with docker-compose (no GCP)"
created: 2026-09-22
type: howto
status: seedling
source: "session 2026-09-22"
tags: [test-agent, docker-compose, local-dev, adk]
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
