---
title: "Check subscription on Public API"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/47219245151/Check+subscription+on+Public+API
space: "Helios"
topic: programming
relevance: 0.746
depth: 2.57
updated: 2022-11-22
attachments: 3
tags:
  - confluence
  - programming
  - space/helios
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
