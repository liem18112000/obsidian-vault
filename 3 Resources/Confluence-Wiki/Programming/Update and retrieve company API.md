---
ai_hash: 7304aef3072a5107
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 1
depth: 3
entities: []
relevance: 0.786
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/48569811582/Update+and+retrieve+company+API
space: TS
status: reference
tags:
- confluence
- programming
- space/ts
title: Update and retrieve company API
topic: programming
type: source
updated: 2025-08-04
---

# Update and retrieve company API

> [!info] Imported from Confluence
> Space **TS** · updated 2025-08-04 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/48569811582/Update+and+retrieve+company+API)
> Relevance 0.786 · topic `programming`

**Update company info**

To retrieve and update company info, we reused some APIs that were originally created for the Klara website.

PATCH:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="cfdf42d6-0988-4c61-8bab-fdfd817e2f86" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
luz_compensation/api/{{tenant-id}}/companies/1
```

</div>

</div>

GET:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a4ee77c6-baca-44ef-bcd7-9056a7204f50" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
luz_compensation/api/{{tenant-id}}/companies/1
```

</div>

</div>

Postman collection

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="b1801e79-ff27-480d-99a0-770e20f03843" macro-name="view-file"><a href="../_attachments/48569811582-company-section.postman_collection.json" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/48569811582/company-section.postman_collection.json?version=1&amp;modificationDate=1751935339594&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/json" data-has-thumbnail="true">

![[48569811582-company-section.postman_collection.json]]

</a></span>

We use these APIs to fetch company info and update certain fields such as the company name, company emails and company phones.

The API for updating company information can also update location information. However, since locations are stored as an array, updating a specific element in the array requires passing the correct element position (as required by @PATCH and JSON PATCH). This approach can introduce many risks.

So, we decided to create a new API for updating location information.

(Will be updated when new API done)

%% ai-graph-start %%

**Related notes:**
- [[Luz_google Api Document]]
- [[LUZ-109076 Public API - Create update new tenant (implementation)]]
- [[Places Hubspot features have been applying to]]
- [[15. Update companies by tenant id]]
- [[RESTful API, Postman,]]

%% ai-graph-end %%