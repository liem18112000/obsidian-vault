---
ai_hash: 84c950124aa300ff
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-20
entities:
- leo-customer360 CD
- app containers
- vServers
- vDB
- vLB
- vStorage
- Terraform
- cd.yml
- customer360-api
- backend-system
- ads-server
- frontend-admin
- GHCR
- docker run
- SSH
- deploy.sh
- managed data services
- storage
- postgres
- load_balancer
- object storage
- managed PostgreSQL
- NLB
- server
- deploy-all.sh
- tf_step
- deploy-*.sh
- terraform init
- terraform output
- redis host
- cache
- vServer IPs
- DB host
- Chain a CD workflow after CI with workflow_run
- gating on conclusion and ref
- api
- backend
- ads
- frontend
source: session 2026-08-20, cd.yml
status: seedling
tags:
- leo-customer360
- cd
- terraform
- safety
- deployment
title: leo-customer360 CD deploys app containers to vServers only, never the vDB/vLB/vStorage
  Terraform
type: argument
---

# leo-customer360 CD deploys app containers to vServers only, never the vDB/vLB/vStorage Terraform

Decision: the `cd.yml` continuous-delivery pipeline deploys **only the vServer app containers** (customer360-api, backend-system, ads-server, frontend-admin) — pull image from GHCR + `docker run` over SSH. It must **never** run the infrastructure Terraform (`deploy.sh`) for the managed data services: `storage` (vStorage / object storage), `postgres` (vDB / managed PostgreSQL), `load_balancer` (vLB / NLB), or `server` (vServer provisioning itself).

**Why:** those are long-lived, expensive, stateful managed resources; a `terraform apply` on every deploy risks destructive drift/recreation. Provisioning is a separate, deliberate, human-run step.

**How it is enforced:** `deploy-all.sh <env> --only api,backend,ads,frontend -y`. In `deploy-all.sh`, `storage|postgres|server|load-balancer` map to `tf_step … deploy.sh` (Terraform apply) while `api|backend|ads|frontend` map to the SSH `deploy-*.sh` scripts — and `--only` restricts execution to exactly the named steps, so the tf_step ones are never reached.

**Read vs touch:** CD still runs `terraform init` + `terraform output` (READ-ONLY) on `server`/`postgres`/`cache` to resolve vServer IPs, DB host, and redis host that the app scripts need — this reads state but never `apply`s, so it does not mutate the vDB/vLB/vStorage. Context: [[Chain a CD workflow after CI with workflow_run, gating on conclusion and ref]].

## Related

- [[Chain a CD workflow after CI with workflow_run, gating on conclusion and ref]]

%% ai-graph-start %%

**Related notes:**
- [[CI-driven CD cannot resolve local gitignored Terraform state — needs remote backend or IPs via secrets]]
- [[Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoringLB]]
- [[leo-customer360 CD builds images on the VM instead of pulling from GHCR (CICD gap)]]
- [[leo-customer360 CD runs after CI via workflow_run and resumable deploy-all.sh]]
- [[leo-customer360 CD UAT deploys only from main + --deploy-uat marker]]

**Relations:**
- leo-customer360 CD — *deploys* — app containers
- app containers — *deployed_to* — vServers
- leo-customer360 CD — *never_deploys* — vDB
- leo-customer360 CD — *never_deploys* — vLB
- leo-customer360 CD — *never_deploys* — vStorage
- vDB — *managed_by* — Terraform
- vLB — *managed_by* — Terraform
- vStorage — *managed_by* — Terraform
- cd.yml — *is_a* — continuous-delivery pipeline
- cd.yml — *deploys* — customer360-api
- cd.yml — *deploys* — backend-system
- cd.yml — *deploys* — ads-server
- cd.yml — *deploys* — frontend-admin
- customer360-api — *pulls_image_from* — GHCR
- backend-system — *pulls_image_from* — GHCR
- ads-server — *pulls_image_from* — GHCR
- frontend-admin — *pulls_image_from* — GHCR
- customer360-api — *deployed_via* — docker run
- backend-system — *deployed_via* — docker run
- ads-server — *deployed_via* — docker run
- frontend-admin — *deployed_via* — docker run
- docker run — *executed_over* — SSH
- cd.yml — *must_never_run* — deploy.sh
- deploy.sh — *manages* — managed data services
- managed data services — *includes* — storage
- managed data services — *includes* — postgres
- managed data services — *includes* — load_balancer
- storage — *is_a* — vStorage
- storage — *is_a* — object storage
- postgres — *is_a* — vDB
- postgres — *is_a* — managed PostgreSQL
- load_balancer — *is_a* — vLB
- load_balancer — *is_a* — NLB
- server — *is_for* — vServer provisioning
- deploy-all.sh — *enforces_policy* — leo-customer360 CD
- deploy-all.sh — *restricts_execution_to* — api
- deploy-all.sh — *restricts_execution_to* — backend
- deploy-all.sh — *restricts_execution_to* — ads
- deploy-all.sh — *restricts_execution_to* — frontend
- storage — *maps_to* — tf_step
- postgres — *maps_to* — tf_step
- server — *maps_to* — tf_step
- load_balancer — *maps_to* — tf_step
- tf_step — *runs* — deploy.sh
- api — *maps_to* — deploy-*.sh
- backend — *maps_to* — deploy-*.sh
- ads — *maps_to* — deploy-*.sh
- frontend — *maps_to* — deploy-*.sh
- leo-customer360 CD — *runs* — terraform init
- leo-customer360 CD — *runs* — terraform output
- terraform init — *is* — READ-ONLY
- terraform output — *is* — READ-ONLY
- terraform init — *on* — server
- terraform init — *on* — postgres
- terraform init — *on* — cache
- terraform output — *on* — server
- terraform output — *on* — postgres
- terraform output — *on* — cache
- terraform output — *resolves* — vServer IPs
- terraform output — *resolves* — DB host
- terraform output — *resolves* — redis host
- deploy-*.sh — *needs* — vServer IPs
- deploy-*.sh — *needs* — DB host
- deploy-*.sh — *needs* — redis host
- Chain a CD workflow after CI with workflow_run — *is_related_to* — leo-customer360 CD
- gating on conclusion and ref — *is_related_to* — leo-customer360 CD
- Chain a CD workflow after CI with workflow_run — *provides_context_for* — gating on conclusion and ref

%% ai-graph-end %%