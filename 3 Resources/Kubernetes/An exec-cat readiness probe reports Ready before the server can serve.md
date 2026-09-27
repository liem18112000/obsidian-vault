---
ai_hash: 5a8eb1431d92ae24
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Service Error Analysis Report - FAILED_TO_STORE on Production
  (2025-12-18)'
status: seedling
tags:
- kubernetes
- readiness-probe
- reliability
- deployment
- luz-jsonstore
- gotcha
title: An exec-cat readiness probe reports Ready before the server can serve
type: lesson
---

# An exec-cat readiness probe reports Ready before the server can serve

A `readinessProbe` that runs `exec: cat /some/file` tests that a **file exists**, not that the service can answer a request. The file is typically written early in startup — before the HTTP connector binds, before the connection pool warms, before the app can serve anything. Kubernetes marks the pod Ready, the Service adds it to its endpoints, traffic arrives, and the client gets **connection refused**.

This was the mechanism behind 25 production `FAILED_TO_STORE` errors in `luz-jsonstore` ([[Every production FAILED_TO_STORE traced back to a rolling deploy, not to load]]). The fix was to point the probe at an endpoint that only answers once the application is genuinely serving:

```yaml
readinessProbe:
  httpGet:                                   # was: exec cat <file>
    path: /luz_jsonstore/api/health/ready
    port: 8080
  initialDelaySeconds: 30
  periodSeconds: 5
  failureThreshold: 6
```

The general rule: **a readiness probe must exercise the same path the traffic will take.** A file check, a TCP check, or a probe on a trivially-static endpoint all answer a weaker question than "can you serve?" — and the gap between the weak answer and the real one is exactly the window in which requests get dropped.

Related failure in the same incident: [[A rolling deploy drops in-flight requests unless preStop outlives endpoint propagation]].

## Related

- [[Every production FAILED_TO_STORE traced back to a rolling deploy, not to load]]
- [[A rolling deploy drops in-flight requests unless preStop outlives endpoint propagation]]

## Related

- [[Every production FAILED_TO_STORE traced back to a rolling deploy, not to load]]
- [[A rolling deploy drops in-flight requests unless preStop outlives endpoint propagation]]

%% ai-graph-start %%

**Related notes:**
- [[A rolling deploy drops in-flight requests unless preStop outlives endpoint propagation]]
- [[Every production FAILED_TO_STORE traced back to a rolling deploy, not to load]]
- [[Service Reliability Solution]]
- [[Health checks should probe dependencies and split critical vs fail-open]]
- [[CoreDNS livenessreadiness 404 means probe path-port mismatch, not a CoreDNS crash]]

%% ai-graph-end %%