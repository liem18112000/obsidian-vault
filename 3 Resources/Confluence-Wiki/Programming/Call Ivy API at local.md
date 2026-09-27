---
ai_hash: 727b6911035fc69b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 5
depth: 2.73
entities: []
relevance: 0.731
source: https://axonivy.atlassian.net/wiki/spaces/TK/pages/47975891549/Call+Ivy+API+at+local
space: TK
status: reference
tags:
- confluence
- programming
- space/tk
title: Call Ivy API at local
topic: programming
type: source
updated: 2024-08-12
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

%% ai-graph-start %%

**Related notes:**
- [[Upload Document API]]
- [[IVY API Calls Overview for luz Modules]]
- [[REST API for deleting EXPENSES documents]]
- [[Swagger with api explorer]]
- [[Temporary Restfull APIs in Ivy]]

%% ai-graph-end %%