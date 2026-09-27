---
ai_hash: e588e0d8b3694f19
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-14
entities: []
source: LUZ-158230 interrogation, session 2026-09-14
status: seedling
tags:
- luz
- ePost
- eArchive
- luz-docs-import
- scope
- LUZ-158230
title: 'luz_docs_import scope: no sender auth, individual tenants, partial-import
  policy'
type: lesson
---

# luz_docs_import scope: no sender auth, individual tenants, partial-import policy

Scope/behaviour facts about the luz_docs_import ZIP import that shape testing:

- **No sender authorization.** luz_docs_import does **not** authorize `senderTenantId`/`senderCompanyId` — the sender fields are stored as **provenance metadata only**, no whitelist enforcement at the import service. So there is **no "unauthorized sender rejected"** behaviour to test at this layer.
- **Individual tenants only.** luz_docs_import serves only individual tenants, who **always have the eArchive widget enabled by default** — so a recipient never lacks an eArchive (no reject/queue-for-missing-account path).
- **Partial-import policy (not all-or-nothing).** Valid documents always land. A document **missing** its `metadata.json` is imported with **minimal/default metadata**; a **malformed** sidecar is **skipped and reported per-document**.
- **Failure notification.** An import failure publishes an event to **luz-messagebroker** (Cloud Run service `dev-luz-message-broker`) which drives the **customer** notification; a successful import reuses the existing eArchive "new document" notification flow.

Established for LUZ-158230.

## Related
[[ePost eArchive ZIP import: transfer.zip shape and luz_docs_import]]
[[HEALTH document type carries verbatim SNOMED healthData]]

%% ai-graph-start %%

**Related notes:**
- [[LUZ-158230 Post Health ZIP import v1 scope decisions]]
- [[senderTenantId is out of scope in luz_docs_import health ZIP import]]
- [[ePost ZIP import (LUZ-158230) behavior rules confirmed by domain expert]]
- [[luz_docs_import health ZIP import uses a two-layer failure model]]
- [[ePost eArchive ZIP import transfer.zip shape and luz_docs_import]]

%% ai-graph-end %%