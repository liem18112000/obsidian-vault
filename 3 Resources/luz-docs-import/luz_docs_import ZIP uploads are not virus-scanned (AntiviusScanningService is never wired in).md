---
ai_hash: 6a109c4bf4fce7d9
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-07-31
entities: []
source: graphify investigation 2026-07-31, commit df874966
status: seedling
tags:
- luz-docs-import
- security
- gotcha
- dead-code
- graphify
title: luz_docs_import ZIP uploads are not virus-scanned (AntiviusScanningService
  is never wired in)
type: lesson
---

# luz_docs_import ZIP uploads are not virus-scanned (AntiviusScanningService is never wired in)

In `luz_docs_import` (verified 2026-07-31, commit df874966) the whole antivirus mechanism is coded but **dead**: `AntiviusScanningService.scanUploadFile()` calls `luz_antivirus` via `AntivirusRestClient`, and `FailureCode.INFECTED`, `JobStatus.SCANNING`, and `BundleConstant.SCANNING_VIRUS_FAIL` all exist — yet the service has **no `@Inject` site and no caller anywhere**. So uploaded ZIPs currently pass through the import flow **without ever being virus-scanned**.

`JobStatus.SCANNING` is declared but never assigned, which is consistent — the state the scan step would have set is unreachable.

**Why it matters / how to apply:** the scaffolding implies scanning was meant to sit between zip validation and job creation (or before each `createDocument` call in the async worker) and was never connected. Before assuming "the importer scans for malware", grep for a caller — there isnt one. If this is a security requirement, wiring it is a real gap, not a config toggle. Confirm with the team whether the omission is intentional.

How it was found: graphify code graph (node present, zero inbound call edges) + `grep -rn "AntiviusScanningService" src | grep -i inject` returning nothing.

See the full flow in the repo report `docs/document-import-technical-path.md` (§7 finding #1). Related: [[luz_docs_import detects dead async-worker pods via kubectl during status polling]]

## Related

- [[luz_docs_import detects dead async-worker pods via kubectl during status polling]]

%% ai-graph-start %%

**Related notes:**
- [[luz-docs-import AV scan covers only the metadata sidecar, never the document binary]]
- [[Per-file AV scan rejects one file; whole-job scan fails the whole import]]
- [[luz_docs_import detects dead async-worker pods via kubectl during status polling]]
- [[luz-docs-import scans metadata sidecars per-file, not the whole ZIP]]
- [[luz-docs-import antivirus whole-zip scan dominates first-import latency and scales with zip size]]

%% ai-graph-end %%