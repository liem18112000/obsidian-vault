---
ai_hash: e2943a1ff547f340
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.81
entities: []
relevance: 0.724
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20436034394/HowToUseNewTokenAPI
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: HowToUseNewTokenAPI
topic: programming
type: source
updated: 2016-10-26
---

# HowToUseNewTokenAPI

> [!info] Imported from Confluence
> Space **LUZ** · updated 2016-10-26 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20436034394/HowToUseNewTokenAPI)
> Relevance 0.724 · topic `programming`

This page demonstrate how a client should do to access the RESTful API with the new security framework.

### 1 - Obtain the authenticated (generic) token

The client have to obtain the token

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a8b371fe-02e3-428c-a72a-c80516315508" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
POST /luzsec/api/tokens
Location: http://localhost:8080
Authorization: Basic XXXXYYYY
 
 
 
HTTP 1.1 200 OK
 
{
    "token" : "XXXYYYZZZZ",
    "publicKey" : "23ASdf2342..."
}
```

</div>

</div>

### 2 - Create the tenant

Use the `token` above for authorization.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="6c01a61d-9f86-4432-9f5a-26ec0e0a5846" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
POST /luztenant/api/{username}/tenants
Location: http://localhost:8080
Authorization: Bearer XXXYYYZZZZ
 
 
HTTP/1.1 200 OK
 
[
  {
    "id": 1,
    "roles": [
      "tenant-creator"
    ],
    "tenantId": "883c804a-cde7-4834-8bce-49251fc1e6a0",
    "username": "admin",
    "type": "PERSON"
  },
  {
    "id": 2,
    "roles": [
      "tenant-creator"
    ],
    "tenantId": "0d633699-1ef3-40fc-9990-f3cb28cf70ba",
    "username": "admin",
    "type": "COMPANY"
  }
]
```

</div>

</div>

### 3 - Obtain the tenant token

Use the `tenantId` from above (type `COMPANY`).

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5fa107cb-f544-4f72-9061-80967e6cb7a8" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
POST /luzsec/api/0d633699-1ef3-40fc-9990-f3cb28cf70ba/tokens
Location: http://localhost:8080
Authorization: Basic XXXXYYYY
 
 
HTTP 1.1 200 OK

{
    "token" : "XXXYYYZZZZ",
    "publicKey" : "23ASdf2342..."
}
```

</div>

</div>

### 4 - Use the new token for accessing other resource.

%% ai-graph-start %%

**Related notes:**
- [[Token JWT Security]]
- [[How to consume luz api]]
- [[Swagger with api explorer]]
- [[Luz 403 Not allowed means the token has no tenant claim, not a missing permission]]
- [[Upload Document API]]

%% ai-graph-end %%