---
ai_hash: 1ff761bdde90ae4f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-28
entities:
- Cloud Run service
- image var
- test_plan_definition agent (tpd)
- knowledge-gathering Terraform stack (kga/bridge)
- Terraform
- terraform apply
- terraform apply -target
- terraform plan
- New image
- Current image
- Pinned digest
- Deployed image (klara-nonprod/kga/kga@sha256:...)
- tfvars image (klara-repo/.../test-agent:latest)
- deploy.sh
- Secret
- Secret container
- gcloud secrets versions add
- Smoke-test
- agent /livez 200 check
- card lists skills check
- bridge /mcp bare returns 401 check
- Escape shell ${VAR} as $${VAR} in a Terraform Cloud Run command list
source: session 2026-08-28, deploy test_plan_definition
status: seedling
tags:
- test-agent
- terraform
- cloud-run
- deploy
- gotcha
title: 'Adding a Cloud Run service that shares one image var: build first, targeted
  apply'
type: lesson
---

# Adding a Cloud Run service that shares one image var: build first, targeted apply

Deploying the test_plan_definition agent + bridge onto the existing knowledge-gathering Terraform stack hit two traps worth remembering:

1. **One `image` var drives ALL Cloud Run services.** The new tpd services need a freshly built image (containing the new package); the live kga/bridge must stay on their current image. You cannot satisfy both in one full `terraform apply`. Fix: build+push the new image to the tag first, then `terraform apply -target=<each tpd resource>` so kga/bridge are excluded and keep their pinned digest. `terraform plan` first — confirm "0 to destroy" and that the only in-place changes are ones you intend.

2. **Pre-existing image drift.** `terraform plan` wanted to update kga+bridge in-place because their DEPLOYED image (`klara-nonprod/kga/kga@sha256:…`, a pinned digest) differed from tfvars `image` (`klara-repo/.../test-agent:latest`). A naive apply would have flipped the live services to a different registry/image — an unintended redeploy. The targeted apply avoided touching them; the drift remains and should be converged deliberately later (deploy.sh's "preserve deployed image from state" logic is what normally prevents this).

3. **Secret-version ordering.** A Cloud Run service mounting `secret:latest` fails to start if the secret has no version yet. Since the service + its secret are created in the same config, do a targeted apply of JUST the secret container first, `gcloud secrets versions add` a value, THEN apply the service.

Net sequence: build image -> targeted-apply the secret -> add secret version -> targeted-apply the rest of the new resources -> smoke-test (agent /livez 200, card lists skills, bridge /mcp bare returns 401). Result here: 7 added, 0 changed, 0 destroyed — kga untouched.

## Related

- [[Escape shell ${VAR} as $${VAR} in a Terraform Cloud Run command list]]

%% ai-graph-start %%

**Related notes:**
- [[test-agent-v2 hardened deploy.sh flow and the unique image-tag bump that forces a new revision]]
- [[Deploying the test-agent-v2 Cloud Run stack (names, tags, plan)]]
- [[test-agent-v2 Cloud Run services use a -v2 name suffix]]
- [[Deploy a unique image tag to force a Cloud Run rollout via terraform]]
- [[Cloud Run won't redeploy on a latest digest change — apply by immutable digest]]

**Relations:**
- Cloud Run service — *shares* — image var
- test_plan_definition agent (tpd) — *deployed onto* — knowledge-gathering Terraform stack (kga/bridge)
- image var — *drives* — Cloud Run service
- test_plan_definition agent (tpd) — *requires* — New image
- knowledge-gathering Terraform stack (kga/bridge) — *requires* — Current image
- terraform apply — *cannot satisfy simultaneously* — New image for tpd and Current image for kga/bridge
- New image — *built and pushed to tag before* — terraform apply -target
- terraform apply -target — *excludes* — knowledge-gathering Terraform stack (kga/bridge)
- knowledge-gathering Terraform stack (kga/bridge) — *retains* — Pinned digest
- terraform plan — *confirms* — 0 to destroy
- terraform plan — *confirms* — intended in-place changes
- terraform plan — *indicated update for* — knowledge-gathering Terraform stack (kga/bridge)
- Deployed image (klara-nonprod/kga/kga@sha256:...) — *differed from* — tfvars image (klara-repo/.../test-agent:latest)
- Naive terraform apply — *would change* — live services to different registry/image
- terraform apply -target — *prevented changes to* — knowledge-gathering Terraform stack (kga/bridge)
- Image drift — *persists* — null
- deploy.sh — *normally prevents* — Image drift
- Cloud Run service — *mounting Secret fails if* — Secret has no version
- Cloud Run service — *and Secret created in* — same config
- Secret container — *targeted applied* — first
- gcloud secrets versions add — *adds value to* — Secret
- Cloud Run service — *applied after* — Secret has version
- Build image — *precedes* — Targeted apply of secret
- Targeted apply of secret — *precedes* — Add secret version
- Add secret version — *precedes* — Targeted apply of new resources
- Targeted apply of new resources — *precedes* — Smoke-test
- Smoke-test — *includes* — agent /livez 200 check
- Smoke-test — *includes* — card lists skills check
- Smoke-test — *includes* — bridge /mcp bare returns 401 check
- Deployment result — *is* — 7 added, 0 changed, 0 destroyed
- knowledge-gathering Terraform stack (kga/bridge) — *was* — untouched
- This note — *is related to* — Escape shell ${VAR} as $${VAR} in a Terraform Cloud Run command list

%% ai-graph-end %%