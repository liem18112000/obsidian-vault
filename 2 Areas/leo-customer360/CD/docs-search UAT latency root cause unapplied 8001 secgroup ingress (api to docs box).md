---
ai_hash: 7ee6e9a1051ace18
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-13
entities:
- docs-search UAT latency
- unapplied 8001 secgroup ingress
- api box
- docs box
- docs-vector-search chatbot
- UAT
- VNG cloud security-group ingress rule
- docs :8001
- 10.100.1.5
- deployments/server/overlays/uat.tfvars
- CD
- infra Terraform
- tfvars secgroup edits
- ./deploy.sh uat apply
- Service on docs box
- curl 127.0.0.1:8001/health
- uvicorn
- 0.0.0.0:8001
- Frontend
- GET https://<host>/health
- frontend-admin
- GET /ai/health
- frontend-admin proxy
- 10.100.1.7
- curl -m5 http://10.100.1.7:8001/health
- SYN dropped
- firewall
- connection refused
- 120s hang
- DOCS_PROXY_TIMEOUT_SECONDS
- cloud secgroup
- firewall drop
- hop times out
- hop refused fast
- nothing listening
- Caddy
- /docs-ai route
- deploy.sh
- TARGET env var
- targeted apply
- unrelated drift
- rename instances
- shrink backend root disk
- vngcloud_vserver_secgrouprule.extra["8001-10.100.1.5/32"]
- leo-customer360 push to main skips the monitoring step; deploy Portainer agents
  manually
- CD secrets must be wired into cd.yml deploy step env, not just added to GitHub
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
- [[CD secrets must be wired into cd.yml deploy step env, not just added to GitHub]]

%% ai-graph-start %%

**Related notes:**
- [[LEO Customer360 VNG topology co-located services use localhost, cross-box hops need explicit extra_ingress]]
- [[docs-vector-search OOMs on ask on a 1vCPU2GB box (Qwen KV cache over RAM+swap)]]
- [[oauth2-proxy cookie_secret must be 162432 bytes; openssl rand -base64 32 (44 chars) crash-loops it]]
- [[Verify uat customer360-api health publicly at beta.leocdp.comc360apihealth]]
- [[Customer360 UAT api box is a shared 1vCPU-2GB vServer running 5 containers]]

**Relations:**
- unapplied 8001 secgroup ingress — *is_root_cause_of* — docs-search UAT latency
- unapplied 8001 secgroup ingress — *from* — api box
- unapplied 8001 secgroup ingress — *to* — docs box
- docs-vector-search chatbot — *experienced* — 120s hang
- VNG cloud security-group ingress rule — *opens_port* — docs :8001
- VNG cloud security-group ingress rule — *from_source* — api box
- api box — *has_IP* — 10.100.1.5
- VNG cloud security-group ingress rule — *existed_in* — deployments/server/overlays/uat.tfvars
- VNG cloud security-group ingress rule — *was_not_applied_by* — CD
- CD — *does_not_run* — infra Terraform
- tfvars secgroup edits — *require_manual_application_via* — ./deploy.sh uat apply
- Service on docs box — *responds_to* — curl 127.0.0.1:8001/health
- uvicorn — *bound_to* — 0.0.0.0:8001
- Frontend — *responds_to* — GET https://<host>/health
- GET https://<host>/health — *is_owned_by* — frontend-admin
- GET /ai/health — *is_proxied_by* — frontend-admin proxy
- frontend-admin proxy — *targets* — docs box
- GET /ai/health — *resulted_in* — 120s hang
- api box — *executed* — curl -m5 http://10.100.1.7:8001/health
- 10.100.1.7 — *is_IP_of* — docs box
- curl -m5 http://10.100.1.7:8001/health — *resulted_in* — hop times out
- SYN dropped — *indicates* — firewall
- SYN dropped — *does_not_indicate* — connection refused
- 120s hang — *is_due_to* — DOCS_PROXY_TIMEOUT_SECONDS
- DOCS_PROXY_TIMEOUT_SECONDS — *is_set_to* — 120
- frontend-admin — *uses* — DOCS_PROXY_TIMEOUT_SECONDS
- hop times out — *indicates* — cloud secgroup
- hop times out — *indicates* — firewall drop
- hop refused fast — *indicates* — nothing listening
- Caddy — *has* — /docs-ai route
- /docs-ai route — *is_not_wired_on* — UAT
- deploy.sh — *supports* — TARGET env var
- TARGET env var — *enables* — targeted apply
- targeted apply — *avoids_reconciling* — unrelated drift
- unrelated drift — *includes* — rename instances
- unrelated drift — *includes* — shrink backend root disk
- vngcloud_vserver_secgrouprule.extra["8001-10.100.1.5/32"] — *is_value_for* — TARGET env var
- ./deploy.sh uat apply — *uses* — TARGET env var
- docs-search UAT latency — *is_related_to* — leo-customer360 push to main skips the monitoring step; deploy Portainer agents manually
- docs-search UAT latency — *is_related_to* — CD secrets must be wired into cd.yml deploy step env, not just added to GitHub

%% ai-graph-end %%