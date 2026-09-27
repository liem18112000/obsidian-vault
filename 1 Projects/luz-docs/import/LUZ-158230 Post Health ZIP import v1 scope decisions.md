---
title: "LUZ-158230 Post Health ZIP import v1 scope decisions"
created: 2026-09-15
type: argument
status: seedling
source: "Testing-Agent refine session 2026-09-14"
tags: [luz-docs, luz_docs_import, earchive, zip-import, LUZ-158230, scope]
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
