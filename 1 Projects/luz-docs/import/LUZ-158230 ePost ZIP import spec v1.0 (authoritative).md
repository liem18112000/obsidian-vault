---
title: "LUZ-158230 ePost ZIP import spec v1.0 (authoritative)"
created: 2026-09-15
type: reference
status: evergreen
source: "ePost ZIP import spec v1.0 + Health documents handoff v1.0 (30.07.2026)"
tags: [luz-docs, luz_docs_import, earchive, zip-import, LUZ-158230, spec, source-of-truth]
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
