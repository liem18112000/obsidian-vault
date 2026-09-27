---
ai_hash: 092ac4d77a46c888
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Background Process Optimization for luz-docs API (LUZ)'
status: seedling
tags:
- architecture
- background-jobs
- scheduling
- anti-pattern
- jax-rs
- confluence-distilled
title: Piggybacking background jobs on HTTP requests couples job load to traffic
type: lesson
---

# Piggybacking background jobs on HTTP requests couples job load to traffic

Triggering background jobs from an HTTP request filter — intercept every request, fire async events, let an in-memory cache decide whether the job is "due" — looks like a free scheduler. It is not. It couples maintenance work to user traffic in three ways that all fail under exactly the conditions you care about.

The shape, as found in `luz-docs`: a JAX-RS `ContainerRequestFilter` inspects each request for a `tenantId`, fires several CDI `fireAsync` events, and job classes throttle themselves against a time cache ("once a day per tenant", "every 5 minutes").

**Why it breaks:**

1. **Job load peaks exactly when user load peaks.** Work is scheduled by request arrival, so the heaviest batch deletions and DB aggregations land during your busiest hour — never at 3am when the system is idle. Database batch deletes lock tables and drain the connection pool at the worst possible time.
2. **Request threads end up waiting on job threads.** The enrichment job looped over documents blocking on `Future.get()` with a 20-minute timeout. A request thread parked for 20 minutes is thread exhaustion, HTTP queuing, and CPU/memory contention — caused by a request that had nothing to do with enrichment.
3. **The throttle is per-instance, so it multiplies by replica count.** See [[In-memory job throttles silently break when you scale to multiple replicas]].

**The fix is to pick the right off-ramp per job, not one mechanism for all of them:**

| Job shape | Move it to |
|---|---|
| Periodic compliance / cleanup (daily, tenant-wide) | Kubernetes `CronJob` |
| Per-item work that must not block a caller | Event-driven queue — Pub/Sub, Cloud Tasks |
| Retry of previously-failed items | Dead-letter queue consumed by a worker |

> [!tip] The diagnostic question
> Ask: *if traffic dropped to zero for six hours, should this work still happen?* If yes, it is scheduled work and does not belong anywhere near a request filter. If it genuinely must happen as a consequence of **this** request, it belongs in the request — done synchronously, or enqueued and acknowledged.

The general principle: **an HTTP request is a poor scheduler.** It fires at the wrong frequency (traffic-shaped, not time-shaped), in the wrong process (one sized for latency, not throughput), with the wrong failure semantics (a job failure surfaces as a user-facing error, or gets swallowed). Schedulers and queues exist because those three properties are hard to retrofit.

Source: [[Background Process Optimization for luz-docs API Architecture and Recommendations]] (LUZ, Confluence).

## Related

- [[In-memory job throttles silently break when you scale to multiple replicas]]

%% ai-graph-start %%

**Related notes:**
- [[In-memory job throttles silently break when you scale to multiple replicas]]
- [[Background Process Optimization for luz-docs API Architecture and Recommendations]]
- [[Read-side fire-and-forget mutation pass the id and re-read in the async, don't mutate the object being serialized]]
- [[If upstream holds memory until you ack, your write latency is their OOM risk]]
- [[New architecture for documentStatistic]]

%% ai-graph-end %%