---
ai_hash: fb79d1cfc958e248
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 2
depth: 3
entities: []
relevance: 0.891
source: https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/6898253602/Swagger+with+api+explorer
space: Arrow
status: reference
tags:
- confluence
- programming
- space/arrow
title: Swagger with api explorer
topic: programming
type: source
updated: 2020-04-27
---

# Swagger with api explorer

> [!info] Imported from Confluence
> Space **Arrow** · updated 2020-04-27 · [open original](https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/6898253602/Swagger+with+api+explorer)
> Relevance 0.891 · topic `programming`

1.Access: <a href="http://localhost:8080/luz_api_explore" class="external-link" rel="nofollow">http://localhost:8080/luz_api_explore</a>

2.Pass project's swagger link: <a href="http://localhost:8080/luz_compensation/api/swagger.json" class="external-link" rel="nofollow">http://localhost:8080/<strong>luz_compensation</strong>/api/swagger.json</a>

3\. For token:

3.1. On server (LOCAL, DEV, STAGING)

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="7ba9cd4d-4302-4f44-887e-89468fa8a3a2" macro-name="view-file"><a href="../_attachments/6898253602-refreshtoken.postman_collection.json" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/6898253602/refreshtoken.postman_collection.json?version=1&amp;modificationDate=1587702165000&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/octet-stream" data-has-thumbnail="true">

![[6898253602-refreshtoken.postman_collection.json]]

</a></span>

3.1.1. Call refresh token grant inital. (enter your username and password in body)

3.1.2. Call tenant-specific access_token

3.2. On Local

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="75555bb2-a986-4403-b47c-996aa5425411" macro-name="view-file"><a href="../_attachments/6898253602-LoginFlow.postman_collection.json" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/6898253602/LoginFlow.postman_collection.json?version=1&amp;modificationDate=1587957811000&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/octet-stream" data-has-thumbnail="true">

![[6898253602-LoginFlow.postman_collection.json]]

</a></span>

3.2.1. Get generic token (Basic Auth)

3.2.2. Get full token (Basic Auth)

%% ai-graph-start %%

**Related notes:**
- [[How to consume luz api]]
- [[HowToUseNewTokenAPI]]
- [[OpenAPI UI (API on SwaggerUI)]]
- [[Swagger UI]]
- [[Token JWT Security]]

%% ai-graph-end %%