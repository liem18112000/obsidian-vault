---
ai_hash: d418820e90b9f6b7
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-12
entities:
- leo-customer360
- main branch
- UAT environment
- DEFAULT_SVC
- sso-realm
- api service
- backend service
- ads service
- frontend service
- tracking service
- docs-search service
- monitoring step
- Portainer
- Netdata
- oauth2-proxy
- Portainer agents
- Continuous Deployment (CD)
- cd.yml workflow
- Actions UI
- deploy-all.sh script
- portainer_agent_server_keys
- deployments/monitoring/overlays/<env>.tfvars
- docs server key
- portainer/agent:lts image
- PORTAINER_ADMIN_PASSWORD
- Portainer environment
- 9001 ingress
- Default secgroup
- api/Portainer box
- infra Terraform apply
- CD secrets
- cd.yml deploy step env
- GitHub
source: session 2026-09-12
status: seedling
tags:
- leo-customer360
- ci-cd
- portainer
- monitoring
- deployment
title: leo-customer360 push to main skips the monitoring step; deploy Portainer agents
  manually
type: lesson
---

# leo-customer360 push to main skips the monitoring step; deploy Portainer agents manually

A push/merge to `main` in leo-customer360 auto-deploys UAT with only `DEFAULT_SVC = sso-realm,api,backend,ads,frontend,tracking,docs-search`. The **`monitoring`** step (Portainer + Netdata + oauth2-proxy) is deliberately **not** in that list.

**So:** infra like Portainer agents will never deploy from a normal push. To run it, trigger CD manually — `gh workflow run cd.yml -f environment=uat -f image_tag=sha-<sha> -f services=monitoring` (or Actions UI → Run workflow) — or run `bash deployments/deploy-all.sh uat --only monitoring -y` locally.

**Which boxes get a Portainer agent** is driven by `portainer_agent_server_keys` (comma-separated `../server` keys) in `deployments/monitoring/overlays/<env>.tfvars`. Adding a key (e.g. `docs`) installs `portainer/agent:lts` on that box at `:9001` and, if `PORTAINER_ADMIN_PASSWORD` is present, registers it as a Portainer environment. The 9001 ingress is already open on the shared Default secgroup from the api/Portainer box, so no infra Terraform apply is needed for a new agent.

## Related
[[CD secrets must be wired into cd.yml deploy step env, not just added to GitHub]]

## Related

- [[CD secrets must be wired into cd.yml deploy step env, not just added to GitHub]]

%% ai-graph-start %%

**Related notes:**
- [[CD secrets must be wired into cd.yml deploy step env, not just added to GitHub]]
- [[Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoringLB]]
- [[leo-customer360 CD UAT deploys only from main + --deploy-uat marker]]
- [[CI-driven CD cannot resolve local gitignored Terraform state — needs remote backend or IPs via secrets]]
- [[leo-customer360 CD deploys app containers to vServers only, never the vDBvLBvStorage Terraform]]

**Relations:**
- leo-customer360 — *uses* — main branch
- main branch — *push triggers* — UAT environment auto-deployment
- UAT environment auto-deployment — *includes* — DEFAULT_SVC
- DEFAULT_SVC — *comprises* — sso-realm
- DEFAULT_SVC — *comprises* — api service
- DEFAULT_SVC — *comprises* — backend service
- DEFAULT_SVC — *comprises* — ads service
- DEFAULT_SVC — *comprises* — frontend service
- DEFAULT_SVC — *comprises* — tracking service
- DEFAULT_SVC — *comprises* — docs-search service
- main branch — *push skips* — monitoring step
- monitoring step — *includes* — Portainer
- monitoring step — *includes* — Netdata
- monitoring step — *includes* — oauth2-proxy
- Portainer agents — *not deployed by* — main branch push
- Portainer agents — *deployed by* — Continuous Deployment (CD) manual trigger
- Continuous Deployment (CD) manual trigger — *via* — cd.yml workflow
- Continuous Deployment (CD) manual trigger — *via* — Actions UI
- Continuous Deployment (CD) manual trigger — *via* — deploy-all.sh script
- Portainer agents — *deployment configured by* — portainer_agent_server_keys
- portainer_agent_server_keys — *defined in* — deployments/monitoring/overlays/<env>.tfvars
- portainer_agent_server_keys — *is a list of* — server keys
- docs server key — *is an example of* — server keys
- adding a server key — *installs* — portainer/agent:lts image
- portainer/agent:lts image — *installs on* — box
- box — *listens on port* — 9001
- PORTAINER_ADMIN_PASSWORD — *enables registration of* — Portainer environment
- 9001 ingress — *is open on* — Default secgroup
- Default secgroup — *originates from* — api/Portainer box
- new agent — *does not require* — infra Terraform apply
- CD secrets — *must be wired into* — cd.yml deploy step env
- CD secrets — *should not just be added to* — GitHub

%% ai-graph-end %%