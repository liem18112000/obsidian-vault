---
title: "MinIO Docker images live on quay.io not Docker Hub"
created: 2026-09-22
type: lesson
status: seedling
source: "session 2026-09-22"
tags: [minio, docker, gotcha, registry]
---

# MinIO Docker images live on quay.io not Docker Hub

MinIO **deprecated its Docker Hub images**. Pulling `minio/mc` (or `minio/minio`) from Docker Hub now fails with `pull access denied ... repository does not exist or may require docker login`. Use the **quay.io** images instead: `quay.io/minio/minio` (server) and `quay.io/minio/mc` (client). Bit me in the first `docker compose up` of the full-parity stack — the server image had already been switched to quay.io but the `mc` init container still pointed at Docker Hub. See [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]].

## Related

- [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]]
