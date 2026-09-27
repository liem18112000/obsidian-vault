---
ai_hash: b8af7cb50c0161a9
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities: []
source: session 2026-09-22
status: seedling
tags:
- test-agent
- object-store
- gotcha
- architecture
title: ObjectStore is test-agent cross-service shared state
type: lesson
---

# ObjectStore is test-agent cross-service shared state

In the test-agent-v2 stack the four agent containers (kga/tpd/tev/admin) are separate processes; the **only** state they share across the gather -> refine -> define -> implement pipeline is the **memory bank**, reached through the `ObjectStore` port (GCS in prod). 

Consequence: an *in-memory-per-container* store (`STORE_BACKEND=memory`) silently breaks the pipeline — KGA writes the pack but TPD, a different container, never sees it. A local multi-service run therefore needs a **shared** store. That is why `LocalFsObjectStore` (`STORE_BACKEND=local`, `common/store/local.py`) exists: a shared-filesystem blob store on a docker volume, with CAS via a generation counter guarded by a portable `O_EXCL` spin-lock (correct on Windows + Linux). Contrast: task/session/A2A state is per-container and does *not* need sharing for a single-instance run. See [[Run test-agent-v2 locally with docker-compose (no GCP)]].

## Related

- [[Run test-agent-v2 locally with docker-compose (no GCP)]]

%% ai-graph-start %%

**Related notes:**
- [[Run test-agent-v2 locally with docker-compose (no GCP)]]
- [[One Postgres backs tasks, sessions, prompts AND pgvector in test-agent-v2]]
- [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]]
- [[test-agent-v2 Redis cache port + Memorystore needs a VPC connector]]
- [[test-agent-v2 cloud resource and credential map (klara-nonprod)]]

%% ai-graph-end %%