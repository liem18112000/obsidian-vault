---
ai_hash: 890d408f86fc6c91
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-13
entities: []
source: session 2026-09-13
status: seedling
tags:
- testing-agent
- gcp
- cloud-run
- cloudsql
- v2
title: test-agent-v2 cloud resource and credential map (klara-nonprod)
type: reference
---

# test-agent-v2 cloud resource and credential map (klara-nonprod)

The test-agent-v2 (ADK) stack runs on GCP project **klara-nonprod**, region **europe-west6**, under `name_prefix = "kga-v2"` — a separate, coexisting deployment from v1 (which uses prefix `kga`).

## Resources
- **Cloud SQL** (A2A task store, DatabaseTaskStore): instance `kga-v2-taskstore`, database `taskstore`, login role `taskstore`, password in Secret Manager secret `kga-v2-db-password` (terraform-generated). The `tasks` table is created lazily — it is **absent until the first A2A Task is persisted**, so an empty/idle stack has no table to truncate.
- **GCS memory bucket**: `klara-nonprod-kga-v2-memory`, rooted at `memory/` with folders `index/ notes/ refine/ test-plan/ runs/`.
- **Cloud Run services** (all four, `*-v2`): `knowledge-gathering-agent-v2`, `test-plan-definition-agent-v2`, `test-evaluation-agent-v2`, `mcp-gateway-v2`. Idle-healthy logs show only `GET /livez 200` probes.

Source of truth for the non-secret config is `deployments/test-agent-v2/terraform.tfvars` + `cloudsql.tf`; the app also reads `test-agent-v2/.env` (GCS_BUCKET/GCP_PROJECT). Secret values are never recorded here.

## Related
[[Wipe test-agent-v2 memory and taskstore via test-agent-v2/tools]]

## Related

- [[Wipe test-agent-v2 memory and taskstore via test-agent-v2/tools]]

%% ai-graph-start %%

**Related notes:**
- [[Wipe test-agent-v2 memory and taskstore via test-agent-v2tools]]
- [[Deploying the test-agent-v2 Cloud Run stack (names, tags, plan)]]
- [[test-agent-v2 Cloud Run services use a -v2 name suffix]]
- [[Testing-Agent GCS memory bank one bucket, memory root, five subfolders]]
- [[Run test-agent-v2 locally with docker-compose (no GCP)]]

%% ai-graph-end %%