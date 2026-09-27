---
ai_hash: 75ecd516580b8a8c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-15
entities:
- LUZ-158230
- ePost ZIP import spec v1.0
- ePost
- Document transfer via ZIP — import specification
- Health documents in ePost — development handoff
- ZIP structure
- UTF-8
- Metadata file
- Document
- Metadata naming convention
- JSON
- Metadata format
- UUID
- ISO YYYY-MM-DD
- HEALTH (document type)
- EPDV-EDI Anhang 3
- FHIR
- DocumentEntry.classCode
- healthcareFacilityTypeCode
- practiceSettingCode
- Import limits
- ZIP file size
- Metadata file size
- Password-protected ZIP
- Encrypted ZIP
- Standard ZIP
- Import behavior
- luz_docs_import
- Client
- Health documents
- Branded folder
- eHealth SDK health-area
- Placeholder screen
- LUZ-158230 QA edge-case decisions (ZIP import)
- LUZ-158230 Post Health ZIP import v1 scope decisions
- Testing-Agent refine confidence is capped by un-ingested spec PDFs
- __MACOSX/
- .DS_Store
- Thumbs.db
- senderTenantId
- senderCompanyId
- senderName
- documentTitle
- documentTypes
- documentReferenceDate
- healthData
- documentCategory
- facility
- practiceSetting
source: ePost ZIP import spec v1.0 + Health documents handoff v1.0 (30.07.2026)
status: evergreen
tags:
- luz-docs
- luz_docs_import
- earchive
- zip-import
- LUZ-158230
- spec
- source-of-truth
title: LUZ-158230 ePost ZIP import spec v1.0 (authoritative)
type: reference
---

# LUZ-158230 ePost ZIP import spec v1.0 (authoritative)

The single source of truth for LUZ-158230 is TWO confirmed ePost documents (both **Version 1.0, 30.07.2026**): "Document transfer via ZIP — import specification" and "Health documents in ePost — development handoff". Where interrogation guesses conflicted with these, the SPEC WINS. Authoritative facts:

## ZIP structure
- Folders (incl. nested) recreated **1:1** in ePost. File/folder names **UTF-8**.
- **Any entry whose name starts with a dot is ignored** (plus `__MACOSX/`, `.DS_Store`, `Thumbs.db`).
- Metadata file must be in the **same folder** as its document.

## Metadata naming
- **Optional** per document. Name = `<complete document filename incl. extension>.metadata.json` (e.g. `lab-report.pdf` -> `lab-report.pdf.metadata.json`), exact match, lowercase postfix. Single UTF-8 JSON object. Not imported as a document.

## Metadata format (tolerant)
- **No schema validation; NO field is required.** Only allowed top-level elements stored; a **disallowed** top-level element (e.g. would overwrite an internal system property) is **silently ignored** (no error).
- Fields: `senderTenantId` (UUID), `senderCompanyId` (int, **always 1**), `senderName`, `documentTitle` (**fallback = filename without extension**), `documentTypes` (["HEALTH"]), `documentReferenceDate` (ISO YYYY-MM-DD), `healthData{author, documentCategory, facility, practiceSetting}`.
- Coded value = `{code, de, fr, it, en}`; **at least `en` RECOMMENDED, not required**. Value sets from **EPDV-EDI Anhang 3**, FHIR-mapped: documentCategory->DocumentEntry.classCode, facility->healthcareFacilityTypeCode, practiceSetting->practiceSettingCode.

## Limits
- **ZIP file size < 2 GB** (the zip file itself; exactly 2GB rejected).
- **Metadata file <= 100 KB** each.
- **Password-protected / encrypted ZIP => rejected.**
- Standard ZIP, UTF-8 names. (NO spec per-binary 200MB limit; NO zip-bomb/ratio guard; limit is on the zip file size, not uncompressed.)

## Import behavior (per-file; one faulty entry does NOT fail the transfer)
- valid metadata -> imported WITH metadata
- no metadata -> imported WITHOUT health metadata
- disallowed top-level element -> imported, element ignored, no error
- **metadata not parseable -> document STILL imported, metadata ignored** (not a failure)
- **metadata without matching document -> REJECTED, reported as failed**
- **document already in target folder with SAME NAME AND SAME SIZE -> SKIPPED, existing kept** (this is the dedup rule)

## Downstream / apps (handoff)
- luz_docs_import persists healthData + documentTypes/senderName/senderTenantId/senderCompanyId/documentTitle/documentReferenceDate and returns them to clients. Doc is a "health document" when documentTypes contains HEALTH. Health docs shown in a **branded folder filtered by senderTenantId** (doc-type-triggered branded folder = TBD extension). eHealth SDK health-area replaced by a placeholder screen by end-2026.

Related: [[LUZ-158230 QA edge-case decisions (ZIP import)]], [[LUZ-158230 Post Health ZIP import v1 scope decisions]], [[Testing-Agent refine confidence is capped by un-ingested spec PDFs]]

## Related

- [[LUZ-158230 QA edge-case decisions (ZIP import)]]
- [[LUZ-158230 Post Health ZIP import v1 scope decisions]]

%% ai-graph-start %%

**Related notes:**
- [[LUZ-158230 QA edge-case decisions (ZIP import)]]
- [[ePost Health document app UI mapping and import behavior]]
- [[ePost ZIP import (LUZ-158230) behavior rules confirmed by domain expert]]
- [[LUZ-158230 Post Health ZIP import v1 scope decisions]]
- [[LUZ-158230 is the ePost eArchive transfer.zip document-import feature]]

**Relations:**
- LUZ-158230 — *is_defined_by* — ePost ZIP import spec v1.0
- ePost ZIP import spec v1.0 — *is_authoritative_for* — LUZ-158230
- ePost ZIP import spec v1.0 — *has_version* — 1.0
- ePost ZIP import spec v1.0 — *has_date* — 30.07.2026
- ePost ZIP import spec v1.0 — *is_based_on* — Document transfer via ZIP — import specification
- ePost ZIP import spec v1.0 — *is_based_on* — Health documents in ePost — development handoff
- Document transfer via ZIP — import specification — *has_version* — 1.0
- Document transfer via ZIP — import specification — *has_date* — 30.07.2026
- Health documents in ePost — development handoff — *has_version* — 1.0
- Health documents in ePost — development handoff — *has_date* — 30.07.2026
- ePost ZIP import spec v1.0 — *specifies* — ZIP structure
- ePost ZIP import spec v1.0 — *specifies* — Metadata naming convention
- ePost ZIP import spec v1.0 — *specifies* — Metadata format
- ePost ZIP import spec v1.0 — *specifies* — Import limits
- ePost ZIP import spec v1.0 — *specifies* — Import behavior
- ePost ZIP import spec v1.0 — *specifies* — Downstream / apps
- ZIP structure — *recreates_folders_in* — ePost
- ZIP structure — *uses_encoding_for_names* — UTF-8
- ZIP structure — *ignores_entry_starting_with* — .
- ZIP structure — *ignores_path* — __MACOSX/
- ZIP structure — *ignores_file* — .DS_Store
- ZIP structure — *ignores_file* — Thumbs.db
- Metadata file — *must_be_in_same_folder_as* — Document
- Metadata naming convention — *is_optional_for* — Document
- Metadata naming convention — *defines_format* — <complete document filename incl. extension>.metadata.json
- Metadata naming convention — *is_a* — JSON
- Metadata naming convention — *uses_encoding* — UTF-8
- Metadata file — *is_not_imported_as_a* — Document
- Metadata format — *has_no* — schema validation
- Metadata format — *allows_field* — senderTenantId
- Metadata format — *allows_field* — senderCompanyId
- Metadata format — *allows_field* — senderName
- Metadata format — *allows_field* — documentTitle
- Metadata format — *allows_field* — documentTypes
- Metadata format — *allows_field* — documentReferenceDate
- Metadata format — *allows_field* — healthData
- senderTenantId — *is_type* — UUID
- senderCompanyId — *has_value* — 1
- documentTitle — *has_fallback* — filename without extension
- documentTypes — *can_include* — HEALTH (document type)
- documentReferenceDate — *has_format* — ISO YYYY-MM-DD
- healthData — *includes* — author
- healthData — *includes* — documentCategory
- healthData — *includes* — facility
- healthData — *includes* — practiceSetting
- Metadata format — *uses_value_sets_from* — EPDV-EDI Anhang 3
- documentCategory — *is_FHIR_mapped_to* — DocumentEntry.classCode
- facility — *is_FHIR_mapped_to* — healthcareFacilityTypeCode
- practiceSetting — *is_FHIR_mapped_to* — practiceSettingCode
- Import limits — *sets_max_size_for* — ZIP file size
- ZIP file size — *is_less_than* — 2 GB
- Import limits — *sets_max_size_for* — Metadata file size
- Metadata file size — *is_less_than_or_equal_to* — 100 KB
- Import limits — *rejects* — Password-protected ZIP
- Import limits — *rejects* — Encrypted ZIP
- Import limits — *requires* — Standard ZIP
- Standard ZIP — *uses_names_encoded_as* — UTF-8
- Import behavior — *imports_with_metadata_if* — valid metadata
- Import behavior — *imports_without_health_metadata_if* — no metadata
- Import behavior — *imports_and_ignores_element_if* — disallowed top-level element
- Import behavior — *imports_and_ignores_metadata_if* — metadata not parseable
- Import behavior — *rejects_if* — metadata without matching document
- Import behavior — *skips_if* — document already in target folder with SAME NAME AND SAME SIZE
- luz_docs_import — *persists* — healthData
- luz_docs_import — *persists* — documentTypes
- luz_docs_import — *persists* — senderName
- luz_docs_import — *persists* — senderTenantId
- luz_docs_import — *persists* — senderCompanyId
- luz_docs_import — *persists* — documentTitle
- luz_docs_import — *persists* — documentReferenceDate
- luz_docs_import — *returns_data_to* — Client
- Document — *is_a_health_document_when_documentTypes_contains* — HEALTH (document type)
- Health documents — *are_shown_in* — Branded folder
- Branded folder — *is_filtered_by* — senderTenantId
- eHealth SDK health-area — *will_be_replaced_by* — Placeholder screen
- eHealth SDK health-area — *replacement_date* — end-2026
- LUZ-158230 — *is_related_to* — LUZ-158230 QA edge-case decisions (ZIP import)
- LUZ-158230 — *is_related_to* — LUZ-158230 Post Health ZIP import v1 scope decisions
- LUZ-158230 — *is_related_to* — Testing-Agent refine confidence is capped by un-ingested spec PDFs

%% ai-graph-end %%