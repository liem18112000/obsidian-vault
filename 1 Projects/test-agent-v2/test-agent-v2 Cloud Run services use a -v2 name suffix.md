---
title: "test-agent-v2 Cloud Run services use a -v2 name suffix"
created: 2026-09-13
type: lesson
status: seedling
source: "session 2026-09-13 deploy 2db8605"
tags: [gcp, cloud-run, test-agent-v2, gotcha, deploy]
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
