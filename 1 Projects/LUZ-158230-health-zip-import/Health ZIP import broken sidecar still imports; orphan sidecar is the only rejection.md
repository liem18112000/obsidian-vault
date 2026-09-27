---
ai_hash: a24579ba0f03bac9
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-04
entities:
- Health ZIP import
- sidecar
- orphan sidecar
- ePost health ZIP import
- transfer.zip
- LUZ-158230
- BRule-4
- UNPARSEABLE <filename>.metadata.json sidecar
- document
- filename
- title
- broken sidecar
- BRule-5
- sidecar with NO matching sibling document
- entry-level rejection
- spec
- BR-05
- field
- schema validation
- Unrecognised top-level keys
- internal system properties
- sender
- healthData
- opaque passthrough subtree
- POJO
- extra languages/keys
- Sidecar recognition
- BR-03
- complete document filename incl. extension + lowercase .metadata.json
- same folder
- documentTypes
- HEALTH
- BR-07
- luz_docs_import adds document metadata only in DocsImportAsyncService.createDocument()
- DocsImportAsyncService.createDocument()
- luz_docs_import isExcludedFile misses Thumbs.db and __MACOSX (BR-04 gap)
- Thumbs.db
- __MACOSX
- BR-04 gap
source: SPEC v1.0 / BRD v0.3 2026-08-04
status: seedling
tags:
- luz-docs-import
- kepler
- LUZ-158230
- import-rules
title: Health ZIP import broken sidecar still imports; orphan sidecar is the only
  rejection
type: lesson
---

# Health ZIP import broken sidecar still imports; orphan sidecar is the only rejection

The ePost health ZIP import (transfer.zip, LUZ-158230) has three tolerant-handling rules that are easy to get wrong:

- **BRule-4** — an UNPARSEABLE `<filename>.metadata.json` sidecar is ignored *entirely* and the document is STILL imported (with filename as title). A broken sidecar must never cost the user the document.
- **BRule-5** — a sidecar with NO matching sibling document is the ONLY entry-level rejection in the whole spec. Everything else imports.
- **BR-05** — no field is mandatory and there is NO schema validation. Unrecognised top-level keys are SILENTLY ignored (this is what protects internal system properties from being overwritten by a sender). Model healthData as an opaque passthrough subtree, not a rigid POJO, so extra languages/keys are not dropped.

Sidecar recognition (BR-03) is by exact name only: complete document filename incl. extension + lowercase `.metadata.json`, same folder. healthData is present when `documentTypes` contains `HEALTH` (BR-07).

Related: [[luz_docs_import adds document metadata only in DocsImportAsyncService.createDocument()]] [[luz_docs_import isExcludedFile misses Thumbs.db and __MACOSX (BR-04 gap)]]

## Related

- [[luz_docs_import adds document metadata only in DocsImportAsyncService.createDocument()]]
- [[luz_docs_import isExcludedFile misses Thumbs.db and __MACOSX (BR-04 gap)]]

%% ai-graph-start %%

**Related notes:**
- [[LUZ-158230 ePost ZIP import spec v1.0 (authoritative)]]
- [[LUZ-158230 QA edge-case decisions (ZIP import)]]
- [[ePost ZIP import (LUZ-158230) behavior rules confirmed by domain expert]]
- [[luz_docs_import adds document metadata only in DocsImportAsyncService.createDocument()]]
- [[luz_docs_import isExcludedFile misses Thumbs.db and __MACOSX (BR-04 gap)]]

**Relations:**
- Health ZIP import — *is* — broken
- sidecar — *still imports with* — Health ZIP import
- orphan sidecar — *is the only* — rejection
- ePost health ZIP import — *is also known as* — transfer.zip
- ePost health ZIP import — *is identified by* — LUZ-158230
- ePost health ZIP import — *has tolerant-handling rule* — BRule-4
- ePost health ZIP import — *has tolerant-handling rule* — BRule-5
- ePost health ZIP import — *has tolerant-handling rule* — BR-05
- BRule-4 — *handles* — UNPARSEABLE <filename>.metadata.json sidecar
- UNPARSEABLE <filename>.metadata.json sidecar — *is ignored* — entirely
- document — *is imported when sidecar is* — UNPARSEABLE <filename>.metadata.json sidecar
- document — *uses as title* — filename
- broken sidecar — *must not cost user the* — document
- BRule-5 — *defines the only* — entry-level rejection
- entry-level rejection — *is for* — sidecar with NO matching sibling document
- BR-05 — *states* — no field is mandatory
- BR-05 — *states* — NO schema validation
- BR-05 — *states* — Unrecognised top-level keys are SILENTLY ignored
- Unrecognised top-level keys — *protect* — internal system properties
- healthData — *is modeled as* — opaque passthrough subtree
- healthData — *is not* — rigid POJO
- Sidecar recognition — *is defined by* — BR-03
- Sidecar recognition — *is by* — exact name only
- exact name only — *includes* — complete document filename incl. extension + lowercase .metadata.json
- exact name only — *requires* — same folder
- healthData — *is present when* — documentTypes contains HEALTH
- BR-07 — *defines condition for* — healthData presence
- Health ZIP import — *is related to* — luz_docs_import adds document metadata only in DocsImportAsyncService.createDocument()
- luz_docs_import adds document metadata only in DocsImportAsyncService.createDocument() — *involves* — DocsImportAsyncService.createDocument()
- Health ZIP import — *is related to* — luz_docs_import isExcludedFile misses Thumbs.db and __MACOSX (BR-04 gap)
- luz_docs_import isExcludedFile misses Thumbs.db and __MACOSX (BR-04 gap) — *mentions* — Thumbs.db
- luz_docs_import isExcludedFile misses Thumbs.db and __MACOSX (BR-04 gap) — *mentions* — __MACOSX
- luz_docs_import isExcludedFile misses Thumbs.db and __MACOSX (BR-04 gap) — *identifies* — BR-04 gap

%% ai-graph-end %%