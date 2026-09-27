---
title: "A failed docker compose --build leaves :latest on the OLD image (silent stale run)"
created: 2026-09-22
type: lesson
status: seedling
source: "session 2026-09-22"
tags: [docker, docker-compose, build, stale-image, gotcha]
---

# A failed docker compose --build leaves :latest on the OLD image (silent stale run)

GOTCHA that cost real time: after editing source, several `docker compose up --build` runs FAILED (at image pulls / disk / pip DNS) — and because a failed build never re-tags, the service `:latest` images stayed pinned to the FIRST successful build. The stack then came up RUNNING STALE CODE (missing new modules `common.store.s3`, `common.queue`, `redis_worker.py`, and the `boto3` dep). It looked healthy because `/livez` always returns ok — health probes do NOT prove the running code matches your source. DETECT: `docker exec <ctr> python -c "import <new.module>"` and `ls /app/*.py` / check image CreatedSince. FIX: rebuild until it SUCCEEDS (`docker compose build`), then `up -d` to recreate containers off the new images. Corollary: a container crash-looping on `cant open file /app/<new>.py` means the image predates a Dockerfile `COPY <new>.py`. See [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]].

## Related

- [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]]
