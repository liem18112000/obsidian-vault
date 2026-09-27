---
title: "LEO Customer360 VNG topology: co-located services use localhost, cross-box hops need explicit extra_ingress"
created: 2026-09-06
type: reference
status: seedling
source: "session 2026-09-06 docs-chatbot UAT wiring"
tags: [leo-customer360, vngcloud, networking, terraform, security-group, deployment]
---

# LEO Customer360 VNG topology: co-located services use localhost, cross-box hops need explicit extra_ingress

In the LEO Customer 360 VNG vServer deployment, all VMs share ONE "Default" security group and sit in the same VPC/subnet (the postgres VPC). This creates two rules of thumb for service-to-service calls:

- **Co-located services** (same box) reach each other on **127.0.0.1** — no firewall change. E.g. Caddy -> frontend-admin :8890, Caddy -> customer360-api :8008.
- **Cross-box** hops must be opened **explicitly** by adding `{ port, cidr = "<source-box-ip>/32" }` to `extra_ingress` in `deployments/server/overlays/<env>.tfvars` (applied to the shared Default secgroup). Because only the destination box listens on that port, opening it on the shared secgroup is safe. E.g. api box (10.100.1.5) -> tracking box :8010; and (added for the docs chatbot) api box -> docs box :8000.

Server-topology facts (UAT): api box = 10.100.1.5 (runs api + keycloak + redis + frontend-admin + ads + Caddy); tracking box = 10.100.1.8; docs box (docs-vector-search RAG :8000) is its own VM. In the terraform `servers` output, `internal_interfaces[].fixed_ip` is the PRIVATE ip and `floating_ip` is the PUBLIC ip.

Gotcha: `extra_ingress` is infra Terraform — CD never applies it. After editing the overlay you must run `./deploy.sh <env> apply` out-of-band, or the new cross-box hop stays blocked (connection refused). Relates to [[Static vs server-backed host decides proxy-vs-direct-CORS for browser widgets]] and [[Register one FastAPI handler under multiple path prefixes with add_api_route]].

## Related

- [[Static vs server-backed host decides proxy-vs-direct-CORS for browser widgets]]
- [[Register one FastAPI handler under multiple path prefixes with add_api_route]]
