---
ai_hash: b839216b22d744f7
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Background Process Optimization for luz-docs API (LUZ)'
status: seedling
tags:
- distributed-systems
- scaling
- kubernetes
- throttling
- race-condition
- confluence-distilled
title: In-memory job throttles silently break when you scale to multiple replicas
type: gotcha
---

# In-memory job throttles silently break when you scale to multiple replicas

A job that throttles itself against an **in-process** cache — "only run this once per day per tenant", held in a `HashMap` or a local time cache — is correct on one instance and silently wrong on every instance after that.

Each replica keeps its own copy of the cache. Nothing is shared, so the guarantee degrades from *once per day* to *once per day, per pod*:

| Replicas | Actual runs per day per tenant |
|--:|--:|
| 1 | 1 |
| 3 | 3 |
| 10 (autoscaled peak) | 10 |

The failure is nasty because it is **invisible in dev and proportional to success**. You develop against one instance and it behaves exactly as designed. It only misbehaves in production, and it gets worse precisely when traffic grows enough to scale you out — so the symptom (duplicated deletions, N× the DB load, repeated notifications) arrives at the same moment as peak load and looks like a capacity problem rather than a correctness bug.

Worse, a rolling deploy resets every cache to empty, so a deployment can trigger a full round of "due" jobs across all tenants at once.

**Fixes, cheapest first:**

- **Move the schedule out of the app.** A Kubernetes `CronJob` runs once regardless of replica count. This is the right answer for anything periodic and is usually less code than the throttle it replaces.
- **Share the throttle state.** Put the last-run timestamp in Redis or the database and make the check a conditional write (`SET key NX EX <ttl>`, or an `UPDATE … WHERE last_run < now() - interval` that returns a row count). The winner runs; the losers skip.
- **Leader election**, if the work must stay in-process — only the elected instance runs the job.

> [!warning] A read-then-write check is still a race
> `if (cache.get(tenant) is stale) { run(); cache.put(tenant, now); }` is not fixed merely by moving the cache to Redis. Two pods can both read "stale" before either writes. The check and the claim must be **one atomic operation** — `SET NX`, a conditional update, or a lock — or you have replaced a per-pod bug with a rarer, harder-to-reproduce one.

Related: [[Piggybacking background jobs on HTTP requests couples job load to traffic]].

Source: [[Background Process Optimization for luz-docs API Architecture and Recommendations]] (LUZ, Confluence).

## Related

- [[Piggybacking background jobs on HTTP requests couples job load to traffic]]

%% ai-graph-start %%

**Related notes:**
- [[Piggybacking background jobs on HTTP requests couples job load to traffic]]
- [[Cache-epoch invalidation fails if the epoch is read through a local L1]]
- [[Per-pod single-flight kills cache stampede without semantic change]]
- [[Two-tier cache must propagate caller TTL to every tier]]
- [[Raising negative-cache TTL turns transient failures into long-lived poison]]

%% ai-graph-end %%