---
ai_hash: 8a3da9b619df5e8b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-13
entities:
- docs-search UAT latency
- docs-vector-search chatbot
- UAT environment
- VNG cloud
- security-group ingress rule
- docs service port 8001
- API box
- IP address 10.100.1.5
- deployments/server/overlays/uat.tfvars
- Continuous Deployment (CD)
- Terraform
- deploy.sh script
- docs box
- 127.0.0.1:8001/health endpoint
- uvicorn
- frontend-admin
- GET /ai/health endpoint
- frontend-admin proxy
- IP address 10.100.1.7
- SYN dropped
- firewall
- connection refused
- DOCS_PROXY_TIMEOUT_SECONDS
- Caddy
- /docs-ai route
- TARGET environment variable
- vngcloud_vserver_secgrouprule.extra["8001-10.100.1.5/32"]
- instances
- backend root disk
- leo-customer360
- Portainer agents
- CD secrets
- cd.yml deploy step env
- GitHub
- monitoring step
source: session 2026-09-13
status: seedling
tags:
- leo-customer360
- networking
- terraform
- vngcloud
- troubleshooting
- docs-search
title: 'docs-search UAT latency root cause: unapplied 8001 secgroup ingress (api to
  docs box)'
type: lesson
---

# docs-search UAT latency root cause: unapplied 8001 secgroup ingress (api to docs box)

On UAT, the docs-vector-search chatbot hung ~120s per request even though the service was perfectly healthy. Root cause: the VNG cloud **security-group ingress rule opening docs `:8001` from the api box (10.100.1.5)** existed in `deployments/server/overlays/uat.tfvars` but had **never been applied** — CD never runs infra Terraform, so tfvars secgroup edits stay dormant until someone runs `./deploy.sh uat apply` out-of-band.

**How it presented / how to diagnose:**
- Service itself fine: on the docs box `curl 127.0.0.1:8001/health` → 200 in ~10ms; container up; no OOM; uvicorn bound `0.0.0.0:8001`; no on-box firewall (ufw inactive, iptables INPUT ACCEPT).
- Frontend fine: `GET https://<host>/health` (frontend-admin own) → 200 fast.
- The tell: `GET /ai/health` (frontend-admin proxy → docs) **connects instantly but TTFB never arrives, hangs to timeout**; and from the api box `curl -m5 http://10.100.1.7:8001/health` **times out** (SYN dropped = firewall, not "connection refused").
- The 120s hang == frontend-admin `DOCS_PROXY_TIMEOUT_SECONDS=120` waiting on the blocked hop.

**Rule of thumb:** hop *times out* → cloud secgroup / firewall drop; hop *refused fast* → nothing listening. A fast 502 from the Caddy `/docs-ai` route was a red herring (that route just is not wired on UAT).

**Fix (surgical):** `deploy.sh` supports a `TARGET` env var for a targeted apply, so you can create ONLY the one rule without reconciling unrelated drift (the same plan also wanted to rename instances and shrink the backend root disk 50->20):
`TARGET='vngcloud_vserver_secgrouprule.extra["8001-10.100.1.5/32"]' ./deploy.sh uat apply`

## Related
[[leo-customer360 push to main skips the monitoring step; deploy Portainer agents manually]]
[[CD secrets must be wired into cd.yml deploy step env, not just added to GitHub]]

## Related

- [[leo-customer360 push to main skips the monitoring step; deploy Portainer agents manually]]
- [[CD secrets must be wired into cd.yml deploy step env]]
- [[not just added to GitHub]]

%% ai-graph-start %%

**Related notes:**
- [[LEO Customer360 VNG topology co-located services use localhost, cross-box hops need explicit extra_ingress]]
- [[oauth2-proxy cookie_secret must be 162432 bytes; openssl rand -base64 32 (44 chars) crash-loops it]]
- [[Verify uat customer360-api health publicly at beta.leocdp.comc360apihealth]]
- [[docs-vector-search OOMs on ask on a 1vCPU2GB box (Qwen KV cache over RAM+swap)]]
- [[Customer360 UAT api box is a shared 1vCPU-2GB vServer running 5 containers]]

**Relations:**
- docs-search UAT latency — *affects* — docs-vector-search chatbot
- docs-vector-search chatbot — *runs on* — UAT environment
- docs-search UAT latency — *caused by* — unapplied security-group ingress rule
- security-group ingress rule — *is part of* — VNG cloud
- security-group ingress rule — *opens* — docs service port 8001
- security-group ingress rule — *allows access from* — API box
- API box — *has IP* — IP address 10.100.1.5
- security-group ingress rule — *defined in* — deployments/server/overlays/uat.tfvars
- Continuous Deployment (CD) — *does not run* — Terraform
- Terraform — *is applied by* — deploy.sh script
- docs box — *hosts* — docs-vector-search chatbot
- docs box — *serves* — 127.0.0.1:8001/health endpoint
- uvicorn — *binds to* — docs service port 8001
- frontend-admin — *makes request to* — GET /ai/health endpoint
- GET /ai/health endpoint — *proxies through* — frontend-admin proxy
- frontend-admin proxy — *proxies to* — docs box
- API box — *attempts connection to* — IP address 10.100.1.7
- IP address 10.100.1.7 — *on port* — docs service port 8001
- SYN dropped — *indicates* — firewall
- connection refused — *indicates* — nothing listening
- docs-search UAT latency — *is related to timeout* — DOCS_PROXY_TIMEOUT_SECONDS
- Caddy — *has* — /docs-ai route
- deploy.sh script — *supports* — TARGET environment variable
- TARGET environment variable — *targets* — vngcloud_vserver_secgrouprule.extra["8001-10.100.1.5/32"]
- deploy.sh script — *can rename* — instances
- deploy.sh script — *can shrink* — backend root disk
- leo-customer360 — *skips* — monitoring step
- leo-customer360 — *requires manual deployment of* — Portainer agents
- CD secrets — *must be wired into* — cd.yml deploy step env
- CD secrets — *should not be added to* — GitHub

%% ai-graph-end %%