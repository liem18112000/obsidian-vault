---
ai_hash: 2f352180755cd75d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-03
entities:
- Customer360 Kubernetes deployment
- kind
- GreenNode VKS
- leo-customer360/k8s/
- CDP stack
- Kubernetes
- Kustomize
- terraform/
- base/
- components/in-cluster-data/
- overlays/local
- overlays/vks
- app tier
- API
- Frontend
- Dagster
- CIR worker
- Keycloak
- db-init job
- c360-config
- namespace
- Postgres
- Redis
- Kafka
- KRaft
- Minio
- seed jobs
- NodePorts
- Ingress
- registry images
- managed endpoints
- managed vDB
- managed vStorage
- k8s/scripts/up.sh
- kind cluster
- kubectl apply
- k8s/scripts/down.sh
- backend-system
- Dockerfile
- dagster dev
- workspace code locations
- DAGSTER_HOME
- PVC
- Secrets
- secretGenerator
- secret.env
- c360-secrets
- SSO_LOGIN
- header auth
- Keycloak realm seed
- Kustomize component
- service tier
- LEO Customer360 GreenNode Terraform infrastructure
- Customer360 GreenNode region split
- HCM03
- HCM04
- images
source: session 2026-08-03
status: seedling
tags:
- kubernetes
- kustomize
- kind
- vks
- customer360
- local-dev
title: Customer360 Kubernetes deployment (local kind + GreenNode VKS)
type: howto
---

# Customer360 Kubernetes deployment (local kind + GreenNode VKS)

`leo-customer360/k8s/` deploys the whole CDP stack on Kubernetes with Kustomize (base + components + overlays), mirroring the `terraform/` env pattern. Built local-first (kind) with a VKS overlay for GreenNode.

**Structure:** `base/` = app tier (api, frontend, dagster, cir worker, keycloak + db-init job, shared ConfigMap `c360-config`, namespace). `components/in-cluster-data/` = postgres, redis, kafka (single-node KRaft), minio + seed jobs. `overlays/local` = base + the data component + NodePorts + `:local` images; `overlays/vks` = base + Ingress + registry images + managed endpoints, and it OMITS the data component (app points at the managed vDB + vStorage from `terraform/`).

**Single-click:** `k8s/scripts/up.sh` = create kind cluster → build+load images → `kubectl apply -k overlays/local` → wait → print URLs (api :8008, frontend :8890, keycloak :8080, dagster :3000, minio :9000/:9001). `down.sh` deletes the cluster.

**Notable:** `backend-system` had NO Dockerfile — one was added to run `dagster dev` with the 7 workspace code locations (DAGSTER_HOME on a PVC). Secrets come from a per-overlay `secretGenerator` reading `secret.env` (gitignored, `disableNameSuffixHash: true` so base can reference `c360-secrets` by a stable name). Local runs with `SSO_LOGIN=false` (header auth) since there is no automated Keycloak realm seed.

## Related
- [[Kustomize component makes a service tier optional per environment]]
- [[LEO Customer360 GreenNode Terraform infrastructure]]
- [[Customer360 GreenNode region split compute HCM03, vStorage HCM04|Customer360 GreenNode region split: compute HCM03, vStorage HCM04]]

%% ai-graph-start %%

**Related notes:**
- [[Kustomize component makes a service tier optional per environment]]
- [[leo-customer360 deploys as Docker containers on VNG vServer VMs over SSH]]
- [[LEO Customer360 GreenNode Terraform infrastructure]]
- [[Customer360 UAT api box is a shared 1vCPU-2GB vServer running 5 containers]]
- [[Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoringLB]]

**Relations:**
- Customer360 Kubernetes deployment — *uses* — kind
- Customer360 Kubernetes deployment — *uses* — GreenNode VKS
- leo-customer360/k8s/ — *deploys* — CDP stack
- leo-customer360/k8s/ — *deploys on* — Kubernetes
- leo-customer360/k8s/ — *uses* — Kustomize
- leo-customer360/k8s/ — *mirrors* — terraform/
- leo-customer360/k8s/ — *built for* — kind
- leo-customer360/k8s/ — *has overlay for* — GreenNode VKS
- Kustomize — *structure includes* — base/
- Kustomize — *structure includes* — components/in-cluster-data/
- Kustomize — *structure includes* — overlays/local
- Kustomize — *structure includes* — overlays/vks
- base/ — *defines* — app tier
- app tier — *comprises* — API
- app tier — *comprises* — Frontend
- app tier — *comprises* — Dagster
- app tier — *comprises* — CIR worker
- app tier — *comprises* — Keycloak
- app tier — *comprises* — db-init job
- app tier — *uses* — c360-config
- app tier — *deploys in* — namespace
- components/in-cluster-data/ — *defines* — Postgres
- components/in-cluster-data/ — *defines* — Redis
- components/in-cluster-data/ — *defines* — Kafka
- components/in-cluster-data/ — *defines* — Minio
- components/in-cluster-data/ — *defines* — seed jobs
- Kafka — *uses* — KRaft
- overlays/local — *includes* — base/
- overlays/local — *includes* — components/in-cluster-data/
- overlays/local — *configures* — NodePorts
- overlays/local — *uses* — :local images
- overlays/vks — *includes* — base/
- overlays/vks — *configures* — Ingress
- overlays/vks — *uses* — registry images
- overlays/vks — *uses* — managed endpoints
- overlays/vks — *omits* — components/in-cluster-data/
- app tier — *connects to* — managed vDB
- app tier — *connects to* — managed vStorage
- managed vDB — *sourced from* — terraform/
- managed vStorage — *sourced from* — terraform/
- k8s/scripts/up.sh — *creates* — kind cluster
- k8s/scripts/up.sh — *builds and loads* — images
- k8s/scripts/up.sh — *applies Kustomize overlay* — overlays/local
- k8s/scripts/down.sh — *deletes* — kind cluster
- backend-system — *initially lacked* — Dockerfile
- Dockerfile — *added for* — backend-system
- Dockerfile — *enables* — dagster dev
- dagster dev — *uses* — workspace code locations
- DAGSTER_HOME — *persisted on* — PVC
- Secrets — *generated by* — secretGenerator
- secretGenerator — *reads* — secret.env
- base — *references* — c360-secrets
- Local runs — *sets* — SSO_LOGIN=false
- SSO_LOGIN=false — *enables* — header auth
- Kustomize component — *allows optional* — service tier
- Customer360 Kubernetes deployment — *related to* — Kustomize component
- Customer360 Kubernetes deployment — *related to* — LEO Customer360 GreenNode Terraform infrastructure
- Customer360 Kubernetes deployment — *related to* — Customer360 GreenNode region split
- Customer360 GreenNode region split — *uses compute* — HCM03
- Customer360 GreenNode region split — *uses vStorage* — HCM04

%% ai-graph-end %%