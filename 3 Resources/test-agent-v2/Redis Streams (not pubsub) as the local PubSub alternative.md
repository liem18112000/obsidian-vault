---
ai_hash: 21775ae040463d83
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities: []
source: session 2026-09-22
status: seedling
tags:
- redis
- redis-streams
- pubsub
- queue
- test-agent
- local-dev
title: Redis Streams (not pub/sub) as the local Pub/Sub alternative
type: lesson
---

# Redis Streams (not pub/sub) as the local Pub/Sub alternative

To replace GCP Pub/Sub locally (avoiding the 3.5GB gcloud emulator image), use **Redis Streams**, NOT raw Redis pub/sub. Redis `PUBLISH`/`SUBSCRIBE` is fire-and-forget: no persistence, no ack/redelivery, no HTTP push — a subscriber offline at publish time loses the message, so scenario batches would silently drop. Redis **Streams** (`XADD` + a consumer group via `XREADGROUP`/`XACK`, with a pending-entries list for redelivery) give the durability + ack semantics Pub/Sub provided.

In test-agent-v2 (commit pending): new `common/queue.py` (`redis_publish` / `redis_consume`), selected by `TPD_QUEUE_BACKEND=redis|pubsub`. The prod Pub/Sub consume side was an HTTP PUSH (`worker.py` Starlette); the Redis side needs a **consumer loop** instead (`redis_worker.py`, run as `python redis_worker.py`) — a different process shape, since Streams are pull-based. Coordinator `_publish` dispatches on the backend; the object-store result path is unchanged. CEILING: no automatic XAUTOCLAIM reclaim of a crashed consumers pending batches (the coordinators per-batch synchronous fallback covers a lost batch anyway). See [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]].

## Related

- [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]]

%% ai-graph-start %%

**Related notes:**
- [[Route google-cloud clients to local emulators via _EMULATOR_HOST]]
- [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]]
- [[Redis Streams consumer-group at-least-once Loader pattern]]
- [[Run an event-broker Redis as a dedicated co-located instance, not the cache Redis]]
- [[Broker Redis needs opposite config from cache Redis, run it separately]]

%% ai-graph-end %%