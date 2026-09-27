---
ai_hash: 3a43ccdea0658b8a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-09
entities: []
source: session 2026-08-09 leo-customer360 terraform adapt
status: seedling
tags:
- terraform
- iac
- method
- config-drift
title: Adapt IaC to code by treating runtime config as the infra source of truth
type: howto
---

# Adapt IaC to code by treating runtime config as the infra source of truth

When asked to "adapt IaC (terraform) to the current code", treat the application's **actual runtime configuration** as the source of truth for what infrastructure must exist — not the IaC's own prior assumptions or its README narrative. The code tells you the real contract:

- **k8s ConfigMap / Secret** and **docker-compose env** — the exact env-var keys the app reads (`MINIO_BUCKET`, `KAFKA_BOOTSTRAP_SERVERS`, `KC_DB_URL`, `REDIS_PORT`, `S3_ENDPOINT`).
- **JDBC / connection URLs** — reveal database names (`.../db_keycloak`), ports, hosts.
- **App source** — which bucket/topic names are actually consumed, and whether a service is even wired up yet (e.g. Kafka topics defined in IaC but no producer/consumer exists = greenfield/forward-looking, flag it).

**Method:** enumerate every infra reference in the running config, diff against what the IaC provisions, and reconcile IaC to match. Concrete drifts found this way in LEO Customer360: IaC provisioned buckets `ingestion/exports/assets` but the app only ever reads one bucket via `MINIO_BUCKET`; IaC never created `db_keycloak`; IaC region was `hcm03` while the k8s vks overlay said `hcm04`.

**For ambiguous drifts (region hcm03 vs hcm04), flag loudly rather than silently pick** — weight the preponderance of signals (zone `HCM03-1A`, vDB `HCM-3` gateways all say hcm03) but leave the human the final call since it changes real, billable infra.

Related: [[Managed DB provisioners create the server but not in-database objects]].

## Related

- [[Managed DB provisioners create the server but not in-database objects]]

%% ai-graph-start %%

**Related notes:**
- [[leo-customer360 CD deploys app containers to vServers only, never the vDBvLBvStorage Terraform]]
- [[Managed DB provisioners create the server but not in-database objects]]
- [[CI-driven CD cannot resolve local gitignored Terraform state — needs remote backend or IPs via secrets]]
- [[Split Terraform cluster-provisioning state separate from in-cluster workload state]]
- [[Customer360 Kubernetes deployment (local kind + GreenNode VKS)]]

%% ai-graph-end %%