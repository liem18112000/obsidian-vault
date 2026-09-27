---
ai_hash: d19ceb4c8a17f010
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-28
entities: []
source: session 2026-08-27
status: seedling
tags:
- cloud-run
- terraform
- deployment
- gotcha
- docker
title: Cloud Run :latest does not roll a new revision on terraform apply — deploy
  by digest
type: lesson
---

# Cloud Run :latest does not roll a new revision on terraform apply — deploy by digest

Deploying a Cloud Run service by the mutable tag `:latest` and then running `terraform apply` (or `gcloud run deploy` with the same ref) does NOT roll a new revision when the *reference string* is unchanged: Terraform diffs the literal `image = "...:latest"`, sees no change, and skips the container — so a freshly pushed `:latest` keeps serving the OLD code. (An unrelated env/annotation change masks this, because it forces a new revision that also pulls the new image — which is why "add an env var" deploys seem to work but "code-only" deploys silently dont.)

Fix: deploy by **immutable digest** so the ref actually changes:
```bash
D=$(gcloud artifacts docker images describe "$IMG:latest" \
      --format="value(image_summary.fully_qualified_digest)")   # or images list --format="value(version)"
terraform apply -var="image=$D"     # $IMG@sha256:...
```
Alternatives: a unique tag per build (git SHA), or bump a template annotation to force a revision. `image_summary.digest` alone returned empty in one gcloud version — `fully_qualified_digest` (repo@sha256:...) or `images list --include-tags --format="value(version)"` is more reliable.

See [[Cloud Run one-port limit forces co-located HTTP servers into separate services]].

## Related

- [[Cloud Run one-port limit forces co-located HTTP servers into separate services]]

%% ai-graph-start %%

**Related notes:**
- [[Cloud Run won't redeploy on a latest digest change — apply by immutable digest]]
- [[Deploy a unique image tag to force a Cloud Run rollout via terraform]]
- [[Single-to-multi container Cloud Run update fails in-place; use terraform -replace]]
- [[Adding a Cloud Run service that shares one image var build first, targeted apply]]
- [[Cloud Build $COMMIT_SHA is the full 40-char git SHA, not the short one]]

%% ai-graph-end %%