---
title: "Timeouts stack in series and the shortest wins; audit the whole chain"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: Bottleneck analysis large-recipient deliveries (HACKA)"
tags: [timeouts, kubernetes, gateway, kong, debugging, load-testing, confluence-distilled]
---

# Timeouts stack in series and the shortest wins; audit the whole chain

Raising a timeout in application config does nothing if a proxy in front of it gives up sooner. Timeouts along a request path **stack in series, and the shortest one wins** — so the effective timeout is a property of the whole chain, not of the component you configured.

A real chain, measured during large-delivery load testing:

```
client → Kong proxy (3600s) → GKE Gateway backend (GCPBackendPolicy) → luz-eletter → luz-storage-batch (MP-REST readTimeout)
```

| Layer | Configured | |
|---|---|---|
| Kong proxy | 3600s | generous |
| **GKE Gateway** (`GCPBackendPolicy`, `infra-gateway-policy`) | **300s** | ← **the real limit** |
| App → storage-batch (`…_MP_REST_READTIMEOUT`) | raised 300 → **900s** | the one someone fixed |

The app-level read timeout was deliberately raised to 900s to accommodate long uploads. **The Gateway policy was never updated to match.** So a request the application would happily wait out is hard-killed at 300s by a layer nobody looked at.

The evidence is unusually clean: a 2,000-document test once produced a **317.8s response that succeeded** — before the Gateway policy existed. *The same response today would be killed at 300s.* The system got a new layer, and with it a new, lower ceiling that no application setting can raise.

**Why this is easy to miss:** each layer's config lives in a different repo, owned by a different team, in a different format — an app env var, a Kubernetes `GCPBackendPolicy` in an overlay, a proxy config. Nothing cross-checks them, and the symptom is a generic connection reset that looks like a network fault rather than a policy.

> [!tip] Write the chain down, with numbers, before tuning anything
> List every hop and its timeout, then find the minimum. That single table is the diagnosis. It also tells you the **order** to change things: raising an inner timeout above an outer one is wasted work, and *lowering* an outer one silently caps everything behind it.

> [!warning] Long synchronous work is the underlying problem
> This chain exists because creating a delivery does **one synchronous multipart upload of every document** — 2,000 docs took 89–272s (avg 168s), 3,000 took ~183s, and 8,000 took ~302s and caused an incident. Timeout tuning buys headroom; it does not remove a workload whose duration scales with input size. The real fix is to stop holding a request open for minutes — see [[Long exports acknowledge immediately, deliver by emailed link to object storage]].

Related: [[Server push choice is decided by proxy idle timeouts and pod affinity, not API elegance]] — the same hidden-ceiling problem for long-lived connections.

Source: [[Bottleneck analysis large-recipient deliveries & cross-sender impact]] (HACKA, Confluence).

## Related

- [[Long exports acknowledge immediately, deliver by emailed link to object storage]]
