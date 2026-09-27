---
ai_hash: 8e1d1f179733af01
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-03
entities:
- Customer360
- Kubernetes
- kind
- GreenNode VKS
- '`leo-customer360/k8s/`'
- CDP stack
- Kustomize
- '`terraform/`'
- '`base/`'
- '`components/in-cluster-data/`'
- '`overlays/local`'
- '`overlays/vks`'
- API
- Frontend
- Dagster
- CIR Worker
- Keycloak
- DB-init job
- '`c360-config`'
- Postgres
- Redis
- Kafka
- Minio
- Seed jobs
- NodePorts
- Local Images
- Ingress
- Registry Images
- Managed Endpoints
- Managed vDB
- Managed vStorage
- '`k8s/scripts/up.sh`'
- '`k8s/scripts/down.sh`'
- '`backend-system`'
- Dockerfile
- '`dagster dev`'
- DAGSTER_HOME
- PVC
- Kubernetes Secrets
- '`secretGenerator`'
- '`secret.env`'
- '`c360-secrets`'
- SSO_LOGIN
- Header Auth
- Keycloak realm seed
- Kustomize component makes a service tier optional per environment
- LEO Customer360 GreenNode Terraform infrastructure
- 'Customer360 GreenNode region split: compute HCM03, vStorage HCM04'
- HCM03
- HCM04
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
- [[Customer360 GreenNode region split: compute HCM03, vStorage HCM04]]

%% ai-graph-start %%

**Related notes:**
- [[Kustomize component makes a service tier optional per environment]]
- [[leo-customer360 deploys as Docker containers on VNG vServer VMs over SSH]]
- [[LEO Customer360 GreenNode Terraform infrastructure]]
- [[Customer360 UAT api box is a shared 1vCPU-2GB vServer running 5 containers]]
- [[Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoringLB]]

**Relations:**
- Customer360 — *has deployment on* — Kubernetes
- Kubernetes — *supports local environment* — kind
- Kubernetes — *supports cloud environment* — GreenNode VKS
- `leo-customer360/k8s/` — *deploys* — CDP stack
- `leo-customer360/k8s/` — *uses* — Kustomize
- `leo-customer360/k8s/` — *mirrors env pattern of* — `terraform/`
- Kustomize — *includes component* — `base/`
- Kustomize — *includes component* — `components/in-cluster-data/`
- Kustomize — *includes overlay* — `overlays/local`
- Kustomize — *includes overlay* — `overlays/vks`
- `base/` — *defines* — API
- `base/` — *defines* — Frontend
- `base/` — *defines* — Dagster
- `base/` — *defines* — CIR Worker
- `base/` — *defines* — Keycloak
- `base/` — *defines* — DB-init job
- `base/` — *defines* — `c360-config`
- `components/in-cluster-data/` — *includes* — Postgres
- `components/in-cluster-data/` — *includes* — Redis
- `components/in-cluster-data/` — *includes* — Kafka
- `components/in-cluster-data/` — *includes* — Minio
- `components/in-cluster-data/` — *includes* — Seed jobs
- `overlays/local` — *uses* — `base/`
- `overlays/local` — *uses* — `components/in-cluster-data/`
- `overlays/local` — *configures* — NodePorts
- `overlays/local` — *uses* — Local Images
- `overlays/vks` — *uses* — `base/`
- `overlays/vks` — *configures* — Ingress
- `overlays/vks` — *uses* — Registry Images
- `overlays/vks` — *configures* — Managed Endpoints
- `overlays/vks` — *omits* — `components/in-cluster-data/`
- `overlays/vks` — *connects to* — Managed vDB
- `overlays/vks` — *connects to* — Managed vStorage
- Managed vDB — *provisioned by* — `terraform/`
- Managed vStorage — *provisioned by* — `terraform/`
- `k8s/scripts/up.sh` — *creates* — kind
- `k8s/scripts/up.sh` — *builds and loads* — Local Images
- `k8s/scripts/up.sh` — *applies* — `overlays/local`
- `k8s/scripts/down.sh` — *deletes* — kind
- `backend-system` — *lacked* — Dockerfile
- Dockerfile — *added for* — `backend-system`
- Dockerfile — *enables* — `dagster dev`
- `dagster dev` — *uses* — DAGSTER_HOME
- DAGSTER_HOME — *stored on* — PVC
- Kubernetes Secrets — *generated by* — `secretGenerator`
- `secretGenerator` — *reads from* — `secret.env`
- `secret.env` — *is* — gitignored
- `secretGenerator` — *configures* — `disableNameSuffixHash: true`
- `base/` — *references* — `c360-secrets`
- `overlays/local` — *runs with* — `SSO_LOGIN=false`
- `SSO_LOGIN=false` — *enables* — Header Auth
- `overlays/local` — *lacks automated* — Keycloak realm seed
- API — *has port* — :8008
- Frontend — *has port* — :8890
- Keycloak — *has port* — :8080
- Dagster — *has port* — :3000
- Minio — *has port* — :9000/:9001
- Kustomize component makes a service tier optional per environment — *relates to* — Kustomize
- LEO Customer360 GreenNode Terraform infrastructure — *relates to* — Customer360
- LEO Customer360 GreenNode Terraform infrastructure — *relates to* — GreenNode
- LEO Customer360 GreenNode Terraform infrastructure — *relates to* — `terraform/`
- Customer360 GreenNode region split: compute HCM03, vStorage HCM04 — *relates to* — Customer360
- Customer360 GreenNode region split: compute HCM03, vStorage HCM04 — *relates to* — GreenNode
- HCM03 — *is type* — compute
- HCM04 — *is type* — vStorage
- GreenNode — *provides compute in* — HCM03
- GreenNode — *provides vStorage in* — HCM04

%% ai-graph-end %%