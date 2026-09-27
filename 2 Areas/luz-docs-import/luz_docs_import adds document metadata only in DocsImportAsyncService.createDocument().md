---
ai_hash: ca6ea5a5604153a4
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-04
entities:
- luz_docs_import
- DocsImportAsyncService
- DocsImportAsyncService.createDocument()
- MultipartFormDataOutput
- files
- folderIds
- metadata
- JSON string
- POST /documents
- luz_docs_view_controller
- LuzDocsViewControllerRestClient.createDocument
- origin
- ZIP-upload
- originCreatedDate
- originUpdatedDate
- documentTitle
- filename
- healthData
- senderName
- TenantId
- CompanyId
- documentTypes
- documentReferenceDate
- LUZ-158230
- health import
- document store
- Health ZIP import broken sidecar still imports; orphan sidecar is the only rejection
source: LUZ-158230 investigation 2026-08-04
status: seedling
tags:
- luz-docs-import
- architecture
- kepler
- metadata
title: luz_docs_import adds document metadata only in DocsImportAsyncService.createDocument()
type: observation
---

# luz_docs_import adds document metadata only in DocsImportAsyncService.createDocument()

In luz_docs_import, the ONLY place document metadata is assembled for a created document is DocsImportAsyncService.createDocument() (DocsImportAsyncService.java:406-428). It builds a MultipartFormDataOutput with parts `files`, `folderIds`, and a `metadata` JSON string, then POSTs to luz_docs_view_controller `POST /documents` (LuzDocsViewControllerRestClient.createDocument).

Today that `metadata` JSON carries only: `origin` ("ZIP-upload"), `originCreatedDate`, `originUpdatedDate`, and `documentTitle` (= filename without extension). To enrich a documents metadata (e.g. add healthData, senderName/TenantId/CompanyId, documentTypes, documentReferenceDate for the LUZ-158230 health import), this single method is the injection point — but the DOWNSTREAM view-controller + document store must also persist and return the new fields; luz_docs_import only forwards them.

**Why it matters:** one small, testable change point; and it makes the "no regression" guarantee easy — emit the extra fields only when a sidecar is present, otherwise the payload is byte-for-byte todays.

Related: [[Health ZIP import broken sidecar still imports; orphan sidecar is the only rejection]]

## Related

- [[Health ZIP import broken sidecar still imports; orphan sidecar is the only rejection]]

%% ai-graph-start %%

**Related notes:**
- [[Metadata sidecars must be scanned in luz-docs-import because they are never uploaded]]
- [[Health ZIP import broken sidecar still imports; orphan sidecar is the only rejection]]
- [[luz_docs_import health ZIP import uses a two-layer failure model]]
- [[luz-docs-import scans metadata sidecars per-file, not the whole ZIP]]
- [[luz_docs_import scope no sender auth, individual tenants, partial-import policy]]

**Relations:**
- luz_docs_import — *uses* — DocsImportAsyncService.createDocument()
- DocsImportAsyncService.createDocument() — *assembles* — MultipartFormDataOutput
- MultipartFormDataOutput — *includes part* — files
- MultipartFormDataOutput — *includes part* — folderIds
- MultipartFormDataOutput — *includes part* — metadata
- metadata — *is a* — JSON string
- DocsImportAsyncService.createDocument() — *POSTs to* — POST /documents
- POST /documents — *is handled by* — luz_docs_view_controller
- DocsImportAsyncService.createDocument() — *calls* — LuzDocsViewControllerRestClient.createDocument
- LuzDocsViewControllerRestClient.createDocument — *targets* — POST /documents
- metadata — *carries field* — origin
- origin — *has value* — ZIP-upload
- metadata — *carries field* — originCreatedDate
- metadata — *carries field* — originUpdatedDate
- metadata — *carries field* — documentTitle
- documentTitle — *derived from* — filename
- DocsImportAsyncService.createDocument() — *is injection point for* — healthData
- DocsImportAsyncService.createDocument() — *is injection point for* — senderName
- DocsImportAsyncService.createDocument() — *is injection point for* — TenantId
- DocsImportAsyncService.createDocument() — *is injection point for* — CompanyId
- DocsImportAsyncService.createDocument() — *is injection point for* — documentTypes
- DocsImportAsyncService.createDocument() — *is injection point for* — documentReferenceDate
- documentReferenceDate — *is for* — LUZ-158230
- LUZ-158230 — *is a type of* — health import
- luz_docs_import — *forwards* — healthData
- luz_docs_import — *forwards* — senderName
- luz_docs_import — *forwards* — TenantId
- luz_docs_import — *forwards* — CompanyId
- luz_docs_import — *forwards* — documentTypes
- luz_docs_import — *forwards* — documentReferenceDate
- luz_docs_view_controller — *must persist* — healthData
- luz_docs_view_controller — *must persist* — senderName
- luz_docs_view_controller — *must persist* — TenantId
- luz_docs_view_controller — *must persist* — CompanyId
- luz_docs_view_controller — *must persist* — documentTypes
- luz_docs_view_controller — *must persist* — documentReferenceDate
- luz_docs_view_controller — *must return* — healthData
- luz_docs_view_controller — *must return* — senderName
- luz_docs_view_controller — *must return* — TenantId
- luz_docs_view_controller — *must return* — CompanyId
- luz_docs_view_controller — *must return* — documentTypes
- luz_docs_view_controller — *must return* — documentReferenceDate
- document store — *must persist* — healthData
- document store — *must persist* — senderName
- document store — *must persist* — TenantId
- document store — *must persist* — CompanyId
- document store — *must persist* — documentTypes
- document store — *must persist* — documentReferenceDate
- document store — *must return* — healthData
- document store — *must return* — senderName
- document store — *must return* — TenantId
- document store — *must return* — CompanyId
- document store — *must return* — documentTypes
- document store — *must return* — documentReferenceDate
- luz_docs_import — *related to* — Health ZIP import broken sidecar still imports; orphan sidecar is the only rejection

%% ai-graph-end %%