---
ai_hash: c355e86488627a75
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-03
entities: []
source: session 2026-08-03
status: seedling
tags:
- kubernetes
- resources
- limits
- jvm
- heap
- oomkilled
- startupprobe
- dagster
title: Right-sizing k8s resource limits on the Customer360 stack
type: lesson
---

# Right-sizing k8s resource limits on the Customer360 stack

Lessons from putting minimal CPU/memory limits on a mixed JVM+Python k8s stack (Customer360 local kind):

- **JVM containers need an explicit heap cap when you set a memory limit.** The JVM sizes its default heap to a percentage of the *node's* RAM, not the cgroup limit, so it blows past a small container limit and gets OOMKilled. Cap it: Bitnami Kafka `KAFKA_HEAP_OPTS=-Xmx512m -Xms256m`; Keycloak `JAVA_OPTS_KC_HEAP=-Xms256m -Xmx512m`.
- **Keycloak `start-dev` re-runs Quarkus augmentation (~60-90s) on every boot**, and a CPU limit makes it slower — the liveness probe (initialDelaySeconds 60) then SIGTERM-kills it (exit 143) into a crash loop. Fix with a `startupProbe` on `/health/started` (port 9000) with a generous `failureThreshold`; liveness/readiness only start after it passes.
- **A CPU *limit* throttles startup, not just steady state.** A multi-process app (dagster dev = 7 gRPC code-location subprocesses) is very slow to become Ready under 500m. Raising the CPU *limit* speeds startup at no idle cost (idle apps don't use it). But more CPU → more concurrent loading → higher peak memory, so tune CPU and memory together.
- **Right-size the outlier instead of forcing it minimal.** dagster genuinely peaks ~2Gi loading 7 locations; it OOMKilled at 768Mi and 1.5Gi before 2.5Gi held. "Minimal for all resources" means right-sized, not uniformly tiny.
- Verify OOM vs other kills via `kubectl get pod X -o jsonpath='{.status.containerStatuses[0].lastState.terminated.reason}'` (OOMKilled exit 137 vs SIGTERM 143).

## Related
- [[StatefulSet volumeClaimTemplates are immutable; PVCs cannot shrink]]
- [[Customer360 Kubernetes deployment (local kind + GreenNode VKS)]]

%% ai-graph-start %%

**Related notes:**
- [[Uncapped Dagster on a swapless 2GB box OOMs the whole host (SSH banner-timeout); cap container memory + add swap]]
- [[Customer360 Kubernetes deployment (local kind + GreenNode VKS)]]
- [[Docker can report OOMKilled=false on a cgroup memcg OOM — check dmesg]]
- [[Deploying Keycloak 26 as a container health port 9000, bootstrap admin, start vs start-dev]]
- [[kind image pulls stall on Docker Hub rate limits; pre-pull and kind load]]

%% ai-graph-end %%