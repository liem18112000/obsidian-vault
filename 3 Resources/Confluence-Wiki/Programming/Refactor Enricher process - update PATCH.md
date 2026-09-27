---
title: "Refactor Enricher process - update PATCH"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48208544163/Refactor+Enricher+process+-+update+PATCH
space: "LUZ"
topic: programming
relevance: 0.907
depth: 3
updated: 2024-12-16
attachments: 7
tags:
  - confluence
  - programming
  - space/luz
---

# Refactor Enricher process - update PATCH

> [!info] Imported from Confluence
> Space **LUZ** · updated 2024-12-16 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48208544163/Refactor+Enricher+process+-+update+PATCH)
> Relevance 0.907 · topic `programming`

Comparation between two implementation:

- Using PUT update method  
  **Endpoint**: `http://luz-jsonstore:8080/luz_jsonstore/api/mdb/{tenant-id}/{collection}/{doc-id}/replace`


![[48208544163-Enrichment process call jsonstore PUT method.png]]




![[48208544163-image-20241216-065554.png]]



**Total call for enrichment process:** 11 calls  
- 8 calls to get latest document metadata  
- 1 call to update (PUT) metadata (1st Phase: Thumbnail, Timestamp, ContentType)  
- 1 call to update (PUT) metadata (2nd Phase: AI analyze)  
- 1 call to update (PATCH) enricher status

- Using PATCH update method  
  **Endpoint**: `http://luz-jsonstore:8080/luz_jsonstore/api/mdb/{tenant-id}/{collection}/{doc-id}`


![[48208544163-Enrichment process call jsonstore to update with PATCH method.png]]

![[48208544163-image-20241216-070150.png]]



**Total call for enrichment process:** 2 calls

- 1 call to update (PATCH) metadata (1st Phase: Thumbnail, Timestamp, ContentType)

<!-- -->

- 1 call to update (PATCH) metadata (2nd Phase: AI analyze, also include enricher status)
