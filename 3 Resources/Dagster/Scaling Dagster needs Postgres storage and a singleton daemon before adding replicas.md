---
ai_hash: 143054aa9f2c7504
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-03
entities: []
source: leo-customer360 dagster-scaling-analysis, 2026-09-03
status: seedling
tags:
- dagster
- kubernetes
- scaling
- gotcha
title: Scaling Dagster needs Postgres storage and a singleton daemon before adding
  replicas
type: lesson
---

# Scaling Dagster needs Postgres storage and a singleton daemon before adding replicas

A single-pod `dagster dev` (webserver + daemon + in-process code-location gRPC in one process, backed by SQLite on a ReadWriteOnce PVC) **cannot** be scaled by raising its replica count. SQLite has one writer, the volume is single-attach, and `dagster dev` is a dev-only launcher.

Production Dagster requires, **in this order**:

0. **Shared storage first.** Move run/event/schedule storage to **PostgreSQL** and compute logs to **S3/MinIO**. Until this is done, more than one replica means DB-lock errors and logs stranded on the "other" pod. This is the hard prerequisite for everything else.
1. **Split `dagster dev`** into a stateless `dagster-webserver` (N replicas, HPA-able, behind a Service) and a `dagster-daemon`.
2. **The daemon must be EXACTLY 1 replica** (`strategy: Recreate`, never HPA). Dagster has no leader election; two daemons run schedules/sensors twice → duplicate ticks and duplicate runs.

Only the webserver is stateless and horizontally scalable. Run `dagster instance migrate` after the storage switch and on every upgrade.

## Related
[[Dagster worker pools are executor queues, not a pod kind]]

## Related

- [[Dagster worker pools are executor queues, not a pod kind]]

%% ai-graph-start %%

**Related notes:**
- [[Dagster auto-creates its tables but not the database]]
- [[Dagster worker pools are executor queues, not a pod kind]]
- [[Split Dagster webserver and daemon must share fail-closed Postgres storage]]
- [[Dagster has no supported storage-backend migration; run history is operational metadata]]
- [[Dagster QueuedRunCoordinator needs a running daemon to drain the run queue]]

%% ai-graph-end %%