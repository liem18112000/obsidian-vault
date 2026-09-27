---
title: "leo-customer360 push to main skips the monitoring step; deploy Portainer agents manually"
created: 2026-09-12
type: lesson
status: seedling
source: "session 2026-09-12"
tags: [leo-customer360, ci-cd, portainer, monitoring, deployment]
---

# leo-customer360 push to main skips the monitoring step; deploy Portainer agents manually

A push/merge to `main` in leo-customer360 auto-deploys UAT with only `DEFAULT_SVC = sso-realm,api,backend,ads,frontend,tracking,docs-search`. The **`monitoring`** step (Portainer + Netdata + oauth2-proxy) is deliberately **not** in that list.

**So:** infra like Portainer agents will never deploy from a normal push. To run it, trigger CD manually — `gh workflow run cd.yml -f environment=uat -f image_tag=sha-<sha> -f services=monitoring` (or Actions UI → Run workflow) — or run `bash deployments/deploy-all.sh uat --only monitoring -y` locally.

**Which boxes get a Portainer agent** is driven by `portainer_agent_server_keys` (comma-separated `../server` keys) in `deployments/monitoring/overlays/<env>.tfvars`. Adding a key (e.g. `docs`) installs `portainer/agent:lts` on that box at `:9001` and, if `PORTAINER_ADMIN_PASSWORD` is present, registers it as a Portainer environment. The 9001 ingress is already open on the shared Default secgroup from the api/Portainer box, so no infra Terraform apply is needed for a new agent.

## Related
[[CD secrets must be wired into cd.yml deploy step env, not just added to GitHub]]

## Related

- [[CD secrets must be wired into cd.yml deploy step env]]
- [[not just added to GitHub]]
