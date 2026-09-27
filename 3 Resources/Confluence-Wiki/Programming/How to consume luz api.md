---
ai_hash: e2f824d2dd0ae106
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 10
depth: 2.73
entities: []
relevance: 0.731
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38184589400/How+to+consume+luz+api
space: Helios
status: reference
tags:
- confluence
- programming
- space/helios
title: How to consume luz api
topic: programming
type: source
updated: 2019-01-09
---

# How to consume luz api

> [!info] Imported from Confluence
> Space **Helios** · updated 2019-01-09 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38184589400/How+to+consume+luz+api)
> Relevance 0.731 · topic `programming`

## Prerequisite:

- Install or use Postman
- Import postman api from <a href="../_attachments/38184589400-luz-api-collection.zip" data-nice-type="Zip Archive">here</a>,
- import enviroment collection <a href="../_attachments/38184589400-luz-enviroment-collection.zip" data-nice-type="Zip Archive">here</a>

  

## Forewords

In order to explore further, we need tenant token, which can be fetch via postman, then we could use API explore tool below.

  

## Fetch token tenant

1.  Choose your environment (mostly it would be Klara Dev).


![[38184589400-Screen Shot 2019-01-09 at 8.52.04 AM.png]]



       2. Login into our CLA (luz_mobile) for list of tenants


![[38184589400-image2019-1-9_8-56-18.png]]



       3. Pick random tenant id


![[38184589400-image2019-1-9_8-58-2.png]]



        4. Request tenant token using picked tenant id


![[38184589400-image2019-1-9_8-59-45.png]]



         5. Now we have our token tenant


![[38184589400-image2019-1-9_9-0-35.png]]



## API explore tool, using tenant token above

<a href="http://10.124.0.20:8080/luz_api_explore/" class="external-link" rel="nofollow">http://10.124.0.20:8080/luz_api_explore/</a>

List consume api

<div>

|  |  |
|----|----|
| \# | API |
| 1 | <a href="http://10.124.0.20:8080/luzfin_finance/api/swagger.json" class="external-link" rel="nofollow">http://10.124.0.20:8080/luzfin_finance/api/swagger.json</a> |

</div>

  

How to explore

1.  Go to api explore URI above, then input the tenant token from previous section, then EXPLORE.


![[38184589400-Screen Shot 2019-01-09 at 9.01.42 AM.png]]



       2. Congratulation, you are now able to explore list of given api.


![[38184589400-image2019-1-9_9-7-4.png]]

%% ai-graph-start %%

**Related notes:**
- [[Swagger with api explorer]]
- [[HowToUseNewTokenAPI]]
- [[Use KLARA Swagger UI for REST API]]
- [[OpenAPI UI (API on SwaggerUI)]]
- [[Token JWT Security]]

%% ai-graph-end %%