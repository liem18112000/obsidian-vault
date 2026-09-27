---
title: "luz_cache APIs"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/47220458629/luz_cache+APIs
space: "Helios"
topic: infra
relevance: 0.75
depth: 3
updated: 2022-11-24
attachments: 10
tags:
  - confluence
  - infra
  - space/helios
---

# luz_cache APIs

> [!info] Imported from Confluence
> Space **Helios** · updated 2022-11-24 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/47220458629/luz_cache+APIs)
> Relevance 0.75 · topic `infra`

**For Tenant specific APIs:** we have 4 APIs


![[47220458629-image-20221124-050127.png]]



**PUT method /{key}:** to save cache

- Authorization: we use access token to call this API

- tenant_id: specific tenant want to store

- key: special string in path request to access cache (must not duplicate)

- keyValueObject:

  

![[47220458629-image-20221124-042309.png]]



  - key: can set empty because luz_cache will build a private key for each request

  - 

![[47220458629-image-20221124-043033.png]]



    value: data to be stored in luz_cache

  - expiration: define time cache will be expired. Have 2 option as below.  


![[47220458629-image-20221124-043325.png]]



**PUT method /{key}/expire:** to set expire time for cache


![[47220458629-image-20221124-045814.png]]



**GET and DELETE method:**

- Authorization: we use access token to call this API

- tenant_id: specific tenant want to store

- key: input special string had stored on cache to get data or delete

**For Public schema APIs:** we also have 4 APIs


![[47220458629-image-20221124-050044.png]]



**For User Specific Cache:** support same as Tenant Specific Cache, input one more username information in path parameter to use this cache


![[47220458629-image-20221124-050544.png]]
