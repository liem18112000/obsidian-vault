---
ai_hash: 78a42c79d3970f6e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-13
entities: []
source: spec-example sidecar + fix-plan PR-A 2026-08-13
status: seedling
tags:
- luz-docs-import
- ePost
- metadata
- schema
- LUZ-158230
title: ePost ZIP-import metadata sidecar field types (senderCompanyId is numeric)
type: reference
---

# ePost ZIP-import metadata sidecar field types (senderCompanyId is numeric)

The ePost ZIP-import `.metadata.json` sidecar has these top-level fields with FIXED JSON types (confirmed from the spec-example sidecar in `01-happy-spec-example.zip`):

| field | JSON type | example |
|-------|-----------|---------|
| `senderTenantId` | string (UUID) | `"9b4f6bb9-…"` |
| `senderCompanyId` | **number** (int) | `1` |
| `senderName` | string | `"Post Health"` |
| `documentTitle` | string | `"Meine Patientenverfuegung 001"` |
| `documentTypes` | **array** of string | `["HEALTH"]` |
| `documentReferenceDate` | string (date) | `"2026-06-22"` |
| `healthData` | object (author + coded documentCategory/facility/practiceSetting) | — |

**Gotcha:** `senderCompanyId` is an integer, NOT a string (company ids are numeric across Klara — the JWT carries `companyId:1`). A sidecar sending it as a string is the "wrong type" the fixture-12 test flags. `documentTypes` must be an array; a bare string is wrong.

**Design decision (F6 fix):** in `HealthDocImporter.applyMetadata` each allowed top-level field is forwarded only when its `JsonValue.ValueType` matches the expected type above; a mismatched field is skipped (spec §4: fields are not schema-validated → ignore a bad field, do not abort the document create). Mirrors the already-defensive `HealthData.fromJson` instanceof guards.

## Related

- [[Run volume import fixtures last; retry-exhaustion is transient saturation not a defect]]

%% ai-graph-start %%

**Related notes:**
- [[Health ZIP import broken sidecar still imports; orphan sidecar is the only rejection]]
- [[LUZ-158230 ePost ZIP import spec v1.0 (authoritative)]]
- [[luz_docs_import adds document metadata only in DocsImportAsyncService.createDocument()]]
- [[ePost Health documents ZIP import business context]]
- [[HEALTH document type carries verbatim SNOMED healthData]]

%% ai-graph-end %%