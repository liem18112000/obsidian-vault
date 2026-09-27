---
ai_hash: db90b6b8576b46b5
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-15
entities:
- LUZ-158230
- ePost eArchive transfer.zip document-import feature
- ePost eArchive
- transfer.zip
- external sender
- Post Health
- HEALTH document type
- metadata.json
- SNOMED-coded healthData
- axonivy-prod/luz_docs_import
- luz_docs_view_controller
- luz_jsonstore
- luz_docs
- import parser/validator
- internal component
- sender→transfer.zip→eArchive pipeline
- metadata.json contract
- luz_docs_import import stores healthData as-is
- folder tree
- document files
- category
- facility
- practice-setting
- test-planning
- debugging
- eArchive ZIP-import path
source: testing-agent run run-87933300, 2026-09-15
status: seedling
tags:
- luz
- eArchive
- zip-import
- luz_docs_import
- LUZ-158230
title: LUZ-158230 is the ePost eArchive transfer.zip document-import feature
type: concept
---

# LUZ-158230 is the ePost eArchive transfer.zip document-import feature

LUZ-158230 enables an **external sender** (launch sender: "Post Health") to deliver documents into a recipient's ePost **eArchive** by uploading a `transfer.zip`. The ZIP holds a folder tree, the actual document files, and a per-document `metadata.json` sidecar. It introduces a `HEALTH` document type whose sidecar carries structured **SNOMED-coded** `healthData` (category / facility / practice-setting, each with de/fr/it/en labels).

Implemented in the **`axonivy-prod/luz_docs_import`** service; supporting repos are `luz_docs_view_controller`, `luz_jsonstore`, `luz_docs`. The import parser/validator lives *inside* luz_docs_import as an internal component, not a shared library.

Why it matters: this is the anchor context for any test-planning or debugging on the eArchive ZIP-import path — the sender→transfer.zip→eArchive pipeline and the metadata.json contract are the whole surface.

## Related
[[luz_docs_import import stores healthData as-is]]

## Related

- [[luz_docs_import import stores healthData as-is]]

%% ai-graph-start %%

**Related notes:**
- [[ePost eArchive ZIP import transfer.zip shape and luz_docs_import]]
- [[LUZ-158230 import stores healthData as-is no validation, no SNOMED label resolution, schemaless Mongo]]
- [[LUZ-158230 Post Health ZIP import v1 scope decisions]]
- [[HEALTH document type carries verbatim SNOMED healthData]]
- [[ePost Health documents ZIP import business context]]

**Relations:**
- LUZ-158230 — *is* — ePost eArchive transfer.zip document-import feature
- ePost eArchive transfer.zip document-import feature — *enables* — external sender
- external sender — *is* — Post Health
- external sender — *delivers documents to* — ePost eArchive
- external sender — *uploads* — transfer.zip
- transfer.zip — *holds* — folder tree
- transfer.zip — *holds* — document files
- transfer.zip — *holds* — metadata.json
- metadata.json — *is a* — sidecar
- ePost eArchive transfer.zip document-import feature — *introduces* — HEALTH document type
- HEALTH document type — *has sidecar* — metadata.json
- metadata.json — *carries* — SNOMED-coded healthData
- SNOMED-coded healthData — *includes* — category
- SNOMED-coded healthData — *includes* — facility
- SNOMED-coded healthData — *includes* — practice-setting
- LUZ-158230 — *implemented in* — axonivy-prod/luz_docs_import
- axonivy-prod/luz_docs_import — *supports* — luz_docs_view_controller
- axonivy-prod/luz_docs_import — *supports* — luz_jsonstore
- axonivy-prod/luz_docs_import — *supports* — luz_docs
- import parser/validator — *lives inside* — axonivy-prod/luz_docs_import
- import parser/validator — *is an* — internal component
- LUZ-158230 — *is anchor context for* — test-planning
- LUZ-158230 — *is anchor context for* — debugging
- test-planning — *on* — eArchive ZIP-import path
- debugging — *on* — eArchive ZIP-import path
- eArchive ZIP-import path — *defined by* — sender→transfer.zip→eArchive pipeline
- eArchive ZIP-import path — *defined by* — metadata.json contract
- LUZ-158230 — *related to* — luz_docs_import import stores healthData as-is

%% ai-graph-end %%