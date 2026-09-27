---
ai_hash: 40aa194dda965d55
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
- storage
- postgres
- load_balancer
- vServer provisioning
- managed data services
- deploy-all.sh
- tf_step
- app deploy scripts
- terraform init
- terraform output
- Terraform apply
- vServer IPs
- DB host
- redis host
- Chain a CD workflow after CI with workflow_run, gating on conclusion and ref
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
- leo-customer360 CD — *never_runs* — Terraform apply
- cd.yml — *is_a* — continuous-delivery pipeline
- cd.yml — *deploys* — customer360-api
- cd.yml — *deploys* — backend-system
- cd.yml — *deploys* — ads-server
- cd.yml — *deploys* — frontend-admin
- cd.yml — *pulls_image_from* — GHCR
- cd.yml — *uses* — docker run
- cd.yml — *uses* — SSH
- deploy.sh — *performs* — Terraform apply
- storage — *is_a* — managed data services
- postgres — *is_a* — managed data services
- load_balancer — *is_a* — managed data services
- vServer provisioning — *is_a* — managed data services
- storage — *is_also_known_as* — vStorage
- postgres — *is_also_known_as* — vDB
- load_balancer — *is_also_known_as* — vLB
- managed data services — *are* — long-lived
- managed data services — *are* — expensive
- managed data services — *are* — stateful
- deploy-all.sh — *enforces_policy_for* — leo-customer360 CD
- deploy-all.sh — *uses_flag* — --only
- customer360-api — *mapped_to_script* — app deploy scripts
- backend-system — *mapped_to_script* — app deploy scripts
- ads-server — *mapped_to_script* — app deploy scripts
- frontend-admin — *mapped_to_script* — app deploy scripts
- storage — *mapped_to_step* — tf_step
- postgres — *mapped_to_step* — tf_step
- vServer provisioning — *mapped_to_step* — tf_step
- load_balancer — *mapped_to_step* — tf_step
- tf_step — *executes* — deploy.sh
- leo-customer360 CD — *runs* — terraform init
- leo-customer360 CD — *runs* — terraform output
- terraform init — *has_property* — READ-ONLY
- terraform output — *has_property* — READ-ONLY
- terraform output — *resolves* — vServer IPs
- terraform output — *resolves* — DB host
- terraform output — *resolves* — redis host
- app deploy scripts — *requires* — vServer IPs
- app deploy scripts — *requires* — DB host
- app deploy scripts — *requires* — redis host
- document — *references* — Chain a CD workflow after CI with workflow_run, gating on conclusion and ref

%% ai-graph-end %%