---
ai_hash: c3d7d26ecfe71de4
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-06
entities:
- LEO Customer360 VNG topology
- vServer deployment
- VMs
- Default security group
- VPC
- subnet
- postgres VPC
- Co-located services
- localhost
- 127.0.0.1
- Caddy
- frontend-admin
- customer360-api
- Cross-box hops
- extra_ingress
- deployments/server/overlays/<env>.tfvars
- api box
- tracking box
- docs box
- api
- keycloak
- redis
- ads
- docs-vector-search RAG
- Terraform
- servers output
- internal_interfaces[].fixed_ip
- PRIVATE ip
- floating_ip
- PUBLIC ip
- CD
- ./deploy.sh <env> apply
- Static vs server-backed host decides proxy-vs-direct-CORS for browser widgets
- Register one FastAPI handler under multiple path prefixes with add_api_route
- 10.100.1.5
- 10.100.1.8
- '8890'
- '8008'
- '8010'
- '8000'
source: session 2026-09-06 docs-chatbot UAT wiring
status: seedling
tags:
- leo-customer360
- vngcloud
- networking
- terraform
- security-group
- deployment
title: 'LEO Customer360 VNG topology: co-located services use localhost, cross-box
  hops need explicit extra_ingress'
type: reference
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

%% ai-graph-start %%

**Related notes:**
- [[leo-customer360 deploys as Docker containers on VNG vServer VMs over SSH]]
- [[Co-located --network host box hides cross-box firewall hops]]
- [[docs-search UAT latency root cause unapplied 8001 secgroup ingress (api to docs box)]]
- [[Customer360 UAT api box is a shared 1vCPU-2GB vServer running 5 containers]]
- [[Monitoring SSO-gate adding a dashboard needs a Keycloak redirect_uri re-sync]]

**Relations:**
- LEO Customer360 VNG topology — *describes* — vServer deployment
- LEO Customer360 VNG topology — *has characteristic* — co-located services use localhost
- LEO Customer360 VNG topology — *has characteristic* — cross-box hops need explicit extra_ingress
- vServer deployment — *contains* — VMs
- VMs — *share* — Default security group
- VMs — *sit in* — VPC
- VMs — *sit in* — subnet
- VPC — *is* — postgres VPC
- Co-located services — *communicate via* — localhost
- localhost — *is* — 127.0.0.1
- Caddy — *calls* — frontend-admin
- frontend-admin — *listens on port* — 8890
- Caddy — *calls* — customer360-api
- customer360-api — *listens on port* — 8008
- Cross-box hops — *require* — extra_ingress
- extra_ingress — *configured in* — deployments/server/overlays/<env>.tfvars
- extra_ingress — *applies to* — Default security group
- api box — *calls* — tracking box
- tracking box — *listens on port* — 8010
- api box — *calls* — docs box
- docs box — *listens on port* — 8000
- api box — *runs* — api
- api box — *runs* — keycloak
- api box — *runs* — redis
- api box — *runs* — frontend-admin
- api box — *runs* — ads
- api box — *runs* — Caddy
- tracking box — *is a* — VM
- docs box — *is a* — VM
- docs box — *runs* — docs-vector-search RAG
- docs-vector-search RAG — *listens on port* — 8000
- Terraform — *provides* — servers output
- servers output — *contains* — internal_interfaces[].fixed_ip
- servers output — *contains* — floating_ip
- internal_interfaces[].fixed_ip — *is a* — PRIVATE ip
- floating_ip — *is a* — PUBLIC ip
- CD — *does not apply* — extra_ingress
- ./deploy.sh <env> apply — *applies* — extra_ingress
- extra_ingress — *relates to* — Static vs server-backed host decides proxy-vs-direct-CORS for browser widgets
- extra_ingress — *relates to* — Register one FastAPI handler under multiple path prefixes with add_api_route
- api box — *has IP* — 10.100.1.5
- tracking box — *has IP* — 10.100.1.8

%% ai-graph-end %%