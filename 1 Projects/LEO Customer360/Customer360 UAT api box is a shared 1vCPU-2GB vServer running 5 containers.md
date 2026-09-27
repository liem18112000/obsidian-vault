---
ai_hash: 4a8760c8f8c2e491
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-19
entities:
- Customer360 UAT api box
- vServer
- containers
- leo-customer360 repo
- UAT deployment
- Docker
- VM
- api server key
- s-general-1x2
- 1 vCPU
- 2 GB RAM
- 20 GB disk
- HCM03-1C
- deployments/server/overlays/uat.tfvars
- redis service
- c360-redis
- '6580'
- redis-cli PING
- api-server service
- customer360-api
- '8008'
- /health
- ads-server service
- customer360-ads
- '9009'
- Keycloak (SSO) service
- c360-keycloak
- '8080'
- '9000'
- :9000/health/ready
- frontend-admin service
- customer360-frontend
- '8890'
- /
- JVM
- production environment
- dedicated vServers
- ads server key
- sso server key
- frontend server key
- host networking
- Portainer
- cAdvisor
- deploy scripts
- floating IP
- ../server Terraform outputs
- server key
- Monitoring the Customer360 box - self-hosted Grafana is free, cost is resource pressure
- LEO Customer360 GreenNode Terraform infrastructure
- Grafana
- Terraform
source: session 2026-08-19
status: seedling
tags:
- customer360
- deployment
- vngcloud
- greennode
- docker
- topology
- uat
title: Customer360 UAT api box is a shared 1vCPU/2GB vServer running 5 containers
type: reference
---

# Customer360 UAT api box is a shared 1vCPU/2GB vServer running 5 containers

In the `leo-customer360` repo, the UAT deployment co-locates five services as Docker
containers on ONE small VM — the `api` server key, flavor **`s-general-1x2` = 1 vCPU /
2 GB RAM / 20 GB disk** (HCM03-1C), defined in `deployments/server/overlays/uat.tfvars`.

All run with `--network host --restart unless-stopped`:

| Service | Container name | Host port(s) | Health |
|---|---|---|---|
| redis | `c360-redis` | 6580 | `redis-cli PING` |
| api-server | `customer360-api` | 8008 | `/health` |
| ads-server | `customer360-ads` | 9009 | `/health` |
| keycloak (SSO) | `c360-keycloak` | 8080 + **9000** (mgmt/health) | `:9000/health/ready` |
| frontend-admin | `customer360-frontend` | 8890 | `/` |

Keycloak is the JVM heavyweight (~400–700 MB). In **prod** these split onto dedicated
vServers (server keys `ads`, `sso`, `frontend`); UAT shares the one box to save cost.

**Gotcha — host networking means ports collide.** Anything new added to this box must
dodge 6580 / 8008 / 8080 / **9000** / 8890 / 9009. Notably Keycloak owns **:9000**, so
don't put Portainer (legacy HTTP default 9000) or cAdvisor (default 8080) there.

Deploy scripts: `deployments/{cache,server,ads-server,sso,frontend}/deploy-*.sh`; each
discovers the target VM's floating IP from `../server` Terraform outputs by server key.

Related: [[Monitoring the Customer360 box - self-hosted Grafana is free, cost is resource pressure]] · [[LEO Customer360 GreenNode Terraform infrastructure]]

%% ai-graph-start %%

**Related notes:**
- [[leo-customer360 deploys as Docker containers on VNG vServer VMs over SSH]]
- [[Verify uat customer360-api health publicly at beta.leocdp.comc360apihealth]]
- [[Deploying Keycloak 26 as a container health port 9000, bootstrap admin, start vs start-dev]]
- [[Customer360 Kubernetes deployment (local kind + GreenNode VKS)]]
- [[UAT vServer Dagster topology split webserver+daemon on one s-general box]]

**Relations:**
- Customer360 UAT api box — *IS_A* — vServer
- Customer360 UAT api box — *HAS_SPEC* — 1 vCPU
- Customer360 UAT api box — *HAS_SPEC* — 2 GB RAM
- Customer360 UAT api box — *HAS_SPEC* — 20 GB disk
- Customer360 UAT api box — *RUNS* — 5 containers
- UAT deployment — *LOCATED_IN* — leo-customer360 repo
- UAT deployment — *CO_LOCATES* — 5 services
- 5 services — *ARE_IMPLEMENTED_AS* — Docker containers
- Docker containers — *RUN_ON* — VM
- VM — *IS* — Customer360 UAT api box
- VM — *HAS_KEY* — api server key
- api server key — *HAS_FLAVOR* — s-general-1x2
- s-general-1x2 — *DEFINED_IN* — deployments/server/overlays/uat.tfvars
- s-general-1x2 — *CORRESPONDS_TO* — HCM03-1C
- containers — *USE_NETWORK_MODE* — host networking
- containers — *RESTART_POLICY* — unless-stopped
- redis service — *HAS_CONTAINER* — c360-redis
- redis service — *EXPOSES_PORT* — 6580
- redis service — *HEALTH_CHECK_COMMAND* — redis-cli PING
- api-server service — *HAS_CONTAINER* — customer360-api
- api-server service — *EXPOSES_PORT* — 8008
- api-server service — *HEALTH_CHECK_ENDPOINT* — /health
- ads-server service — *HAS_CONTAINER* — customer360-ads
- ads-server service — *EXPOSES_PORT* — 9009
- ads-server service — *HEALTH_CHECK_ENDPOINT* — /health
- Keycloak (SSO) service — *HAS_CONTAINER* — c360-keycloak
- Keycloak (SSO) service — *EXPOSES_PORT* — 8080
- Keycloak (SSO) service — *EXPOSES_PORT* — 9000
- Keycloak (SSO) service — *HEALTH_CHECK_ENDPOINT* — :9000/health/ready
- Keycloak (SSO) service — *IS_A* — JVM heavyweight
- frontend-admin service — *HAS_CONTAINER* — customer360-frontend
- frontend-admin service — *EXPOSES_PORT* — 8890
- frontend-admin service — *HEALTH_CHECK_ENDPOINT* — /
- production environment — *USES* — dedicated vServers
- dedicated vServers — *FOR_SERVICE* — ads server key
- dedicated vServers — *FOR_SERVICE* — sso server key
- dedicated vServers — *FOR_SERVICE* — frontend server key
- UAT deployment — *SHARES_RESOURCE* — Customer360 UAT api box
- host networking — *CAUSES* — ports collide
- ports collide — *INVOLVES_PORT* — 6580
- ports collide — *INVOLVES_PORT* — 8008
- ports collide — *INVOLVES_PORT* — 8080
- ports collide — *INVOLVES_PORT* — 9000
- ports collide — *INVOLVES_PORT* — 8890
- ports collide — *INVOLVES_PORT* — 9009
- Keycloak (SSO) service — *OWNS_PORT* — 9000
- Portainer — *HAS_DEFAULT_PORT* — 9000
- cAdvisor — *HAS_DEFAULT_PORT* — 8080
- deploy scripts — *ARE_LOCATED_AT* — deployments/{cache,server,ads-server,sso,frontend}/deploy-*.sh
- deploy scripts — *DISCOVER* — floating IP
- floating IP — *FROM* — ../server Terraform outputs
- ../server Terraform outputs — *USES* — server key
- Customer360 UAT api box — *RELATED_NOTE* — Monitoring the Customer360 box - self-hosted Grafana is free, cost is resource pressure
- Customer360 UAT api box — *RELATED_NOTE* — LEO Customer360 GreenNode Terraform infrastructure
- Monitoring the Customer360 box - self-hosted Grafana is free, cost is resource pressure — *MENTIONS* — Grafana
- LEO Customer360 GreenNode Terraform infrastructure — *MENTIONS* — Terraform

%% ai-graph-end %%