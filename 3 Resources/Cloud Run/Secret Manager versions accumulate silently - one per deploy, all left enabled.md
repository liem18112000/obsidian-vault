---
ai_hash: 45398e538d88aa88
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-25
entities: []
source: session 2026-09-25 — test-agent-v2
status: seedling
tags:
- gcp
- secret-manager
- deploy
- gotcha
- hygiene
title: Secret Manager versions accumulate silently — one per deploy, all left enabled
type: observation
---

# Secret Manager versions accumulate silently — one per deploy, all left enabled

A deploy script that does `gcloud secrets versions add` on every run adds a version every run — and `add` never touches the previous ones. They all stay in state `ENABLED` forever.

On test-agent-v2 this had quietly reached **58 and 59 enabled versions** on two secrets, one per deploy since the project started. Nothing warns you: `versions add` prints the new version number and exits 0, and `versions list` is not something anyone runs day to day.

Why it matters: *every* enabled version is still readable by anything holding `secretmanager.versionAccessor`. So the count is the real answer to "how many old values of this secret are still live?" — and with a per-deploy `add`, the answer is "all of them".

Two consequences worth knowing:

- Any cleanup pass that disables the superseded versions is O(count) sequential `gcloud` calls. At ~58 per secret that is minutes of wall clock, not seconds — budget for it rather than assuming it's instant.
- `gcloud secrets versions list --sort-by=~name --limit=1` does sort version names **numerically**, not lexicographically, so it correctly returns 59 rather than 9. Verified empirically against this secret; worth re-checking if you rely on it, because the field is printed as a bare number and string-sorting would silently pick the wrong "newest".

## Related

- [[Cloud Run resolves a latest secret reference at instance start, not per request]]
- [[test-agent-v2 Cloud Run services use a -v2 name suffix]]

%% ai-graph-start %%

**Related notes:**
- [[Cloud Run resolves a latest secret reference at instance start, not per request]]
- [[Adding a Cloud Run service that shares one image var build first, targeted apply]]
- [[Stale terraform state != live cloud — verify with gcloud before deleting a retired deployment]]
- [[A disabled Cloud Run service 503s at the edge and never reaches your app]]
- [[Cloud Run v2 service design gotchas]]

%% ai-graph-end %%