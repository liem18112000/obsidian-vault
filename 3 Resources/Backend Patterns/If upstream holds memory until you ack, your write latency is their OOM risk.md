---
ai_hash: 8425e7966c29c5de
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: luz_docs Document Reliable Delivery POC (LUZ)'
status: seedling
tags:
- backpressure
- async
- retry
- pubsub
- distributed-systems
- reliability
- confluence-distilled
title: If upstream holds memory until you ack, your write latency is their OOM risk
type: lesson
---

# If upstream holds memory until you ack, your write latency is their OOM risk

When an upstream service buffers data in memory and can only release it once you return `200`, **your write latency becomes their memory pressure**. Slow storage downstream shows up as an out-of-memory risk upstream — in a service that looks, from its own metrics, like it is doing nothing wrong.

The concrete case: `luz_eletter` accepts a batch of up to 100K documents into temporary memory, then moves them to `luz_docs`. It cannot free that memory until `luz_docs` confirms the write. Slow storing in `luz_docs` therefore stalls memory release in `luz_eletter` — the bottleneck is in one service, the symptom in another.

**The resolution is to split the pipeline by what actually needs a synchronous guarantee**, rather than making the whole chain synchronous or the whole chain async:

| Phase | Guarantee needed | Why |
|---|---|---|
| **1. Sender accepts the batch** → stored in sender DB | **Synchronous, must return 200** | Upstream is holding the bytes in memory and cannot release until confirmed. This is the precondition for the whole flow. |
| **2. Fan out** sender DB → per-recipient DB | **Asynchronous, retried** | Nobody is blocked waiting. One file per thread, max 30 threads, retry every 5 min, max 3 attempts. Caller does not need the status. |

Phase 1 buys the upstream its memory back as fast as possible. Phase 2 gets durability from retries instead of from a blocked caller.

**The question that separates the phases** is not "is this important?" — both phases are — but **"is anyone holding a resource while they wait?"** If yes, that step must be fast and acknowledged. If no, make it async and let retries provide the guarantee.

> [!tip] Design the failure path first
> The acceptance criterion was *100K documents sent, every one delivered*. At that volume a bounded retry (3 attempts, 5-minute spacing) will leave a residue of permanent failures, so the design needs an answer for what happens to attempt 4 — a dead-letter queue or a failure record the sender can query. "Retry on failure" is only a complete design once you have said what happens when retries run out.

> [!warning] Async does not mean fire-and-forget
> Moving phase 2 to a queue only helps if the enqueue itself is durable. If the handoff is an in-process thread pool rather than a persisted queue, a pod restart loses the in-flight work and the guarantee is weaker than the synchronous version you replaced.

Related: [[Piggybacking background jobs on HTTP requests couples job load to traffic]] — the mirror image, where work that *should* be async is attached to the request path.

Source: [[luz_docs Improvement - Document Reliable Delivery Proof Of Concept]] (LUZ, Confluence).

## Related

- [[Piggybacking background jobs on HTTP requests couples job load to traffic]]

%% ai-graph-start %%

**Related notes:**
- [[luz_docs Improvement - Document Reliable Delivery Proof Of Concept]]
- [[Bottleneck analysis large-recipient deliveries & cross-sender impact]]
- [[Timeouts stack in series and the shortest wins; audit the whole chain]]
- [[Score async API designs on crash recovery and multi-instance, not latency]]
- [[Invoice Run, ePost backend storage]]

%% ai-graph-end %%