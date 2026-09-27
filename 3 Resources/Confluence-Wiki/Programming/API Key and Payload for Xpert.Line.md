---
title: "API Key and Payload for Xpert.Line."
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47180283940/API+Key+and+Payload+for+Xpert.Line.
space: "TS"
topic: programming
relevance: 0.852
depth: 3
updated: 2024-05-20
attachments: 7
tags:
  - confluence
  - programming
  - space/ts
---

# API Key and Payload for Xpert.Line.

> [!info] Imported from Confluence
> Space **TS** · updated 2024-05-20 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47180283940/API+Key+and+Payload+for+Xpert.Line.)
> Relevance 0.852 · topic `programming`

### 1. Create the API KEY

- We need to create the API KEY from **KLARA BUSINESS AG** company

- Go to the tab “**Users**” and click the button “**Add new API key**”

- Add a new API KEY with the role “**Accountant**”.

- We will copy the API KEY and send it to Xpert.Line.


![[47180283940-image-20220912-024130.png]]

![[47180283940-image-20220912-024237.png]]



### 2. Payload to Xpert.Line

Example in DEV: <a href="https://api-dev.klara.tech/docs#/Store/post_core_latest_tenant_dunning" class="external-link" data-card-appearance="inline" rel="nofollow">https://api-dev.klara.tech/docs#/Store/post_core_latest_tenant_dunning</a>

Documentations: <a href="https://api-dev.klara.tech/docs" class="external-link" rel="nofollow">https://api.klara.ch/docs</a>

- URL: `https://api.klara.ch/core/latest/tenant-dunning`

- HTTP Method: `POST`

- Header: `X-API-KEY` → API KEY is provided by us

- Content-type: application/JSON

- Body:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="abd2fa1b-6191-42b2-aad0-281287547ad8" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
[
  {
    "amount": 1200,
    "dunningDate": "2022-01-19T10:10:50",
    "dunningLevel": 0,
    "id": 20
  },
  {
    "amount": 3400,
    "dunningDate": "2016-01-19T09:55:50",
    "dunningLevel": 2,
    "id": 30
  }
]
```

</div>

</div>

- **IMPORTANT: for each request, add only 1000 entities at most.**

- The “Id” of the request will be the “Id” of the table customer in public schema of **luz-store**

### 3. Example:

- Swagger in dev: <a href="https://api-dev.klara.tech/docs#/Store/post_core_latest_tenant_dunning" class="external-link" data-card-appearance="inline" rel="nofollow">https://api-dev.klara.tech/docs#/Store/post_core_latest_tenant_dunning</a>

<!-- -->

- Postman:


![[47180283940-image-20220912-044929.png]]

![[47180283940-image-20220912-045009.png]]
