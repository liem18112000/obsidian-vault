---
ai_hash: 063db0f78de1071c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-15
entities:
- LUZ-158230 Post Health ZIP import v1
- Post Health ZIP import
- Testing-Agent
- Malformed items
- Notification
- SNOMED healthData
- Recipient resolution
- Rollout
- Architecture
- luz_docs_import
- eArchive document service
- HEALTH document type
- JSON shape
- axonivy-prod
- luz_docs_view_controller
- luz_jsonstore
- luz_docs
- product-owner
- compliance/legal
- luz_docs_import dedup
- folders via view-controller API
- documents via import job history
- LUZ-158230 transfer.zip import size limits
- 2GB zip
- 200MB file
- sender-supplied SNOMED codes
- de/fr/it/en labels
- resolved target recipient/tenant
- ZIP parsing/validation
- health-data sender in prod
- scope decisions
- all outcomes
- partial best-effort import
- per-item reporting
- healthData
source: Testing-Agent refine session 2026-09-14
status: seedling
tags:
- luz-docs
- luz_docs_import
- earchive
- zip-import
- LUZ-158230
- scope
title: LUZ-158230 Post Health ZIP import v1 scope decisions
type: argument
---

# LUZ-158230 Post Health ZIP import v1 scope decisions

v1 scope/behaviour decisions for the Post Health ZIP import (LUZ-158230), confirmed during a Testing-Agent refine session:

- **Malformed items**: partial best-effort import — valid docs/folders import, invalid ones are skipped and reported **per-item** with granular per-document status. NOT atomic all-or-nothing.
- **Notification**: notify sender/recipient in **all** outcomes — success, partial, and failure.
- **SNOMED healthData**: v1 accepts sender-supplied SNOMED codes + de/fr/it/en labels **as-is, no server-side validation**. healthData is owned/stored canonically by the **eArchive document service** on the HEALTH document type; luz_docs_import only validates JSON shape and forwards it (import is stateless about long-term metadata).
- **Recipient resolution**: the import request already carries the **resolved** target recipient/tenant; import only validates existence (no internal identity lookup).
- **Rollout**: open to all senders at launch (no gating/feature flag), but enabling a new health-data sender in prod requires **product-owner + compliance/legal sign-off**.
- **Architecture**: ZIP parsing/validation is a dedicated internal module inside luz_docs_import.

Repos: axonivy-prod/luz_docs_import (primary), luz_docs_view_controller, luz_jsonstore, luz_docs.

Related: [[luz_docs_import dedup: folders via view-controller API, documents via import job history]], [[LUZ-158230 transfer.zip import size limits (2GB zip, 200MB file)]]

## Related

- [[luz_docs_import dedup: folders via view-controller API]]
- [[documents via import job history]]
- [[LUZ-158230 transfer.zip import size limits (2GB zip]]
- [[200MB file)]]

%% ai-graph-start %%

**Related notes:**
- [[LUZ-158230 ePost ZIP import - test scope decisions]]
- [[LUZ-158230 is the ePost eArchive transfer.zip document-import feature]]
- [[luz_docs_import scope no sender auth, individual tenants, partial-import policy]]
- [[LUZ-158230 import stores healthData as-is no validation, no SNOMED label resolution, schemaless Mongo]]
- [[LUZ-158230 QA edge-case decisions (ZIP import)]]

**Relations:**
- LUZ-158230 Post Health ZIP import v1 — *is a version of* — Post Health ZIP import
- LUZ-158230 Post Health ZIP import v1 — *has scope decisions* — scope decisions
- scope decisions — *confirmed during* — Testing-Agent
- scope decisions — *include* — Malformed items
- scope decisions — *include* — Notification
- scope decisions — *include* — SNOMED healthData
- scope decisions — *include* — Recipient resolution
- scope decisions — *include* — Rollout
- scope decisions — *include* — Architecture
- Malformed items — *result in* — partial best-effort import
- Malformed items — *are reported* — per-item reporting
- Notification — *applies to* — all outcomes
- SNOMED healthData — *accepts* — sender-supplied SNOMED codes
- SNOMED healthData — *accepts* — de/fr/it/en labels
- SNOMED healthData — *has* — no server-side validation
- SNOMED healthData — *is a type of* — healthData
- healthData — *owned by* — eArchive document service
- healthData — *stored by* — eArchive document service
- healthData — *on* — HEALTH document type
- luz_docs_import — *validates* — JSON shape
- luz_docs_import — *forwards* — healthData
- Recipient resolution — *carries* — resolved target recipient/tenant
- luz_docs_import — *validates existence of* — resolved target recipient/tenant
- Rollout — *is open to* — all senders at launch
- enabling a new health-data sender in prod — *requires sign-off from* — product-owner
- enabling a new health-data sender in prod — *requires sign-off from* — compliance/legal
- Architecture — *describes* — ZIP parsing/validation
- ZIP parsing/validation — *is a module inside* — luz_docs_import
- axonivy-prod — *contains repo* — luz_docs_import
- axonivy-prod — *contains repo* — luz_docs_view_controller
- axonivy-prod — *contains repo* — luz_jsonstore
- axonivy-prod — *contains repo* — luz_docs
- luz_docs_import — *is primary repo in* — axonivy-prod
- LUZ-158230 Post Health ZIP import v1 — *related to* — luz_docs_import dedup
- luz_docs_import dedup — *handles* — folders via view-controller API
- luz_docs_import dedup — *handles* — documents via import job history
- LUZ-158230 Post Health ZIP import v1 — *related to* — LUZ-158230 transfer.zip import size limits
- LUZ-158230 transfer.zip import size limits — *includes* — 2GB zip
- LUZ-158230 transfer.zip import size limits — *includes* — 200MB file

%% ai-graph-end %%