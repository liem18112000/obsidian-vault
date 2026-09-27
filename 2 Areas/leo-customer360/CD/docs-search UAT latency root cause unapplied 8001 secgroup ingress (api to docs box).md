---
title: "docs-search UAT latency root cause: unapplied 8001 secgroup ingress (api to docs box)"
created: 2026-09-13
type: lesson
status: seedling
source: "session 2026-09-13"
tags: [leo-customer360, networking, terraform, vngcloud, troubleshooting, docs-search]
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
