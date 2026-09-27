---
ai_hash: 1e1b841270d75815
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Performance Test - Thumbnail Cloud Run / GKE (2026-04-13)'
status: seedling
tags:
- cloud-run
- autoscaling
- overload
- performance
- monitoring
- gcp
title: Hitting Cloud Run maxScale turns latency into compounding errors, and 2xx throughput
  falls
type: lesson
---

# Hitting Cloud Run maxScale turns latency into compounding errors, and 2xx throughput falls

When a Cloud Run service reaches `autoscaling.knative.dev/maxScale`, the autoscaler has nothing left to give. What follows is not a plateau — it is a **collapse that accelerates**:

- 5xx grew from **320/min to 3,573/min** across the test window as in-flight requests piled up.
- Meanwhile **2xx throughput *fell*** — 26.9K → 16.3K per minute.

That second line is the diagnostic one. **Successful throughput going *down* while load goes *up* means the service is spending cycles on requests that will fail anyway** — queueing them, timing them out, tearing them down. Past the knee, extra load is negative-yield.

Two things to take from it:

- **`maxScale` is a hard cliff, not a soft limit.** Below it, extra load costs latency; at it, extra load costs successful requests. Alert on *approaching* the cap, not on errors — by the time 5xx appears you are already on the wrong side.
- **Track 2xx throughput, not just error rate.** An error-rate graph shows a service degrading; a falling success-throughput graph shows a service in congestive collapse, and they need different responses.

A related diagnostic from the same runs: k6 reported **5.17% failures** with no matching **5xx in the Cloud Run logs**. That mismatch means the failures were **client-side** — k6 timeouts and connection resets, or upstream overload — and never reached the service to be logged. *Load-generator failures with no server-side errors point upstream of the service, not at it.*

## Related

- [[Cloud Run concurrency capacity is an upper bound CPU rarely lets you reach]]
- [[Cold starts appear as a p95 spike during the scale-up ramp, not in steady state]]

## Related

- [[Cloud Run concurrency capacity is an upper bound CPU rarely lets you reach]]

%% ai-graph-start %%

**Related notes:**
- [[Cloud Run concurrency capacity is an upper bound CPU rarely lets you reach]]
- [[Cold starts appear as a p95 spike during the scale-up ramp, not in steady state]]
- [[Performance Test - 100% Thumbnail Cloud Run]]
- [[A disabled Cloud Run service 503s at the edge and never reaches your app]]
- [[Right-sizing k8s resource limits on the Customer360 stack]]

%% ai-graph-end %%