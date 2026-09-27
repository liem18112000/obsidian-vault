---
title: "Cloud Run concurrency capacity is an upper bound CPU rarely lets you reach"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: Performance Test - 100% Thumbnail Cloud Run (2026-04-13)"
tags: [cloud-run, capacity-planning, performance, autoscaling, gcp, load-testing]
---

# Cloud Run concurrency capacity is an upper bound CPU rarely lets you reach

The obvious capacity formula for a Cloud Run service is

```
maxScale × containerConcurrency ÷ processing_time
```

For the thumbnail service that gave `120 × 8 ÷ 0.33 s ≈ 2,900 RPS`. The **measured practical ceiling was ~417 RPS** — seven times lower.

The gap is **CPU**. Long before the concurrency slots fill, the instances pin at 100% CPU, processing time inflates, and the formula's denominator — which you assumed constant — grows. Concurrency capacity is an upper bound that the CPU budget almost never lets you reach.

The useful reading of the two numbers together:

- At **240 RPS** the service hit `maxScale=120` but stayed healthy: `240 × 0.43 s ≈ 103` in-flight vs `960` concurrency slots, CPU settling to 66–73%. Safe — but only because the cap happened to be sufficient.
- At **480 RPS** it saturated: maxScale pinned *and* CPU at 100% for the whole run.

So capacity planning needs **both** levers checked. Raising `containerConcurrency` when CPU is the constraint just puts more requests on the same starved instance and makes latency worse. The fix at the wall is to raise `maxScale` **and/or** CPU per instance — concurrency is not the knob.

Rule: **compute the theoretical ceiling to know what to test, then measure to find the real one, and treat the ratio between them as your CPU headroom.**

## Related

- [[Hitting Cloud Run maxScale turns latency into compounding errors, and 2xx throughput falls]]
- [[Cold starts appear as a p95 spike during the scale-up ramp, not in steady state]]

## Related

- [[Hitting Cloud Run maxScale turns latency into compounding errors, and 2xx throughput falls]]
