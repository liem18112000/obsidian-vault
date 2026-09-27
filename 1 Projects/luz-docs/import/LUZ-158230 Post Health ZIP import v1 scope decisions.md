---
ai_hash: 0cd64f218b173f46
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-15
entities:
- LUZ-158230
- Post Health ZIP import
- v1 scope/behaviour decisions
- Testing-Agent
- Malformed items
- Notification
- SNOMED healthData
- sender-supplied SNOMED codes
- de/fr/it/en labels
- server-side validation
- eArchive document service
- HEALTH document type
- luz_docs_import
- JSON shape
- Recipient resolution
- target recipient/tenant
- Rollout
- senders
- new health-data sender in prod
- product-owner
- compliance/legal sign-off
- Architecture
- ZIP parsing/validation
- internal module
- axonivy-prod/luz_docs_import
- luz_docs_view_controller
- luz_jsonstore
- luz_docs
- luz_docs_import dedup folders via view-controller API, documents via import job
  history
- LUZ-158230 transfer.zip import size limits (2GB zip, 200MB file)
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

Related: [[luz_docs_import dedup folders via view-controller API, documents via import job history|luz_docs_import dedup: folders via view-controller API, documents via import job history]], [[LUZ-158230 transfer.zip import size limits (2GB zip, 200MB file)]]

## Related

- [[luz_docs_import dedup folders via view-controller API, documents via import job history]]
- [[LUZ-158230 transfer.zip import size limits (2GB zip, 200MB file)]]

%% ai-graph-start %%

**Related notes:**
- [[LUZ-158230 ePost ZIP import - test scope decisions]]
- [[LUZ-158230 is the ePost eArchive transfer.zip document-import feature]]
- [[luz_docs_import scope no sender auth, individual tenants, partial-import policy]]
- [[LUZ-158230 import stores healthData as-is no validation, no SNOMED label resolution, schemaless Mongo]]
- [[LUZ-158230 QA edge-case decisions (ZIP import)]]

**Relations:**
- LUZ-158230 — *identifies* — Post Health ZIP import
- v1 scope/behaviour decisions — *for* — Post Health ZIP import
- v1 scope/behaviour decisions — *confirmed by* — Testing-Agent
- v1 scope/behaviour decisions — *include* — Malformed items
- v1 scope/behaviour decisions — *include* — Notification
- v1 scope/behaviour decisions — *include* — SNOMED healthData
- v1 scope/behaviour decisions — *include* — Recipient resolution
- v1 scope/behaviour decisions — *include* — Rollout
- v1 scope/behaviour decisions — *include* — Architecture
- Malformed items — *are* — skipped and reported
- Notification — *targets* — sender/recipient
- SNOMED healthData — *accepts* — sender-supplied SNOMED codes
- SNOMED healthData — *accepts* — de/fr/it/en labels
- SNOMED healthData — *has no* — server-side validation
- SNOMED healthData — *owned by* — eArchive document service
- SNOMED healthData — *on* — HEALTH document type
- luz_docs_import — *validates* — JSON shape
- luz_docs_import — *forwards* — SNOMED healthData
- Recipient resolution — *uses* — target recipient/tenant
- luz_docs_import — *validates existence of* — target recipient/tenant
- Rollout — *is* — open to all senders
- new health-data sender in prod — *requires* — product-owner
- new health-data sender in prod — *requires* — compliance/legal sign-off
- Architecture — *describes* — ZIP parsing/validation
- ZIP parsing/validation — *is an* — internal module
- internal module — *inside* — luz_docs_import
- axonivy-prod/luz_docs_import — *is a* — Repo
- axonivy-prod/luz_docs_import — *is primary for* — Post Health ZIP import
- luz_docs_view_controller — *is a* — Repo
- luz_jsonstore — *is a* — Repo
- luz_docs — *is a* — Repo
- Post Health ZIP import — *related to* — luz_docs_import dedup folders via view-controller API, documents via import job history
- Post Health ZIP import — *related to* — LUZ-158230 transfer.zip import size limits (2GB zip, 200MB file)

%% ai-graph-end %%