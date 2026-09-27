---
ai_hash: d2ad403a4a523c9e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
entities:
- document-put-cascade
- Document PUT Cascade
- DocumentResource.updateDocumentMetadata
- Java @PUT annotation
- Java @Path annotation
- Response
- documentService
- credentialToken
- tenantId
- documentId
- documents
- DocumentResource.java
- PUT /{tenantId}/documents/{document-id}
- document
- materialized technical fields
- API Entry Point
- Service Validation
- Cascade Decision Gate
- Materialized Fields Computation
- Save and Side Effects
- Failure Paths
- Files of Record
- Glossary for Newbies
- DocumentService.updateDocumentMetadata
- REST entry point
- logic
- full document metadata PUT
- service
- request
- folderIds
- user edits
- technical _... fields
- materialized fields
- tenant
- allowlisted
- securityClassCode
- _folderNames
- _effectiveSecurityClassCodes
- _isPublic
- Folder names
- document security codes
- security codes from folders
- folder rename cascade
- synchronous
- derived fields
- diagrams/01-put-flow.png
- note set
---

# Document PUT Cascade

Source endpoint:

```java
@PUT
@Path("/{document-id}")
public Response updateDocumentMetadata(...) {
    documentService.updateDocumentMetadata(credentialToken, tenantId, documentId, documents);
    return Response.ok().build();
}
```

Code location:

`C:\Users\dvtliem\Kepler\luz_docs\src\main\java\ch\klara\luz\docs\rest\DocumentResource.java`

## Start here

This note set explains how `PUT /{tenantId}/documents/{document-id}` updates a document and, when needed, recomputes the materialized technical fields.

Read in this order:

1. [[01 API Entry Point|API entry point]]
2. [[02 Service Validation|Service validation]]
3. [[03 Cascade Decision Gate|Cascade decision gate]]
4. [[04 Materialized Fields Computation|Materialized fields computation]]
5. [[05 Save and Side Effects|Save and side effects]]
6. [[06 Failure Paths|Failure paths]]
7. [[07 Files of Record|Files of record]]
8. [[08 Glossary for Newbies|Glossary for newbies]]

## TL;DR

`DocumentResource.updateDocumentMetadata` is only the REST entry point. The real logic is in `DocumentService.updateDocumentMetadata`.

On a full document metadata `PUT`, the service validates the request, deduplicates and checks `folderIds`, blocks user edits to technical `_...` fields, then decides whether materialized fields must be recomputed.

If the tenant is allowlisted and either `folderIds` or `securityClassCode` changed, the code recomputes:

| Field | Meaning |
|---|---|
| `_folderNames` | Folder names in the same order as `folderIds`. |
| `_effectiveSecurityClassCodes` | Union of document security codes plus security codes from folders. |
| `_isPublic` | Whether the document should be considered public. |

Unlike folder rename cascade, this `PUT` path is **synchronous**: the derived fields are computed before the document is saved.

## Overview diagram

![[diagrams/01-put-flow.png]]

%% ai-graph-start %%

**Related notes:**
- [[01 API Entry Point]]
- [[luz_docs folder security-class changes have 3 entry points but only PUT cascades]]
- [[03 Cascade Decision Gate]]
- [[08 Glossary for Newbies]]
- [[02 Service Validation]]

**Relations:**
- document-put-cascade — *is a* — Document PUT Cascade
- DocumentResource.updateDocumentMetadata — *is annotated with* — Java @PUT annotation
- DocumentResource.updateDocumentMetadata — *is annotated with* — Java @Path annotation
- Java @Path annotation — *has path* — /{document-id}
- DocumentResource.updateDocumentMetadata — *returns* — Response
- DocumentResource.updateDocumentMetadata — *calls* — documentService.updateDocumentMetadata
- documentService.updateDocumentMetadata — *takes parameter* — credentialToken
- documentService.updateDocumentMetadata — *takes parameter* — tenantId
- documentService.updateDocumentMetadata — *takes parameter* — documentId
- documentService.updateDocumentMetadata — *takes parameter* — documents
- DocumentResource.java — *contains* — DocumentResource.updateDocumentMetadata
- PUT /{tenantId}/documents/{document-id} — *updates* — document
- PUT /{tenantId}/documents/{document-id} — *recomputes* — materialized technical fields
- note set — *explains* — PUT /{tenantId}/documents/{document-id}
- note set — *explains* — materialized technical fields
- note set — *includes* — API Entry Point
- note set — *includes* — Service Validation
- note set — *includes* — Cascade Decision Gate
- note set — *includes* — Materialized Fields Computation
- note set — *includes* — Save and Side Effects
- note set — *includes* — Failure Paths
- note set — *includes* — Files of Record
- note set — *includes* — Glossary for Newbies
- DocumentResource.updateDocumentMetadata — *is a* — REST entry point
- DocumentService.updateDocumentMetadata — *contains* — logic
- full document metadata PUT — *involves* — service
- service — *validates* — request
- service — *deduplicates* — folderIds
- service — *checks* — folderIds
- service — *blocks* — user edits
- user edits — *to* — technical _... fields
- service — *decides recomputation of* — materialized fields
- materialized fields — *are recomputed if* — tenant
- tenant — *is* — allowlisted
- materialized fields — *are recomputed if* — folderIds
- folderIds — *changed* — true
- materialized fields — *are recomputed if* — securityClassCode
- securityClassCode — *changed* — true
- _folderNames — *is a* — derived field
- _folderNames — *represents* — Folder names
- _folderNames — *is ordered by* — folderIds
- _effectiveSecurityClassCodes — *is a* — derived field
- _effectiveSecurityClassCodes — *represents* — Union of document security codes plus security codes from folders
- _isPublic — *is a* — derived field
- _isPublic — *represents* — Whether the document should be considered public
- derived fields — *computation is* — synchronous
- derived fields — *are computed before* — document
- document — *is saved* — true
- diagrams/01-put-flow.png — *is an* — Overview diagram
- document — *has* — security codes
- folders — *have* — security codes
- DocumentResource.updateDocumentMetadata — *is located at* — DocumentResource.java
- DocumentService.updateDocumentMetadata — *is the real* — logic
- service — *refers to* — DocumentService.updateDocumentMetadata
- documentService — *is a* — service
- DocumentResource.updateDocumentMetadata — *is the* — Source endpoint

%% ai-graph-end %%