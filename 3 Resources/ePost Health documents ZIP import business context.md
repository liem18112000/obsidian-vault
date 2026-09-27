---
ai_hash: 2007d37bcb967229
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-24
entities: []
tags:
- klara
- epost
- luz-docs-import
- health
- business
- spec
---

# ePost Health documents — ZIP import business context

The business driver behind **luz-docs-import**: let health-sector senders bulk-deliver official health documents
into a Swiss citizen's **KLARA myLife** digital storage.

- **Who:** health senders (e.g. *Post Health*, clinics, practices) → KLARA myLife citizens (multilingual CH).
- **What:** advance directives (*Patientenverfügung*), vaccination records, medical documents — `documentTypes: ["HEALTH"]`.
- **Delivery unit:** one **ZIP** ("transfer.zip"). Folders inside become storage folders (e.g. `Medical documents/`, `Vaccination/`).
  Each document `X.pdf` ships a sidecar **`X.pdf.metadata.json`**. Ignored entries: `__MACOSX/`, `.DS_Store`, `Thumbs.db`.
- **Per-document metadata (`.metadata.json`):** `senderTenantId`, `senderCompanyId`, `senderName`, `documentTitle`,
  `documentTypes`, `documentReferenceDate` (YYYY-MM-DD), and `healthData` = `author` + `documentCategory` /
  `facility` / `practiceSetting`, each a **SNOMED code** with **de/fr/it/en** labels.

Ties to other work:
- Explains why the demo's Storage shows **Vaccination** + **Medical documents** folders (they come straight from the sample ZIP).
- Explains why the **first antivirus scan is on the metadata file** (not the ZIP) — see [[luz-docs-import ZIP import call chain]].

Sources: `…/luz-docs-import/business_resource/` — "ePost — ZIP document import specification" + "ePost — Health documents development handoff". Jira LUZ-158243 / LUZ-158230 (axonivy site) cover the feature but weren't accessible via the connected Atlassian MCP (only *leocdp* granted).

%% ai-graph-start %%

**Related notes:**
- [[LUZ-158230 is the ePost eArchive transfer.zip document-import feature]]
- [[LUZ-158230 ePost ZIP import spec v1.0 (authoritative)]]
- [[ePost eArchive ZIP import transfer.zip shape and luz_docs_import]]
- [[HEALTH document type carries verbatim SNOMED healthData]]
- [[LUZ-158230 Post Health ZIP import v1 scope decisions]]

%% ai-graph-end %%