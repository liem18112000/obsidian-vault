---
ai_hash: 6f48212930042a14
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-03
entities: []
source: session 2026-08-03
status: seedling
tags:
- kubernetes
- kustomize
- component
- overlays
- technique
title: Kustomize component makes a service tier optional per environment
type: lesson
---

# Kustomize component makes a service tier optional per environment

To run the same app against different backing services per environment (in-cluster data locally, managed/cloud data in prod), put the swappable tier in a **Kustomize Component**, not in `base`.

Pattern: `base/` holds only the env-agnostic app tier. A `components/in-cluster-data/` dir (with `kind: Component`, `apiVersion: kustomize.config.k8s.io/v1alpha1`) holds Postgres/Redis/Kafka/MinIO. The `local` overlay lists it under `components:` so those StatefulSets render; the `prod`/`vks` overlay omits it and instead patches the app's ConfigMap to point at managed endpoints. This is cleaner than putting data services in base and deleting them per-overlay with fragile `$patch: delete` directives.

Supporting tricks used together:
- Per-overlay `secretGenerator` reading a gitignored `secret.env`, with `generatorOptions.disableNameSuffixHash: true` so `base` can reference the Secret by a stable name (`c360-secrets`) without a hash suffix.
- `images:` transformer in each overlay to swap `:local` (kind-loaded) for registry images.
- Verify any overlay renders with `kubectl kustomize overlays/<env>` before applying.

## Related
- [[Customer360 Kubernetes deployment (local kind + GreenNode VKS)]]

%% ai-graph-start %%

**Related notes:**
- [[Customer360 Kubernetes deployment (local kind + GreenNode VKS)]]
- [[LEO Customer360 GreenNode Terraform infrastructure]]
- [[Adapt IaC to code by treating runtime config as the infra source of truth]]
- [[leo-customer360 CD deploys app containers to vServers only, never the vDBvLBvStorage Terraform]]
- [[A local bring-up script must pin kubectl --context or it deploys to the wrong cluster]]

%% ai-graph-end %%