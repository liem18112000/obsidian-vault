---
ai_hash: e728840a1a136be9
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-09
entities: []
source: PROD investigation 2026-09-09
status: seedling
tags:
- coredns
- dns
- root-cause
- availability
- luz
- replica
title: Luz DNS storm gate is both CoreDNS replicas down together plus morning load,
  not enrichment volume
type: observation
---

# Luz DNS storm gate is both CoreDNS replicas down together plus morning load, not enrichment volume

Follow-up that CORRECTS the earlier enrichment dose-response for the Luz PROD DNS incident. A full week of data breaks the "enrichment volume drives the storm" story: Sep 2 had the HIGHEST enrichment re-runs (12,152) and ZERO UnknownHostException, while Sep 1 (5,246) and Sep 8 (12,004) stormed. Enrichment does not correlate.

The real gate is a TOTAL DNS resolution gap: coredns-custom runs 2 replicas, and because the liveness/readiness probe 404s constantly, BOTH replicas are on a perpetual kill/restart cycle (~225 kills/day total). Normally the two are out of phase (one serves while the other restarts), so resolution keeps working. A storm happens only when BOTH are not-ready at the same moment:
- Sep 8: both killed 06:32:30 (kwtzv) + 06:34:21 (jsfvv) -> restarts overlap -> UnknownHost peaks 06:36-06:41 (08:36-08:41 CEST).
- Sep 1: both restarting 06:02-06:05 -> peak 06:04.

Two conditions must coincide:
1. A total gap (both replicas down/not-ready together) - intermittent alignment of their restart cycles.
2. Morning DNS LOAD hitting that gap - luz-jsonstore doing heavy fresh mongod-hostname resolution during the business ramp + enrichment (08:00-09:00 CEST). At night/weekend the same gap resolves silently. That is WHY it is a workday-morning phenomenon: time-of-day = when load is present, not when CoreDNS is worse (CoreDNS is uniformly broken 24/7).

Both big events were exactly one week apart (Tuesdays) - a possible weekly re-alignment trigger (rollout / node op), UNCONFIRMED. Confirming the exact gaps needs CoreDNS pod-readiness / Service-endpoint history (was the DNS Service at 0 ready endpoints?), which is RBAC-gated.

Lesson: a clean-looking dose-response from 3-4 cherry-picked days can be a coincidence; validate against the FULL period before claiming causation.

## Related
[[CoreDNS livenessreadiness 404 means probe path-port mismatch, not a CoreDNS crash]]
[[MongoTimeoutException from UnknownHostException is a DNS fault, not DB overload]]
[[Intermittent DB saturation = stacked loads crossing a fixed ceiling]]

## Related

- [[CoreDNS livenessreadiness 404 means probe path-port mismatch]]
- [[not a CoreDNS crash]]
- [[MongoTimeoutException from UnknownHostException is a DNS fault]]
- [[not DB overload]]

%% ai-graph-start %%

**Related notes:**
- [[luz-docs enrichment re-run campaign saturates PROD MongoDBs each morning]]
- [[MongoTimeoutException from UnknownHostException is a DNS fault, not DB overload]]
- [[Luz eLetter dispatch and enrichment re-run both funnel through luz-jsonstore into Mongo]]
- [[CoreDNS livenessreadiness 404 means probe path-port mismatch, not a CoreDNS crash]]
- [[Intermittent DB saturation = stacked loads crossing a fixed ceiling]]

%% ai-graph-end %%