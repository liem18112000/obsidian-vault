---
ai_hash: 5321dc8fefdcc7f7
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.85
entities: []
relevance: 0.734
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49574871045/Post-deployment+Batch+messageCount+backfill+Test+Prod+LUZ-155431+LUZ-155435
space: LUZ
status: reference
tags:
- confluence
- infra
- space/luz
title: 'Post-deployment: Batch messageCount backfill (Test & Prod) — LUZ-155431 /
  LUZ-155435'
topic: infra
type: source
updated: 2026-07-09
---

# Post-deployment: Batch messageCount backfill (Test & Prod) — LUZ-155431 / LUZ-155435

> [!info] Imported from Confluence
> Space **LUZ** · updated 2026-07-09 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49574871045/Post-deployment+Batch+messageCount+backfill+Test+Prod+LUZ-155431+LUZ-155435)
> Relevance 0.734 · topic `infra`

# Post-deployment: Batch messageCount backfill (LUZ-155431 / LUZ-155435)

**Audience:** Release engineers, ops, developers deploying `luz-epc-api` to GKE  
**Applies to:** Test and Production  
**Repo reference:** `luz_epc/scripts/backfill-batch-message-count/POST_RELEASE_DOC.md`

------------------------------------------------------------------------

## 1. Purpose

After deploying `luz-epc-api` with the restored `batches` collection and materialized `messageCount`, MongoDB must be aligned with MessageV2 data **before** users rely on:

- Admin **Reports** — `POST /batches/search`

- **SmartSend** order overview pagination

- Monitoring batch totals (where applicable)

This page describes **manual post-deployment steps** on GKE. Scripts are bundled in the API image (same pattern as Message V2 migration).

------------------------------------------------------------------------

## 2. What is deployed automatically

<div>

|  |  |  |
|----|----|----|
| Step | Automated | Details |
| Build `luz-epc-api` image with scripts | Yes | Cloud Build → `api.Dockerfile` → `/app/backfill-batch-message-count` |
| Push image to Artifact Registry | Yes | `luz-epc-api:<commit>` |
| Update K8s manifest image tag | Yes | `luz_kubernetes/update_image_version.sh` |
| GKE rollout | Yes | Standard deploy per environment |
| Compare + backfill on MongoDB | **Manual** | `kubectl exec` into `luz-epc-api` pod |

</div>

------------------------------------------------------------------------

## 3. Scripts overview

<div>

|  |  |  |
|----|----|----|
| Script (in pod) | Mode | Purpose |
| `/app/backfill-batch-message-count/compare-batches-vs-messages.js` | Read-only | Report missing batches, mismatches, orphans |
| `/app/backfill-batch-message-count/app.js` | **Writes** | Upsert `batches` + `messageCount` |

</div>

**DB connection:** `DB_CONNECTION_STRING` from `luz-epc-api-env-secret` (per env). No manual URI needed.

------------------------------------------------------------------------

## 4. Release order

1.  Development — validate first

2.  **Test** — this page

3.  **Production** — after Test sign-off + MongoDB backup

------------------------------------------------------------------------

## 5. Pre-requisites

- <span class="placeholder-inline-tasks">`luz-epc-api` rollout complete (image includes scripts)</span>
- <span class="placeholder-inline-tasks">Correct `kubectl` context / GKE cluster</span>
- <span class="placeholder-inline-tasks">`kubectl exec` access in target namespace</span>
- <span class="placeholder-inline-tasks">**Test / Prod:** MongoDB backup before writes</span>
- <span class="placeholder-inline-tasks">Prod: stakeholders notified</span>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="877f3a2c-7b6c-475a-b88c-8809668affa0" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl rollout status deployment/luz-epc-api -n <namespace> --timeout=600s
kubectl exec deployment/luz-epc-api -n <namespace> -- ls /app/backfill-batch-message-count
```

</div>

</div>

------------------------------------------------------------------------

# TEST environment

## Test — details

<div>

|               |                                |
|---------------|--------------------------------|
| Item          | Value                          |
| GKE namespace | `test`                         |
| K8s overlay   | `kubernetes-overlays/env-test` |
| Deployment    | `luz-epc-api`                  |
| GCP project   | `klara-nonprod`                |

</div>

## Test — checklist

**Before:** deployment done, Mongo backup, dev sign-off  
**During:** pre-compare, dry-run, backfill, logs saved  
**After:** post-compare (insert=0, mismatches=0), smoke tests, sign-off

## Test — commands

**Step 0**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="be25f443-a561-43c8-b4c7-ab36caf852f9" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
NS=test
kubectl rollout status deployment/luz-epc-api -n $NS --timeout=600s
```

</div>

</div>

**Step 1 — Pre-compare**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a7e7ffff-43fd-4b13-a6db-86f611ab58bb" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl exec deployment/luz-epc-api -n test -- \
  node /app/backfill-batch-message-count/compare-batches-vs-messages.js \
  | tee compare-batches-output-pre-test.log
```

</div>

</div>

**Step 2 — Dry-run**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="152b5167-2f12-481f-873e-b34d45ea4023" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl exec deployment/luz-epc-api -n test -- \
  env DRY_RUN=true node /app/backfill-batch-message-count/app.js \
  | tee backfill-dryrun-test.log
```

</div>

</div>

**Step 3 — Backfill**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="41ccb71c-2218-441f-8ea6-aa09ac4ad207" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl exec deployment/luz-epc-api -n test -- \
  node /app/backfill-batch-message-count/app.js \
  | tee backfill-output-test.log
```

</div>

</div>

Or PowerShell full sequence:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="cb26cb32-b78d-43b8-afe2-bfedff0b42b7" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
cd luz_epc\scripts\backfill-batch-message-count
.\run-post-release-k8s.ps1 -Namespace test -ConfirmBackfill
```

</div>

</div>

**Step 4 — Post-compare** (must show insert=0, mismatches=0)

**Step 5 — Smoke tests:** Reports batch search, SmartSend pagination, Monitoring

**Step 6 — Catch-up:** re-run backfill + compare if needed (idempotent)

------------------------------------------------------------------------

# PRODUCTION environment

## Prod — details

<div>

|               |                                |
|---------------|--------------------------------|
| Item          | Value                          |
| GKE namespace | `prod`                         |
| K8s overlay   | `kubernetes-overlays/env-prod` |
| Deployment    | `luz-epc-api`                  |
| GCP project   | `klara-prod`                   |

</div>

## Prod — additional requirements

> **Warning:** Backfill writes to MongoDB. Test sign-off required first.

- <span class="placeholder-inline-tasks">Mandatory MongoDB backup before backfill</span>
- <span class="placeholder-inline-tasks">Maintenance window agreed</span>
- <span class="placeholder-inline-tasks">On-call available</span>

## Prod — commands

Same steps as Test with `namespace=prod` and `*-prod.log` files.

**Recommended — background backfill:**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1ec8e63e-e96c-49ef-8e64-2e18db31a958" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl exec deployment/luz-epc-api -n prod -- \
  node /app/backfill-batch-message-count/app.js \
  > backfill-output-prod.log 2>&1 &
tail -f backfill-output-prod.log
```

</div>

</div>

------------------------------------------------------------------------

## Sign-off

<div>

|             |             |          |                 |             |     |      |
|-------------|-------------|----------|-----------------|-------------|-----|------|
| Environment | Pre-compare | Backfill | Post-compare OK | Smoke tests | By  | Date |
| Test        |             |          |                 |             |     |      |
| Production  |             |          |                 |             |     |      |

</div>

------------------------------------------------------------------------

## Troubleshooting

<div>

|                              |                                        |
|------------------------------|----------------------------------------|
| Issue                        | Fix                                    |
| deployment not found         | Check namespace                        |
| DB_CONNECTION_STRING missing | Check `luz-epc-api-env-secret`         |
| Script not in pod            | Redeploy with current `api.Dockerfile` |
| OOM exit 137                 | `BACKFILL_BATCH_SIZE=200`              |
| Drift after backfill         | Re-run backfill                        |

</div>

------------------------------------------------------------------------

## Related

- Jira: LUZ-155431, LUZ-155435

- Repo: `luz_epc/scripts/backfill-batch-message-count/`

- Pattern: Message V2 migration `kubectl exec` flow

%% ai-graph-start %%

**Related notes:**
- [[Migration Script Execution Guide for luz-epc-api]]
- [[One API end to end testing]]
- [[Infrastructure]]
- [[ELM5 PubSub Message Queue]]
- [[Deploy luz-epc-redis-service on GCP]]

%% ai-graph-end %%