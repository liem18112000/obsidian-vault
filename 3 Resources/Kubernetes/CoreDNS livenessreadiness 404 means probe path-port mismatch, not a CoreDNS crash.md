---
ai_hash: f873ca31765815ff
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-09
entities: []
source: PROD investigation 2026-09-09
status: seedling
tags:
- coredns
- dns
- kubernetes
- liveness-probe
- gotcha
- luz
title: CoreDNS liveness/readiness 404 means probe path-port mismatch, not a CoreDNS
  crash
type: lesson
---

# CoreDNS liveness/readiness 404 means probe path-port mismatch, not a CoreDNS crash

When a CoreDNS pod is crash-looping because its liveness/readiness probe fails with **HTTP 404** (not "connection refused", not a timeout), the DNS server itself is almost certainly fine — a 404 means an HTTP server IS listening and answering but the requested PATH is not served. The fix is in the probe/Corefile, not CoreDNS.

CoreDNS health endpoints (must match the probe exactly):
- `health` plugin serves **`/health` on :8080** (liveness).
- `ready` plugin serves **`/ready` on :8181** (readiness).
- Any other path on those ports -> 404. Wrong port with no server -> "connection refused" (different symptom).

Common real causes of the 404:
- Probe uses `/healthz` (Kubernetes convention) instead of CoreDNS`s **`/health`**.
- Readiness probe points at the health port (:8080) or wrong path.
- The `ready` (or `health`) plugin is missing from the Corefile while the Deployment still probes it.

Confirming that CoreDNS is healthy (so it IS a mismatch): the CoreDNS container logs are clean — Corefile loads with a stable config SHA, `plugin/health: Going into lameduck mode for 5s` appears on each shutdown (proves the health plugin is loaded), and there are no ERROR/bind lines. It boots, serves `.:53`, then gets SIGTERM`d by the failing probe and restarts.

Diagnose: `kubectl -n kube-system get deploy <coredns> -o yaml` and read the `livenessProbe`/`readinessProbe` httpGet path+port; compare against the Corefile`s `health`/`ready` directives. (Reading these needs container.deployments.get / configMaps.get — a plain logging.viewer + limited RBAC account cannot, so this often has to be handed to a cluster-admin.)

Seen in Luz PROD: a self-managed `coredns-custom` (CoreDNS 1.12.4) crash-looped ~225x/day on liveness/readiness 404s — the chronic DNS fragility behind [[MongoTimeoutException from UnknownHostException is a DNS fault, not DB overload]].

## Related
[[MongoTimeoutException from UnknownHostException is a DNS fault, not DB overload]]

## Related

- [[MongoTimeoutException from UnknownHostException is a DNS fault]]
- [[not DB overload]]

%% ai-graph-start %%

**Related notes:**
- [[MongoTimeoutException from UnknownHostException is a DNS fault, not DB overload]]
- [[Luz DNS storm gate is both CoreDNS replicas down together plus morning load, not enrichment volume]]
- [[luz-docs enrichment re-run campaign saturates PROD MongoDBs each morning]]
- [[Liveness-probe death spiral killing a thread-pool-saturated pod turns overload into a self-perpetuating outage]]
- [[KC_HTTP_RELATIVE_PATH moves Keycloak health endpoints under the prefix too]]

%% ai-graph-end %%