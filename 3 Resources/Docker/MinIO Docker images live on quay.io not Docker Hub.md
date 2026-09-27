---
ai_hash: 681a037d160aa78d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities: []
source: session 2026-09-22
status: seedling
tags:
- minio
- docker
- gotcha
- registry
title: MinIO Docker images live on quay.io not Docker Hub
type: lesson
---

# MinIO Docker images live on quay.io not Docker Hub

MinIO **deprecated its Docker Hub images**. Pulling `minio/mc` (or `minio/minio`) from Docker Hub now fails with `pull access denied ... repository does not exist or may require docker login`. Use the **quay.io** images instead: `quay.io/minio/minio` (server) and `quay.io/minio/mc` (client). Bit me in the first `docker compose up` of the full-parity stack — the server image had already been switched to quay.io but the `mc` init container still pointed at Docker Hub. See [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]].

## Related

- [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]]

%% ai-graph-start %%

**Related notes:**
- [[A failed docker compose --build leaves latest on the OLD image (silent stale run)]]
- [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]]
- [[Route google-cloud clients to local emulators via _EMULATOR_HOST]]
- [[Separate docker-compose files are isolated networks; use one file + a profile for optional services]]
- [[Bitnami 2025 catalog reorg removed pinned bitnami version tags]]

%% ai-graph-end %%