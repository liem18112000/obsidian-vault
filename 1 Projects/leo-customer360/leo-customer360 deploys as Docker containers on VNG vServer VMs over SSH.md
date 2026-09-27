---
ai_hash: 55b4a7dd68f2dc05
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-21
entities:
- leo-customer360
- LEOCDP Customer360
- Docker containers
- VNG Cloud vServer VMs
- SSH
- Kubernetes
- GHCR
- deployments/deploy-all.sh
- api
- ads
- frontend
- monitoring
- backend
- deployments/server/deploy-api.sh
- deployments/ads-server/deploy-ads.sh
- deployments/frontend/deploy-frontend.sh
- deployments/monitoring/deploy-monitoring.sh
- deployments/server/deploy-backend.sh
- UAT topology
- PROD topology
- api box
- backend box
- customer360-api
- ads-server
- frontend-admin
- Keycloak
- Redis
- Portainer
- Netdata
- Dagster
- MemStore
- NLB
- Caddy
- oauth2-proxy
- c360-oauth2-proxy
- customer360 realm
- beta.leocdp.com
- HCM03-1C
- FastAPI
- uvicorn
- Python 3.11
- SQLAlchemy 2
- Postgres
- pgvector
- PostGIS
- Kafka
- MinIO
- OpenTelemetry
- OTLP
- IaaS
- TLS
- path routing
- dashboards
- mon box
source: session 2026-08-21
status: seedling
tags:
- leo-customer360
- LEOCDP
- VNG-Cloud
- infra
- deployment
title: leo-customer360 deploys as Docker containers on VNG vServer VMs over SSH
type: observation
---

# leo-customer360 deploys as Docker containers on VNG vServer VMs over SSH

The **leo-customer360 / LEOCDP Customer360** platform deploys to **VNG Cloud vServer VMs** (IaaS), **not Kubernetes** — the `k8s/` dir in the repo is a local/alt path only. Each service runs as a **Docker container on the VM**, images are CI-built and pulled from **GHCR**, and deployment happens **over SSH** via per-module bash scripts orchestrated by `deployments/deploy-all.sh`.

## Deploy orchestration
`deploy-all.sh <uat|prod>` runs steps in dependency order: storage, postgres, server, db-schema, cache, sso, backend, load-balancer, proxy, sso-realm, api, frontend, ads, monitoring, seed.

Step -> script map:
- api -> `deployments/server/deploy-api.sh`
- ads -> `deployments/ads-server/deploy-ads.sh`
- frontend -> `deployments/frontend/deploy-frontend.sh`
- monitoring -> `deployments/monitoring/deploy-monitoring.sh`
- backend (Dagster) -> `deployments/server/deploy-backend.sh`

Deploy scripts build the container env locally and ship it **base64-encoded** over SSH, then `docker run` on the box.

## UAT topology (small!)
2 boxes, both `s-general-1x2` = **1 vCPU / 2 GB**:
- **api box** (private 10.100.1.5) co-locates EVERYTHING: customer360-api(8008) + ads-server(9009) + frontend-admin(8890) + Keycloak(8080/9000) + Redis(6580, container) + Portainer(9443) + Netdata(19999).
- **backend box**: Dagster only (this is the "except dagster" split).

## PROD topology
Dedicated vServer per service: api (`s2-general-4x8`), sso (Keycloak), frontend, ads (4x8). Redis becomes managed **MemStore**. Monitoring defaults to the api box but the overlay recommends a dedicated `mon` box (`mon_server_key`) once under load.

## Front door & misc
L4 **NLB -> Caddy** reverse proxy (TLS + path routing) on the box. The LB can't do OIDC, so **oauth2-proxy** (Keycloak confidential client `c360-oauth2-proxy` in the `customer360` realm) gates the dashboards. Public host **beta.leocdp.com**. Only AZ enabled on this account is **HCM03-1C**.

Stack: FastAPI/uvicorn (Python 3.11) + SQLAlchemy 2 + Postgres (pgvector/PostGIS) + Redis + Keycloak + Dagster + Kafka + MinIO.

## Related
[[Trace FastAPI with OpenTelemetry zero-code instrumentation emitting OTLP]]

## Related

- [[Trace FastAPI with OpenTelemetry zero-code instrumentation emitting OTLP]]

%% ai-graph-start %%

**Related notes:**
- [[Customer360 UAT api box is a shared 1vCPU-2GB vServer running 5 containers]]
- [[Customer360 Kubernetes deployment (local kind + GreenNode VKS)]]
- [[leo-customer360 tracing OTel off-by-default on UAT, on at 10% on PROD]]
- [[Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoringLB]]
- [[leo-customer360 VNG deploy builds app images on the VM from a tarred local checkout, not from a registry]]

**Relations:**
- leo-customer360 — *is_also_known_as* — LEOCDP Customer360
- leo-customer360 — *deploys_as* — Docker containers
- Docker containers — *run_on* — VNG Cloud vServer VMs
- VNG Cloud vServer VMs — *is_type* — IaaS
- leo-customer360 — *does_not_use* — Kubernetes
- Docker containers — *images_pulled_from* — GHCR
- deployment — *happens_over* — SSH
- deployment — *orchestrated_by* — deployments/deploy-all.sh
- deployments/deploy-all.sh — *orchestrates_step* — api
- deployments/deploy-all.sh — *orchestrates_step* — ads
- deployments/deploy-all.sh — *orchestrates_step* — frontend
- deployments/deploy-all.sh — *orchestrates_step* — monitoring
- deployments/deploy-all.sh — *orchestrates_step* — backend
- api — *uses_script* — deployments/server/deploy-api.sh
- ads — *uses_script* — deployments/ads-server/deploy-ads.sh
- frontend — *uses_script* — deployments/frontend/deploy-frontend.sh
- monitoring — *uses_script* — deployments/monitoring/deploy-monitoring.sh
- backend — *uses_script* — deployments/server/deploy-backend.sh
- deploy scripts — *ship_env_as* — base64-encoded
- base64-encoded — *sent_over* — SSH
- UAT topology — *includes_vm* — api box
- UAT topology — *includes_vm* — backend box
- api box — *co-locates* — customer360-api
- api box — *co-locates* — ads-server
- api box — *co-locates* — frontend-admin
- api box — *co-locates* — Keycloak
- api box — *co-locates* — Redis
- api box — *co-locates* — Portainer
- api box — *co-locates* — Netdata
- backend box — *runs_service* — Dagster
- PROD topology — *uses_dedicated_vServer_for* — api
- PROD topology — *uses_dedicated_vServer_for* — Keycloak
- PROD topology — *uses_dedicated_vServer_for* — frontend
- PROD topology — *uses_dedicated_vServer_for* — ads
- PROD topology — *uses_managed_service* — MemStore
- MemStore — *is_type_of* — Redis
- PROD topology — *recommends_dedicated_vm_for* — monitoring
- NLB — *routes_to* — Caddy
- Caddy — *is_type* — reverse proxy
- Caddy — *provides* — TLS
- Caddy — *provides* — path routing
- oauth2-proxy — *gates* — dashboards
- oauth2-proxy — *is_client* — c360-oauth2-proxy
- c360-oauth2-proxy — *in_realm* — customer360 realm
- customer360 realm — *managed_by* — Keycloak
- beta.leocdp.com — *is_public_host_for* — leo-customer360
- HCM03-1C — *is_enabled_AZ_for* — account
- Stack — *includes* — FastAPI
- Stack — *includes* — uvicorn
- Stack — *includes* — Python 3.11
- Stack — *includes* — SQLAlchemy 2
- Stack — *includes* — Postgres
- Postgres — *uses_extension* — pgvector
- Postgres — *uses_extension* — PostGIS
- Stack — *includes* — Redis
- Stack — *includes* — Keycloak
- Stack — *includes* — Dagster
- Stack — *includes* — Kafka
- Stack — *includes* — MinIO
- FastAPI — *can_be_traced_with* — OpenTelemetry
- OpenTelemetry — *emits* — OTLP

%% ai-graph-end %%