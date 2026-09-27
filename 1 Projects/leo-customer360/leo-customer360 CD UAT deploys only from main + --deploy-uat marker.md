---
ai_hash: fd79960923e3b48a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-21
entities:
- leo-customer360
- GitHub Actions CI/CD
- CI
- CD
- ci.yml
- cd.yml
- main branch
- --deploy-uat marker
- GHCR
- v* tag
- feature branch
- workflow_run
- workflow_dispatch
- deploy-all.sh
- uat environment
- api service
- backend service
- ads service
- frontend service
- monitoring module
- load-balancer module
- Docker containers
- VNG vServer VMs
- SSH
- Jaeger
- oauth2 gate
- LB backend
- deploy-monitoring.sh
- deploy.sh
- DEFAULT_SVC
- tests
- images
- commit title
- squash-merge
- PR title
- config/terraform
source: session 2026-08-21
status: seedling
tags:
- leo-customer360
- cicd
- github-actions
- deployment
- gotcha
title: 'leo-customer360 CD: UAT deploys only from main + --deploy-uat marker'
type: observation
---

# leo-customer360 CD: UAT deploys only from main + --deploy-uat marker

How the **leo-customer360** GitHub Actions CI/CD gates deployments (dug out of .github/workflows/ci.yml + cd.yml):

## The gating rules
- **CI (ci.yml)** runs tests on every branch (`branches: ['**']`), but only **builds + pushes images to GHCR** when `github.ref == refs/heads/main` OR a `v*` tag. On a feature branch: tests only, **no new images**.
- **CD (cd.yml)** triggers on `workflow_run` after CI succeeds, and deploys **uat only when the ref is `main` AND the head commit title contains the literal marker `--deploy-uat`** (opt-in). It also supports manual `workflow_dispatch` (choose env/tag/services) and `vX.Y.Z` tags. It then runs `bash deploy-all.sh <env> --only <services> -y`, pulling `:latest`.

## Consequences (the gotcha)
- Committing `--deploy-uat` on a **feature branch does nothing** — no images built, no deploy. The marked commit must be on **main**.
- Because CD reads the marker from **main's head commit title**, a plain `Merge pull request #N` merge commit will **NOT** carry it -> deploy skipped. To trigger via PR, **squash-merge and keep `--deploy-uat` in the squash title** (set the PR title to include it so the default squash title carries it), or push the marked commit directly to main.
- Team convention: deploy commits look like `chore: redeploy all vServer services (run N) --deploy-uat`.

## Service SCOPE gotcha (bit me on the Jaeger SSO deploy)
On a `--deploy-uat` push the CD job hardcodes **`services=api,backend,ads,frontend`** (the `DEFAULT_SVC` in cd.yml) and runs `deploy-all.sh uat --only "$SERVICES"`. So a `--deploy-uat` merge NEVER deploys the **`monitoring`** or **`load-balancer`** modules — changes to those (e.g. a new Jaeger container, oauth2 gate, or an LB backend) land on `main` and CD goes green, but are **not applied to the box**. Deploy them manually: `(cd deployments/monitoring && ./deploy-monitoring.sh uat)` and `(cd deployments/load_balancer && ./deploy.sh uat apply)`; or use the manual `workflow_dispatch` on cd.yml with an explicit `services=` input. (Only api/backend/ads/frontend also get fresh GHCR images; the other modules are config/terraform, not images.)

## Related
[[leo-customer360 deploys as Docker containers on VNG vServer VMs over SSH]]

## Service SCOPE gotcha (bit me on the Jaeger SSO deploy)
On a `--deploy-uat` push the CD job hardcodes **`services=api,backend,ads,frontend`** (the `DEFAULT_SVC` in cd.yml) and runs `deploy-all.sh uat --only "$SERVICES"`. So a `--deploy-uat` merge NEVER deploys the **`monitoring`** or **`load-balancer`** modules — changes to those (e.g. a new Jaeger container, oauth2 gate, or an LB backend) land on `main` and CD goes green, but are **not applied to the box**. Deploy them manually: `(cd deployments/monitoring && ./deploy-monitoring.sh uat)` and `(cd deployments/load_balancer && ./deploy.sh uat apply)`; or use the manual `workflow_dispatch` on cd.yml with an explicit `services=` input. (Only api/backend/ads/frontend also get fresh GHCR images; the other modules are config/terraform, not images.)

## Related

- [[leo-customer360 deploys as Docker containers on VNG vServer VMs over SSH]]

%% ai-graph-start %%

**Related notes:**
- [[Chain a CD workflow after CI with workflow_run, gating on conclusion and ref]]
- [[Deploy an unmerged feature branch to leo-customer360 UAT with BUILD_LOCAL=1]]
- [[leo-customer360 CD builds images on the VM instead of pulling from GHCR (CICD gap)]]
- [[Feature-branch images are never pushed to GHCR in leo-customer360 CI]]
- [[leo-customer360 VNG deploy builds app images on the VM from a tarred local checkout, not from a registry]]

**Relations:**
- leo-customer360 — *uses* — GitHub Actions CI/CD
- GitHub Actions CI/CD — *includes* — CI
- GitHub Actions CI/CD — *includes* — CD
- CI — *defined_by* — ci.yml
- CD — *defined_by* — cd.yml
- CI — *runs_on* — every branch
- CI — *runs* — tests
- CI — *builds_and_pushes_to* — GHCR
- CI — *builds_and_pushes_when_ref_is* — main branch
- CI — *builds_and_pushes_when_ref_is* — v* tag
- CI — *only_runs_tests_on* — feature branch
- CD — *triggers_on* — workflow_run
- CD — *triggers_on* — workflow_dispatch
- CD — *deploys_to* — uat environment
- CD — *deploys_to_uat_when_ref_is* — main branch
- CD — *deploys_to_uat_when_commit_title_contains* — --deploy-uat marker
- CD — *supports* — manual workflow_dispatch
- CD — *supports* — vX.Y.Z tags
- CD — *executes* — deploy-all.sh
- deploy-all.sh — *deploys_to* — uat environment
- deploy-all.sh — *uses_flag* — --only <services>
- deploy-all.sh — *pulls* — :latest
- --deploy-uat marker — *is_required_on* — main branch
- --deploy-uat marker — *must_be_in* — squash title
- CD job — *hardcodes_services* — api service
- CD job — *hardcodes_services* — backend service
- CD job — *hardcodes_services* — ads service
- CD job — *hardcodes_services* — frontend service
- DEFAULT_SVC — *includes* — api service
- DEFAULT_SVC — *includes* — backend service
- DEFAULT_SVC — *includes* — ads service
- DEFAULT_SVC — *includes* — frontend service
- --deploy-uat marker — *does_not_deploy* — monitoring module
- --deploy-uat marker — *does_not_deploy* — load-balancer module
- monitoring module — *deployed_manually_by* — deploy-monitoring.sh
- load-balancer module — *deployed_manually_by* — deploy.sh
- manual workflow_dispatch — *can_deploy* — monitoring module
- manual workflow_dispatch — *can_deploy* — load-balancer module
- api service — *gets* — fresh GHCR images
- backend service — *gets* — fresh GHCR images
- ads service — *gets* — fresh GHCR images
- frontend service — *gets* — fresh GHCR images
- monitoring module — *is_type* — config/terraform
- load-balancer module — *is_type* — config/terraform
- leo-customer360 — *deploys_as* — Docker containers
- Docker containers — *run_on* — VNG vServer VMs
- VNG vServer VMs — *accessed_via* — SSH
- Jaeger — *is_a* — new container
- oauth2 gate — *is_a* — component
- LB backend — *is_a* — component
- monitoring module — *located_in* — deployments/monitoring
- load-balancer module — *located_in* — deployments/load_balancer
- commit title — *contains* — --deploy-uat marker
- squash-merge — *keeps* — --deploy-uat marker
- PR title — *can_set* — squash title

%% ai-graph-end %%