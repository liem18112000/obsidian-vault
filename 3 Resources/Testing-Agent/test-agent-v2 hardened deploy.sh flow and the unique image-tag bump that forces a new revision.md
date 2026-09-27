---
ai_hash: a5e93f7ee71e32e5
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-14
entities: []
source: session 2026-09-14 deploy db7b619
status: seedling
tags:
- testing-agent
- deploy
- terraform
- cloud-run
- cloud-build
- test-agent-v2
title: test-agent-v2 hardened deploy.sh flow and the unique image-tag bump that forces
  a new revision
type: howto
---

# test-agent-v2 hardened deploy.sh flow and the unique image-tag bump that forces a new revision

The working test-agent-v2 deploy lives at `deployments/test-agent-v2/deploy.sh` (self-contained: its own main.tf/services.tf/variables.tf + local terraform.tfstate + gitignored terraform.tfvars). This hardened version FIXES the old v1 gotchas:

- **Async build + poll** — `gcloud builds submit --async` then poll `builds describe` for SUCCESS, so the 'can only stream logs if Viewer/Owner' non-zero exit can no longer fail a build that actually succeeded (was the old streaming-abort bug).
- **Build BEFORE apply** — the image is built first because the Cloud Run containers override the entrypoint (uvicorn/bridge), so the `hello` placeholder can't boot on a true first deploy.
- **Retry the full apply once** — the terraform-generated Cloud SQL db-password secret VERSION can race the service that mounts it; a re-apply clears it.
- Order: init -> apply secret CONTAINERS (targeted) -> add secret VERSIONS from ../../test-agent-v2/.env -> apply artifact repo -> build+push -> full `terraform apply -var=image=$IMAGE`.

**Key requirement:** the tfvars `image` is a UNIQUE `<gitsha>-<label>` tag (e.g. `test-agent-v2:db7b619-critique-gate`), NOT `:latest`. You MUST bump it to the new commit before deploying, else terraform sees the same image string -> `0 changed` -> no new Cloud Run revision -> your code never ships. Verify success: the final apply says `N changed` (not 0), and `gcloud run services describe <svc> --format='value(spec.template.spec.containers[0].image)'` shows the new tag.

Env: project klara-nonprod, region europe-west6, artifact repo kga-v2, image path europe-west6-docker.pkg.dev/klara-nonprod/kga-v2/test-agent-v2. Services: knowledge-gathering-agent-v2 / test-plan-definition-agent-v2 / test-evaluation-agent-v2 / admin-agent-v2 + mcp-gateway-v2. `SKIP_BUILD=1 bash deploy.sh` for infra-only; `IMAGE=<ref> bash deploy.sh` to override the tag; `PLAN=1` for plan-only. After deploy, /mcp-reconnect the client to pick up changed MCP tool signatures.

Related: [[v2 deploy collides with v1 names]], [[MCP instructions load at init - reconnect to refresh]]

## Related

- [[MCP instructions load at init - reconnect to refresh]]

%% ai-graph-start %%

**Related notes:**
- [[Adding a Cloud Run service that shares one image var build first, targeted apply]]
- [[Deploying the test-agent-v2 Cloud Run stack (names, tags, plan)]]
- [[test-agent-v2 Cloud Run services use a -v2 name suffix]]
- [[test-agent-v2 deploy + get_deliverables E2E verification]]
- [[Deploy a unique image tag to force a Cloud Run rollout via terraform]]

%% ai-graph-end %%