---
ai_hash: 9cf4ac218e760c2b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-28
entities: []
source: session 2026-08-28 (KGA refactor deploy)
status: seedling
tags:
- gcp
- cloud-run
- terraform
- artifact-registry
- deployment
- gotcha
title: Cloud Run won't redeploy on a :latest digest change — apply by immutable digest
type: lesson
---

# Cloud Run won't redeploy on a :latest digest change — apply by immutable digest

Cloud Run tracks the exact image reference string on a service revision. If a service already points at a **mutable tag** like `...kga:latest` and you rebuild that tag to new content, neither `gcloud`/Terraform nor Cloud Run creates a new revision — because the config string (`kga:latest`) is byte-identical to what is already deployed. Terraform reports "No changes"; the service keeps serving the OLD image behind the tag.

To force the new image out, deploy by the **immutable digest** instead of the tag:
```
DIGEST=$(gcloud artifacts docker images list REPO/IMG --include-tags \
  --filter="tags=latest" --format="value(version)")   # sha256:...
terraform apply -var="image=REPO/IMG@$DIGEST"          # differs from :latest -> new revision
```
The digest string differs from the stored `:latest`, so Terraform/Cloud Run sees a real change and rolls a new revision. (Note: `gcloud artifacts docker images describe ...:latest` can fail with a `containeranalysis.occurrences.list` permission error — that is the vuln-scan metadata, not the digest; use `images list --format="value(version)"` to read the digest without that permission.)

Surfaced deploying a rebuilt `:latest` to two Cloud Run services that were already on `:latest` — a plain re-apply was a no-op until I switched to the digest.

%% ai-graph-start %%

**Related notes:**
- [[Cloud Run latest does not roll a new revision on terraform apply — deploy by digest]]
- [[Deploy a unique image tag to force a Cloud Run rollout via terraform]]
- [[Adding a Cloud Run service that shares one image var build first, targeted apply]]
- [[test-agent-v2 hardened deploy.sh flow and the unique image-tag bump that forces a new revision]]
- [[Cloud Build $COMMIT_SHA is the full 40-char git SHA, not the short one]]

%% ai-graph-end %%