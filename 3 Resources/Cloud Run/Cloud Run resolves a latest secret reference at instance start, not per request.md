---
title: "Cloud Run resolves a :latest secret reference at instance start, not per request"
created: 2026-09-25
type: lesson
status: seedling
source: "session 2026-09-25 — test-agent-v2 bearer rotation"
tags: [gcp, cloud-run, secret-manager, rotation, gotcha]
---

# Cloud Run resolves a :latest secret reference at instance start, not per request

A Cloud Run container that mounts a secret as `secretKeyRef: {secret: X, version: latest}` resolves that reference **once, when the instance boots**, and then holds the value in its environment for the instance's whole life. So adding a new Secret Manager version revokes exactly nobody — every already-running instance keeps accepting the old value.

This is easy to get wrong because `latest` *reads* like "always current". It isn't a live lookup; it's a pointer dereferenced at startup.

It bites hardest when `min_instances >= 1`, because then nothing ever scales to zero and no instance restarts on its own. The old credential can stay valid indefinitely.

A rotation is therefore two halves, and the second half is the one that actually revokes:

1. Add the new version, then **disable every superseded version** so the old value can't be read back out of Secret Manager. Safe only while nothing pins a version *number* — all consumers must be on `latest`.
2. **Force a new revision on every consumer** so it re-reads the reference. The cheapest trigger is a throwaway env var whose only job is to make the revision differ:

```bash
gcloud run services update "$svc" --region "$region" \
  --update-env-vars="BEARER_ROTATED_AT=$(date +%s)"
```

That stamp is terraform drift the next `apply` silently drops, which is harmless: by then `latest` already resolves to the new value, so the revision terraform creates reads the same secret.

Worked example: `deployments/test-agent-v2/rotate_a2a_bearer_key.sh`, which rotates both bearers across all seven services and then curl-probes the gateway to prove the old token now 401s.

## Related

- [[Rotating only the gateway bearer on test-agent-v2 locks nobody out]]
- [[test-agent-v2 Cloud Run services use a -v2 name suffix]]
