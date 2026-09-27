---
ai_hash: d314cd714fa27127
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-12
entities:
- leo-customer360
- main branch
- UAT environment
- DEFAULT_SVC
- sso-realm service
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
- gh workflow run cd.yml command
- cd.yml workflow
- Actions UI
- deployments/deploy-all.sh script
- portainer_agent_server_keys variable
- deployments/monitoring/overlays/<env>.tfvars file
- docs server key
- server
- portainer/agent:lts image
- 9001 port
- PORTAINER_ADMIN_PASSWORD variable
- Portainer environment
- Default secgroup
- api/Portainer box
- CD secrets must be wired into cd.yml deploy step env, not just added to GitHub note
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
- [[leo-customer360 CD deploys app containers to vServers only, never the vDBvLBvStorage Terraform]]
- [[CI-driven CD cannot resolve local gitignored Terraform state — needs remote backend or IPs via secrets]]

**Relations:**
- leo-customer360 — *pushes to* — main branch
- main branch — *triggers auto-deployment of* — UAT environment
- UAT environment — *deploys* — DEFAULT_SVC
- DEFAULT_SVC — *includes* — sso-realm service
- DEFAULT_SVC — *includes* — api service
- DEFAULT_SVC — *includes* — backend service
- DEFAULT_SVC — *includes* — ads service
- DEFAULT_SVC — *includes* — frontend service
- DEFAULT_SVC — *includes* — tracking service
- DEFAULT_SVC — *includes* — docs-search service
- monitoring step — *is not included in* — DEFAULT_SVC
- monitoring step — *includes* — Portainer
- monitoring step — *includes* — Netdata
- monitoring step — *includes* — oauth2-proxy
- Portainer agents — *are part of* — monitoring step
- Portainer agents — *do not deploy from* — main branch
- Portainer agents — *deploy via* — Continuous Deployment (CD)
- Continuous Deployment (CD) — *can be triggered by* — gh workflow run cd.yml command
- Continuous Deployment (CD) — *can be triggered by* — Actions UI
- Continuous Deployment (CD) — *can be triggered by* — deployments/deploy-all.sh script
- gh workflow run cd.yml command — *uses* — cd.yml workflow
- deployments/deploy-all.sh script — *deploys* — monitoring step
- Portainer agents — *deployment is driven by* — portainer_agent_server_keys variable
- portainer_agent_server_keys variable — *is defined in* — deployments/monitoring/overlays/<env>.tfvars file
- docs server key — *is an example of* — portainer_agent_server_keys variable
- adding docs server key — *installs* — portainer/agent:lts image
- portainer/agent:lts image — *installs on* — server
- portainer/agent:lts image — *uses* — 9001 port
- PORTAINER_ADMIN_PASSWORD variable — *registers* — Portainer environment
- 9001 port — *ingress is open on* — Default secgroup
- Default secgroup — *is from* — api/Portainer box
- CD secrets must be wired into cd.yml deploy step env, not just added to GitHub note — *is related to* — Continuous Deployment (CD)
- CD secrets must be wired into cd.yml deploy step env, not just added to GitHub note — *is related to* — cd.yml workflow

%% ai-graph-end %%