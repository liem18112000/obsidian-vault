---
ai_hash: 3778fb7ef5cb43ec
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Performance Test - 100% Thumbnail Cloud Run (2026-04-13)'
status: seedling
tags:
- cloud-run
- cold-start
- latency
- performance
- load-testing
- gcp
title: Cold starts appear as a p95 spike during the scale-up ramp, not in steady state
type: lesson
---

# Cold starts appear as a p95 spike during the scale-up ramp, not in steady state

Cloud Run cold starts measured **~10.5 s per new instance** for the thumbnail service. But the number that showed up in the metrics was different: **p95 spiked to 5.3 s in the first minute** of the test — while the autoscaler ramped 39 → 120 instances — then settled to **~430 ms**.

Two consequences worth internalising:

- **Cold-start cost is a function of the ramp, not of the service.** It appears when instance count is *changing*, and disappears once the fleet is warm. A p95 that is terrible for sixty seconds and fine afterwards is a scaling artifact, not a latency regression — and averaging over the whole run hides both facts.
- **Per-instance cold start ≠ observed p95.** Only the requests unlucky enough to land on a booting instance pay the full 10.5 s; the rest are served by warm ones. The observed percentile is diluted by the warm fleet, which is why the p95 spike (5.3 s) is *lower* than the actual cold start.

Practical reading: **always look at latency percentiles alongside the instance-count graph.** Without the instance count, the spike looks like an unexplained outlier. With it, the story is obvious — and the fix is a warm floor (`minScale`) or a faster boot, not query tuning.

This is also why a load test should report the steady-state window separately from the ramp. Mixing them produces a p95 that describes neither.

## Related

- [[Cloud Run concurrency capacity is an upper bound CPU rarely lets you reach]]
- [[Hitting Cloud Run maxScale turns latency into compounding errors, and 2xx throughput falls]]

## Related

- [[Cloud Run concurrency capacity is an upper bound CPU rarely lets you reach]]

%% ai-graph-start %%

**Related notes:**
- [[Cloud Run concurrency capacity is an upper bound CPU rarely lets you reach]]
- [[Hitting Cloud Run maxScale turns latency into compounding errors, and 2xx throughput falls]]
- [[Performance Test - 100% Thumbnail Cloud Run]]
- [[A latency penalty in the tail not the median points to JITGC warm-up under CPU contention]]
- [[Trust median and p90 over many runs, not the mean of a few, for latency on a noisy shared env]]

%% ai-graph-end %%