---
ai_hash: 9010bf8cf4b18a0c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-07-09
entities:
- luz_docs
- WildFly pods
- sketchcounter state
- pod-local state
- pom.xml
- Dockerfile
- Kubernetes deployment
- standalone.xml
- standalone-ha.xml
- standalone-full-ha.xml
- Infinispan
- Redis
- Kubernetes StatefulSet
- HPA
- replicas
- load balancer
- JVMs
- in-memory state
- mutable aggregate state
- counter
- HyperLogLog sketch
- in-memory cache
- CDI bean
- pod-local JVM memory
- silent correctness bug
- HLL
- _isPublic
- _effectiveSecurityClassCodes
- _shard
- document store
- MongoDB
- write-path/materialize pattern
- centrally-persisted value
- tenant-wide state
- MicroProfile
- cardinality-sketch utility
- luz_docs countN badge
- fuzzy-zone fallback
source: verified via pom.xml, Dockerfile, kubernetes HPA config, 2026-07-09
status: seedling
tags:
- luz-docs
- kepler
- wildfly
- architecture
- gotcha
title: luz_docs runs non-clustered WildFly pods, so pod-local sketch/counter state
  is broken
type: lesson
---

# luz_docs runs non-clustered WildFly pods, so pod-local sketch/counter state is broken

Verified directly from luz_docs' pom.xml, Dockerfile, and its Kubernetes deployment: the app runs plain standalone.xml (not standalone-ha.xml / standalone-full-ha.xml), has no Infinispan or Redis dependency anywhere in source, and is deployed as a Kubernetes StatefulSet with an HPA scaling 1->10 replicas. So it is already multiple independent, non-clustered WildFly JVMs behind a load balancer, with zero shared in-memory state between pods today.

This matters for any design that wants to keep mutable aggregate state (a counter, a HyperLogLog sketch, an in-memory cache of "current count") in a CDI bean or any other pod-local JVM memory: with HPA at >1 replica, a read hitting pod A only ever reflects writes that happened to route through pod A. That is not a probabilistic-accuracy problem (unlike HLL's inherent error) -- it is a silent correctness bug, because the "missing" data from other pods never gets folded in at all.

The fix this codebase already uses for equivalent problems (_isPublic, _effectiveSecurityClassCodes, _shard) is to materialize the derived state into the document store (MongoDB, via the existing write-path/materialize pattern) instead of JVM memory, so every pod reads the same centrally-persisted value regardless of which pod produced it. Any new per-tenant/per-dimension aggregate (counter, HLL sketch, cache) on this app should follow the same rule: never trust pod-local memory to represent tenant-wide state when the app can scale beyond 1 replica.

See [[MicroProfile and WildFly have no HyperLogLog or cardinality-sketch utility]] and [[luz_docs countN badge can use HyperLogLog with a fuzzy-zone fallback]].

## Related

- [[MicroProfile and WildFly have no HyperLogLog or cardinality-sketch utility]]
- [[luz_docs countN badge can use HyperLogLog with a fuzzy-zone fallback]]

%% ai-graph-start %%

**Related notes:**
- [[MicroProfile and WildFly have no HyperLogLog or cardinality-sketch utility]]
- [[luz_docs estimated-count POC drops CAS and backfill gate]]
- [[luz_docs countN badge can use HyperLogLog with a fuzzy-zone fallback]]
- [[luz_docs documentscount is scan-bound and cannot reach sub-second at 128k]]
- [[HPA replica scale-out cannot fix a serial wait that lives in another service]]

**Relations:**
- luz_docs — *runs* — WildFly pods
- WildFly pods — *are* — non-clustered
- pod-local sketchcounter state — *is broken by* — non-clustered WildFly pods
- luz_docs — *uses* — pom.xml
- luz_docs — *uses* — Dockerfile
- luz_docs — *uses* — Kubernetes deployment
- luz_docs — *runs* — standalone.xml
- luz_docs — *does not run* — standalone-ha.xml
- luz_docs — *does not run* — standalone-full-ha.xml
- luz_docs — *has no dependency on* — Infinispan
- luz_docs — *has no dependency on* — Redis
- luz_docs — *is deployed as* — Kubernetes StatefulSet
- Kubernetes StatefulSet — *uses* — HPA
- HPA — *scales* — replicas
- WildFly JVMs — *are* — independent
- WildFly JVMs — *are* — non-clustered
- WildFly JVMs — *are behind* — load balancer
- WildFly JVMs — *have* — zero shared in-memory state
- mutable aggregate state — *includes* — counter
- mutable aggregate state — *includes* — HyperLogLog sketch
- mutable aggregate state — *includes* — in-memory cache
- mutable aggregate state — *can be stored in* — CDI bean
- mutable aggregate state — *can be stored in* — pod-local JVM memory
- HPA — *causes* — silent correctness bug
- silent correctness bug — *affects* — pod-local JVM memory
- HLL — *has* — inherent error
- _isPublic — *is a* — problem
- _effectiveSecurityClassCodes — *is a* — problem
- _shard — *is a* — problem
- problem — *is fixed by* — materialize derived state
- derived state — *is materialized into* — document store
- document store — *is* — MongoDB
- MongoDB — *uses* — write-path/materialize pattern
- derived state — *becomes* — centrally-persisted value
- new per-tenant/per-dimension aggregate — *should follow* — same rule
- pod-local memory — *should not represent* — tenant-wide state
- MicroProfile — *has no* — HyperLogLog utility
- WildFly — *has no* — HyperLogLog utility
- MicroProfile — *has no* — cardinality-sketch utility
- WildFly — *has no* — cardinality-sketch utility
- luz_docs countN badge — *can use* — HyperLogLog sketch
- HyperLogLog sketch — *can use* — fuzzy-zone fallback

%% ai-graph-end %%