---
ai_hash: 1be559f4ea0677ed
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-11
entities: []
source: session 2026-08-11, data/Lam/new-import-08.zip
status: seedling
tags:
- luz-docs-import
- import
- zip
- metadata
- snomed
title: luz-docs-import ingest ZIP format (folders + per-file .metadata.json sidecar)
type: reference
---

# luz-docs-import ingest ZIP format (folders + per-file .metadata.json sidecar)

An import ZIP for the **luz-docs-import** service is structured as: top-level folders act as logical document folders (e.g. `Medical documents/`, `Vaccination/`, `Lab results/`). Inside each folder, every document is a **pair** — the binary file (e.g. a PDF) plus a sidecar named exactly `<full-filename-including-extension>.metadata.json` (so `an image 001.pdf` -> `an image 001.pdf.metadata.json`).

**Sidecar** = UTF-8, 4-space-indented JSON, no trailing newline. Schema observed in `new-import-08.zip`:

```jsonc
{
  "senderTenantId": "<uuid>",
  "senderCompanyId": 1,
  "senderName": "Post Health",
  "documentTitle": "...",
  "documentTypes": ["HEALTH"],
  "documentReferenceDate": "YYYY-MM-DD",
  "healthData": {
    "author": "...",
    "documentCategory":  { "code": "...", "de": "...", "fr": "...", "it": "...", "en": "..." },
    "facility":          { "code": "...", "de": "...", "fr": "...", "it": "...", "en": "..." },
    "practiceSetting":   { "code": "...", "de": "...", "fr": "...", "it": "...", "en": "..." }
  }
}
```

The `code` values under `documentCategory` / `facility` / `practiceSetting` are **SNOMED CT** codes with `de/fr/it/en` localized labels — e.g. `371538006` (Patientenverfügung / Advance directives), `394914008` (patient's home), `394234008` (Radiology).

**ZIP entry order** in the reference archive: all directory entries first, then per document the file entry immediately followed by its `.metadata.json` entry.

**Gotcha:** the sidecar is matched to its file by the *full* filename including extension, so the importer keys on `<name>.<ext>.metadata.json`, not `<name>.metadata.json`.

A reusable stdlib-only Python replicator lives at `luz_docs_import/data/Lam/new-import-generator/generate_import_zip.py` (verified byte-exact for the reference content).

%% ai-graph-start %%

**Related notes:**
- [[LUZ-158230 ePost ZIP import spec v1.0 (authoritative)]]
- [[ePost eArchive ZIP import transfer.zip shape and luz_docs_import]]
- [[ePost Health documents ZIP import business context]]
- [[LUZ-158230 is the ePost eArchive transfer.zip document-import feature]]
- [[luz_docs_import adds document metadata only in DocsImportAsyncService.createDocument()]]

%% ai-graph-end %%