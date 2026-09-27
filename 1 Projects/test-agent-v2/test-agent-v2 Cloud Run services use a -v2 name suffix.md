---
ai_hash: 11a5a1e2046f11d9
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-13
entities:
- test-agent-v2
- Cloud Run services
- -v2
- knowledge-gathering-agent-v2
- test-plan-definition-agent-v2
- test-evaluation-agent-v2
- mcp-gateway-v2
- terraform
- name_prefix
- gcloud run services describe
- bare-name
- empty output
- deployed image tag
- readiness
- deploy.sh
- service URLs
- db_instance_name
- artifact_repo
- v1
- klara-nonprod
- test-agent-v2 image built only from pyproject + src + main.py
source: session 2026-09-13 deploy 2db8605
status: seedling
tags:
- gcp
- cloud-run
- test-agent-v2
- gotcha
- deploy
title: test-agent-v2 Cloud Run services use a -v2 name suffix
type: lesson
---

# test-agent-v2 Cloud Run services use a -v2 name suffix

test-agent-v2 deploys four Cloud Run services whose names all carry a `-v2` suffix (applied by terraform `name_prefix`): `knowledge-gathering-agent-v2`, `test-plan-definition-agent-v2`, `test-evaluation-agent-v2`, `mcp-gateway-v2`.

**Gotcha:** `gcloud run services describe <bare-name>` (e.g. `knowledge-gathering-agent`) returns *empty output* rather than erroring — it looks exactly like a rollout that produced no image. Always use the `-v2` suffixed names when verifying the deployed image tag / readiness:

```bash
gcloud run services describe knowledge-gathering-agent-v2 \
  --region=europe-west6 --project=klara-nonprod \
  --format="value(spec.template.spec.containers[0].image, status.conditions[0].status)"
```

The service URLs printed by `deploy.sh` are the ground truth for the real names. Related: db_instance_name/artifact_repo also need distinct `-v2` values so v2 does not 409 against v1 in the shared klara-nonprod project.

## Related

- [[test-agent-v2 image built only from pyproject + src + main.py]]

%% ai-graph-start %%

**Related notes:**
- [[Deploying the test-agent-v2 Cloud Run stack (names, tags, plan)]]
- [[test-agent-v2 hardened deploy.sh flow and the unique image-tag bump that forces a new revision]]
- [[test-agent-v2 image built only from pyproject + src + main.py]]
- [[Adding a Cloud Run service that shares one image var build first, targeted apply]]
- [[test-agent-v2 cloud resource and credential map (klara-nonprod)]]

**Relations:**
- test-agent-v2 — *deploys* — Cloud Run services
- Cloud Run services — *use name suffix* — -v2
- terraform — *applies* — name_prefix
- name_prefix — *is* — -v2
- test-agent-v2 — *deploys* — knowledge-gathering-agent-v2
- test-agent-v2 — *deploys* — test-plan-definition-agent-v2
- test-agent-v2 — *deploys* — test-evaluation-agent-v2
- test-agent-v2 — *deploys* — mcp-gateway-v2
- gcloud run services describe — *with* — bare-name
- bare-name — *returns* — empty output
- gcloud run services describe — *verifies* — deployed image tag
- gcloud run services describe — *verifies* — readiness
- deploy.sh — *prints* — service URLs
- service URLs — *are ground truth for* — real names
- db_instance_name — *needs distinct values for* — -v2
- artifact_repo — *needs distinct values for* — -v2
- -v2 — *avoids 409 against* — v1
- -v2 — *in project* — klara-nonprod
- v1 — *in project* — klara-nonprod
- test-agent-v2 — *related to* — test-agent-v2 image built only from pyproject + src + main.py

%% ai-graph-end %%