---
ai_hash: 7f00897c7d2916ea
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-10
entities:
- luz-docs folder delete
- document security classes
- Mongo query
- folder
- Java
- verifySecurityClasses
- jsonStoreMongoService.countCollections
- $match
- $count
- filter
- result.isEmpty()
- securityClassCodes
- inheritedSecurityClassCodes
- tenant's classes
- DocumentMismatchSecurityClassCodeException
- CollectionUtil.isMatchingSecurityClasses
- _deletionStatus
- buildDeletionStatusCondition
- getAllDocumentsMetadata
- luz-docs folder delete filter double-fetched every subfolder
- Split bulk scans on folderIds.1 exists to separate single-array-element fast path
- folder-delete filter
- document
source: session 2026-06-10, FolderUtil.verifyFolderDocumentsSecurityClasses
status: seedling
tags:
- luz-docs
- mongodb
- security-classes
- performance
title: luz-docs folder delete verifies document security classes with one limit-1
  Mongo query per folder
type: lesson
---

# luz-docs folder delete verifies document security classes with one limit-1 Mongo query per folder

Instead of fetching every document in a folder and calling `verifySecurityClasses` per document in Java, the folder-delete filter now runs ONE count query per folder (jsonStoreMongoService.countCollections -> [{$match: filter}, {$count: ...}]) that searches for any violating document. Mongo $count emits NO row when nothing matches, so an empty result array means no violators and any row means at least one — check result.isEmpty(), do not parse the number. A document violates when its combined (`securityClassCodes` + `inheritedSecurityClassCodes`) is non-empty AND shares no element with the tenant's classes. As a find filter:

- `securityClassCodes: {\: {\: tenantClasses}}` — array shares nothing with tenant (also matches missing/empty field, which is why the next clause exists)
- same for `inheritedSecurityClassCodes`
- `\: [{"securityClassCodes.0": {\: true}}, {"inheritedSecurityClassCodes.0": {\: true}}]` — combined non-empty; without this, class-less (public) documents would falsely violate

If the query returns anything → throw `DocumentMismatchSecurityClassCodeException`, aborting the whole delete — exactly the old Java semantics of `CollectionUtil.isMatchingSecurityClasses`. Empty tenant list works too: `\: []` matches nothing, so \ matches all → any classed doc violates, mirroring Java.

Caveat: `_deletionStatus` is matched with strict equality `"false"` (docs missing the field are excluded) to mirror the old `getAllDocumentsMetadata` criteria — do NOT swap in `buildDeletionStatusCondition`, whose \ treats a missing field as non-deleted.

Context: [[luz-docs folder delete filter double-fetched every subfolder]], [[Split bulk scans on folderIds.1 exists to separate single-array-element fast path]]

## Related

- [[luz-docs folder delete filter double-fetched every subfolder]]
- [[Split bulk scans on folderIds.1 exists to separate single-array-element fast path]]

%% ai-graph-start %%

**Related notes:**
- [[Validate with a count query for violators instead of loading all documents]]
- [[getCollectionMetadataByTerms silently ignored includeDeletedRecord for non-document collections]]
- [[Split bulk scans on folderIds.1 exists to separate single-array-element fast path]]
- [[luz-docs folder delete filter double-fetched every subfolder]]
- [[Luz delete-folder tests can only delete public folders, not ones carrying a security class]]

**Relations:**
- luz-docs folder delete — *verifies* — document security classes
- luz-docs folder delete — *uses* — Mongo query
- Mongo query — *is per* — folder
- Mongo query — *is* — limit-1
- folder-delete filter — *runs* — jsonStoreMongoService.countCollections
- jsonStoreMongoService.countCollections — *uses* — $match
- jsonStoreMongoService.countCollections — *uses* — $count
- document — *violates if* — combined (securityClassCodes + inheritedSecurityClassCodes) is non-empty AND shares no element with tenant's classes
- query — *throws* — DocumentMismatchSecurityClassCodeException
- DocumentMismatchSecurityClassCodeException — *aborts* — delete
- CollectionUtil.isMatchingSecurityClasses — *has semantics of* — old Java
- _deletionStatus — *is matched with* — strict equality 'false'
- _deletionStatus — *mirrors criteria of* — getAllDocumentsMetadata
- buildDeletionStatusCondition — *treats missing field as* — non-deleted
- luz-docs folder delete — *is related to* — luz-docs folder delete filter double-fetched every subfolder
- luz-docs folder delete — *is related to* — Split bulk scans on folderIds.1 exists to separate single-array-element fast path
- folder-delete filter — *is part of* — luz-docs folder delete
- Java — *calls* — verifySecurityClasses
- result.isEmpty() — *indicates* — no violators
- securityClassCodes — *are compared with* — tenant's classes
- inheritedSecurityClassCodes — *are compared with* — tenant's classes
- jsonStoreMongoService.countCollections — *takes* — filter

%% ai-graph-end %%