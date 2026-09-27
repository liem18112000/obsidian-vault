---
ai_hash: 08bd5351adec6154
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-23
entities: []
source: session 2026-09-23
status: seedling
tags:
- terraform
- gcloud
- cleanup
- state
- gotcha
title: Stale terraform state != live cloud — verify with gcloud before deleting a
  retired deployment
type: lesson
---

# Stale terraform state != live cloud — verify with gcloud before deleting a retired deployment

LESSON (cleaning up a retired deployment): a leftover terraform.tfstate does NOT mean the cloud resources still exist — state goes stale when resources are destroyed out-of-band or a prior destroy shrank it. Before deleting a stale deployment dir, VERIFY the live cloud is clean with read-only `gcloud` lists, do not trust state: `gcloud sql instances list`, `gcloud run services list --region <r>`, `gcloud artifacts repositories list --location <r>`. Here test-agent-v1s state still claimed the `kga-taskstore` Cloud SQL instance, but it (and the `kga` artifact repo + all v1 Cloud Run services) were already gone — only the `-v2` equivalents (kga-v2-taskstore, kga-v2, *-agent-v2) remained. CRUCIAL: v1 state also held 7 `google_project_service` API-enablement entries (aiplatform/run/sqladmin/…) — those are PROJECT-SHARED; never `terraform destroy` them (would disable APIs for v2 and everything else). Since the real resources were already gone and the APIs are shared, the safe cleanup was just `rm -rf` the orphaned dir (deleting local state never calls the cloud) — NOT a terraform destroy. Also confirm the new version uses distinct names (v2 = kga-v2-taskstore/kga-v2) so removing v1 cant touch it. See [[v2 deploy collides with v1 names]].

%% ai-graph-start %%

**Related notes:**
- [[terraform destroy fails to drop a Cloud SQL Postgres DB with active connections or owned objects]]
- [[Wipe test-agent-v2 memory and taskstore via test-agent-v2tools]]
- [[Concurrent test-agent-v2 deploys collide on terraform local-state lock (fails safe)]]
- [[test-agent-v2 cloud resource and credential map (klara-nonprod)]]
- [[Cloud Run v2 deletion_protection defaults true — set false and apply before destroy]]

%% ai-graph-end %%