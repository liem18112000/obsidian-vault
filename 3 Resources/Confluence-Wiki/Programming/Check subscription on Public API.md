---
ai_hash: 513cc87a7fc29842
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 3
depth: 2.57
entities: []
relevance: 0.746
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/47219245151/Check+subscription+on+Public+API
space: Helios
status: reference
tags:
- confluence
- programming
- space/helios
title: Check subscription on Public API
topic: programming
type: source
updated: 2022-11-22
---

# Check subscription on Public API

> [!info] Imported from Confluence
> Space **Helios** · updated 2022-11-22 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/47219245151/Check+subscription+on+Public+API)
> Relevance 0.746 · topic `programming`

- **luz_eletter:** checking subscription in filter request.


![[47219245151-image-20221122-070557.png]]



For every request on luz_eletter, they checking the required widget code by using the annotation `@AccessibleWithSubscription`. This annotation will define required widget code need to be use for requests.


![[47219245151-image-20221122-071415.png]]



- **luz_public_api_adapter:** checking subscription in filter request.


![[47219245151-image-20221122-070639.png]]



For every request on luz_public_api_adapter, they checking the required widget code need to be pass by check request path, then forward request to other modules

%% ai-graph-start %%

**Related notes:**
- [[Apply OpenAPI and try out API on SwaggerUI]]
- [[Roles and Permissions check for accessing public API]]
- [[Adding filter - public api adapter]]
- [[Public API Eletter (0.02.09.00)]]
- [[One API (06.12.2022 - 19.12.2022)]]

%% ai-graph-end %%