---
title: "Call Ivy API at local"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TK/pages/47975891549/Call+Ivy+API+at+local
space: "TK"
topic: programming
relevance: 0.731
depth: 2.73
updated: 2024-08-12
attachments: 5
tags:
  - confluence
  - programming
  - space/tk
---

# Call Ivy API at local

> [!info] Imported from Confluence
> Space **TK** · updated 2024-08-12 · [open original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/47975891549/Call+Ivy+API+at+local)
> Relevance 0.731 · topic `programming`

# I. Call via Postman

1.  Start IVY at local

2.  Open Postman

3.  Call API

    1.  GET - localhost:8081/designer/faces/api/{{tenantId}}/companies/1/documents

4.  Now call the API you want

# I. Call via another service

1.  Start IVY at local

2.  Go to the service that you want to call IVY - ex: luz-store

3.  Go to IVY rest client

    1.  

![[47975891549-image-20240812-022732.png]]



4.  Add a cookie header at the API you want to call

    1.  

![[47975891549-image-20240812-022919.png]]



5.  Open Postman

6.  Call API

    1.  GET - localhost:8081/designer/faces/api/{{tenantId}}/companies/1/documents

7.  Copy the JSESSION cookie from Postman

    1.  

![[47975891549-image-20240812-030352.png]]


    2.  

![[47975891549-image-20240812-030426.png]]



8.  Back to the service and paste the header to the rest client method

    1.  

![[47975891549-image-20240812-030523.png]]



9.  DONE
