---
ai_hash: 5a625d7a810d0c88
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities: []
source: session 2026-09-22
status: seedling
tags:
- docker
- docker-compose
- build
- stale-image
- gotcha
title: A failed docker compose --build leaves :latest on the OLD image (silent stale
  run)
type: lesson
---

# A failed docker compose --build leaves :latest on the OLD image (silent stale run)

GOTCHA that cost real time: after editing source, several `docker compose up --build` runs FAILED (at image pulls / disk / pip DNS) — and because a failed build never re-tags, the service `:latest` images stayed pinned to the FIRST successful build. The stack then came up RUNNING STALE CODE (missing new modules `common.store.s3`, `common.queue`, `redis_worker.py`, and the `boto3` dep). It looked healthy because `/livez` always returns ok — health probes do NOT prove the running code matches your source. DETECT: `docker exec <ctr> python -c "import <new.module>"` and `ls /app/*.py` / check image CreatedSince. FIX: rebuild until it SUCCEEDS (`docker compose build`), then `up -d` to recreate containers off the new images. Corollary: a container crash-looping on `cant open file /app/<new>.py` means the image predates a Dockerfile `COPY <new>.py`. See [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]].

## Related

- [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]]

%% ai-graph-start %%

**Related notes:**
- [[Thin overlay image to refresh app code when full rebuilds keep failing]]
- [[Verify a deployed service isn't stale match box RepoDigest to GHCR latest and read its sha-commit tag]]
- [[Repeated compose up -d can corrupt the bridge network — down+up to rebuild it]]
- [[Cloud Run won't redeploy on a latest digest change — apply by immutable digest]]
- [[test-agent-v2 image built only from pyproject + src + main.py]]

%% ai-graph-end %%