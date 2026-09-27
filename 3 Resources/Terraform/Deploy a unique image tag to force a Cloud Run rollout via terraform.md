---
ai_hash: 0fd4f7ddb51723d3
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities: []
source: test-agent-v2 deploy de764e1, 2026-09-22
status: seedling
tags:
- terraform
- cloud-run
- deploy
- gotcha
- test-agent
title: Deploy a unique image tag to force a Cloud Run rollout via terraform
type: lesson
---

# Deploy a unique image tag to force a Cloud Run rollout via terraform

When rolling new code to Cloud Run via terraform, build+deploy a **unique** image tag (a commit SHA works well), not a mutable `:latest`. Terraform diffs the `image` string: re-applying the SAME tag (even if the underlying `:latest` digest moved) shows **no change → no new revision → the new code never rolls out**. A unique tag changes the string, so terraform updates the service in-place and Cloud Run cuts a fresh revision.

Concrete: test-agent-v2 `deployments/test-agent-v2/deploy.sh` resolves the tag from `terraform.tfvars` `image = "...:<tag>"` and uses it for BOTH the Cloud Build and the final `terraform apply -var=image=<tag>`. Deploying new code = bump that tag to the commit SHA (e.g. `de764e1`), then run deploy.sh — all services roll in-place to the new image.

Related gotchas in this repo: `deploy.sh` runs `terraform apply`, which the Claude Code auto-mode classifier BLOCKS — the user must run it themselves (e.g. `! IMAGE=... bash deploy.sh`). `terraform.tfvars` is gitignored, so the tag bump is a working-tree-only change.

%% ai-graph-start %%

**Related notes:**
- [[Cloud Run latest does not roll a new revision on terraform apply — deploy by digest]]
- [[Cloud Run won't redeploy on a latest digest change — apply by immutable digest]]
- [[test-agent-v2 hardened deploy.sh flow and the unique image-tag bump that forces a new revision]]
- [[test-agent-v2 image built only from pyproject + src + main.py]]
- [[Adding a Cloud Run service that shares one image var build first, targeted apply]]

%% ai-graph-end %%