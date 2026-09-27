---
ai_hash: 1d07e0e08666185c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-04
entities:
- postgres skill
- pg.ps1
- luz-store-invoice-run-v2
- kubectl port-forward
- kubectl
- klara-nonprod GKE context
- kind-customer360
- dev namespace
- rancher-desktop
- GKE
- klara-nonprod
- europe-west6-a
- Postgres
- kubectl config use-context
- kubectl config current-context
- dev cluster
source: session 2026-08-04
status: seedling
tags:
- kubectl
- context
- luz-alloydb
- postgres-skill
- dev
- gotcha
title: postgres skill dev port-forward needs the klara-nonprod GKE context, not kind-customer360
type: lesson
---

# postgres skill dev port-forward needs the klara-nonprod GKE context, not kind-customer360

The `postgres` skill (`pg.ps1`, used by `luz-store-invoice-run-v2`) opens its dev connection via `kubectl port-forward service/luz-alloydb-main 5432:5432 -n dev`. That port-forward runs against **whatever the current kubectl context is** — so if the context has no `dev` namespace it fails with:

```
Error from server (NotFound): namespaces "dev" not found
port-forward did not come up on 127.0.0.1:5432 within ~16s.
```

The local default context `kind-customer360` (and `rancher-desktop`) have no `dev` namespace. The dev cluster is the GKE one:

```powershell
kubectl config use-context gke_klara-nonprod_europe-west6-a_klara-nonprod
```

Run that first, then the skill queries connect. This matches the org default (klara-nonprod / dev / europe-west6). If a query suddenly cannot reach Postgres on dev, check `kubectl config current-context` before assuming the DB or the skill is broken.

## Related

- [[luz-store-invoice-run-v2]]

%% ai-graph-start %%

**Related notes:**
- [[Verify kubectl context before GKE rollout - _context file can disagree]]
- [[luz-store on dev is a Deployment, not a StatefulSet — roll out with kubectl set image]]
- [[Local access to GKE-hosted Luz services port-forward api-forwarder + luz-vault (+ mongo pod)]]
- [[Run local luz-jsonstore against a real tenant GKE Mongo via port-forwards]]
- [[klara-prod is a separate GCP project, not a namespace]]

**Relations:**
- postgres skill — *uses* — pg.ps1
- pg.ps1 — *is used by* — luz-store-invoice-run-v2
- postgres skill — *opens dev connection via* — kubectl port-forward
- kubectl port-forward — *runs against* — kubectl context
- kind-customer360 — *is a* — kubectl context
- rancher-desktop — *is a* — kubectl context
- klara-nonprod GKE context — *is a* — kubectl context
- kind-customer360 — *has no* — dev namespace
- rancher-desktop — *has no* — dev namespace
- klara-nonprod GKE context — *is the* — dev cluster
- dev cluster — *is* — GKE
- GKE — *is* — klara-nonprod
- klara-nonprod — *is in zone* — europe-west6-a
- kubectl config use-context — *sets* — kubectl context
- kubectl config current-context — *checks* — kubectl context
- skill — *queries connect to* — Postgres
- Postgres — *is on* — dev
- luz-store-invoice-run-v2 — *is related to* — postgres skill
- klara-nonprod GKE context — *is needed for* — postgres skill dev port-forward
- kind-customer360 — *is not suitable for* — postgres skill dev port-forward
- dev namespace — *is required for* — kubectl port-forward

%% ai-graph-end %%