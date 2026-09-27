---
title: "LUZ-158230 is the ePost eArchive transfer.zip document-import feature"
created: 2026-09-15
type: concept
status: seedling
source: "testing-agent run run-87933300, 2026-09-15"
tags: [luz, eArchive, zip-import, luz_docs_import, LUZ-158230]
---

# LUZ-158230 is the ePost eArchive transfer.zip document-import feature

LUZ-158230 enables an **external sender** (launch sender: "Post Health") to deliver documents into a recipient's ePost **eArchive** by uploading a `transfer.zip`. The ZIP holds a folder tree, the actual document files, and a per-document `metadata.json` sidecar. It introduces a `HEALTH` document type whose sidecar carries structured **SNOMED-coded** `healthData` (category / facility / practice-setting, each with de/fr/it/en labels).

Implemented in the **`axonivy-prod/luz_docs_import`** service; supporting repos are `luz_docs_view_controller`, `luz_jsonstore`, `luz_docs`. The import parser/validator lives *inside* luz_docs_import as an internal component, not a shared library.

Why it matters: this is the anchor context for any test-planning or debugging on the eArchive ZIP-import path — the sender→transfer.zip→eArchive pipeline and the metadata.json contract are the whole surface.

## Related
[[luz_docs_import import stores healthData as-is]]

## Related

- [[luz_docs_import import stores healthData as-is]]
