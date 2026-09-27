---
title: "Redis Streams (not pub/sub) as the local Pub/Sub alternative"
created: 2026-09-22
type: lesson
status: seedling
source: "session 2026-09-22"
tags: [redis, redis-streams, pubsub, queue, test-agent, local-dev]
---

# Redis Streams (not pub/sub) as the local Pub/Sub alternative

To replace GCP Pub/Sub locally (avoiding the 3.5GB gcloud emulator image), use **Redis Streams**, NOT raw Redis pub/sub. Redis `PUBLISH`/`SUBSCRIBE` is fire-and-forget: no persistence, no ack/redelivery, no HTTP push — a subscriber offline at publish time loses the message, so scenario batches would silently drop. Redis **Streams** (`XADD` + a consumer group via `XREADGROUP`/`XACK`, with a pending-entries list for redelivery) give the durability + ack semantics Pub/Sub provided.

In test-agent-v2 (commit pending): new `common/queue.py` (`redis_publish` / `redis_consume`), selected by `TPD_QUEUE_BACKEND=redis|pubsub`. The prod Pub/Sub consume side was an HTTP PUSH (`worker.py` Starlette); the Redis side needs a **consumer loop** instead (`redis_worker.py`, run as `python redis_worker.py`) — a different process shape, since Streams are pull-based. Coordinator `_publish` dispatches on the backend; the object-store result path is unchanged. CEILING: no automatic XAUTOCLAIM reclaim of a crashed consumers pending batches (the coordinators per-batch synchronous fallback covers a lost batch anyway). See [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]].

## Related

- [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]]
