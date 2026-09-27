---
title: "Jaeger on a small box: use in-memory bounded storage, not badger, to avoid OOM 502"
created: 2026-09-16
type: lesson
status: seedling
source: "session 2026-09-16 leo-customer360 jaeger 502"
tags: [jaeger, tracing, oom, docker, leo-customer360]
---

# Jaeger on a small box: use in-memory bounded storage, not badger, to avoid OOM 502

Jaeger `all-in-one` with badger on-disk storage (`SPAN_STORAGE_TYPE=badger`) **memory-maps its on-disk DB**. As the DB grows, opening it balloons virtual memory (observed: 5.5 GB `total-vm` for a ~1.2 GB DB with 245 tables) and the resident open-time working set exceeds a small `--memory` cap. On a tight box that means a cgroup-OOM crash-loop: badger reopens the DB on each boot, gets reaped before the query server comes up, so the oauth2-proxy / Caddy in front returns **502**. It also silently fills the disk (each stale volume kept growing).

On a small shared box (1 vCPU / 2 GB, ~300 MB spare), the durable fix is **in-memory bounded storage**:

```bash
docker run -d --name c360-jaeger --memory 300m \
  -e COLLECTOR_OTLP_ENABLED=true -e QUERY_BASE_PATH=/jaeger \
  -e SPAN_STORAGE_TYPE=memory \
  jaegertracing/all-in-one:1.62.0 --memory.max-traces=20000
```

This gives a **deterministic RAM bound** (no mmap of a growing DB) and **zero disk growth**. Trade-off: traces are lost on restart — the right ceiling for on-demand request tracing, wrong if traces must persist (then use badger/ES on a dedicated box).

**Why not just wipe or TTL the badger volume:** it only resets the clock. Here badger OOM-looped twice (grew to 3.8 GB, then 1.2 GB after a fresh volume + 24h TTL) — the memory blow-up recurs as soon as the DB regrows.

Diagnosed via [[Docker can report OOMKilled=false on a cgroup memcg OOM — check dmesg]] (docker inspect showed exit 0 / OOMKilled=false; dmesg proved memcg-OOM). Context: leo-customer360, `deployments/monitoring/deploy-monitoring.sh`, UAT box beta.leocdp.com.

## Related

- [[Docker can report OOMKilled=false on a cgroup memcg OOM — check dmesg]]
