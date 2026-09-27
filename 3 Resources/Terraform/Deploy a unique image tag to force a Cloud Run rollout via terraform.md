---
title: "Deploy a unique image tag to force a Cloud Run rollout via terraform"
created: 2026-09-22
type: lesson
status: seedling
source: "test-agent-v2 deploy de764e1, 2026-09-22"
tags: [terraform, cloud-run, deploy, gotcha, test-agent]
---

# Deploy a unique image tag to force a Cloud Run rollout via terraform

When rolling new code to Cloud Run via terraform, build+deploy a **unique** image tag (a commit SHA works well), not a mutable `:latest`. Terraform diffs the `image` string: re-applying the SAME tag (even if the underlying `:latest` digest moved) shows **no change → no new revision → the new code never rolls out**. A unique tag changes the string, so terraform updates the service in-place and Cloud Run cuts a fresh revision.

Concrete: test-agent-v2 `deployments/test-agent-v2/deploy.sh` resolves the tag from `terraform.tfvars` `image = "...:<tag>"` and uses it for BOTH the Cloud Build and the final `terraform apply -var=image=<tag>`. Deploying new code = bump that tag to the commit SHA (e.g. `de764e1`), then run deploy.sh — all services roll in-place to the new image.

Related gotchas in this repo: `deploy.sh` runs `terraform apply`, which the Claude Code auto-mode classifier BLOCKS — the user must run it themselves (e.g. `! IMAGE=... bash deploy.sh`). `terraform.tfvars` is gitignored, so the tag bump is a working-tree-only change.
