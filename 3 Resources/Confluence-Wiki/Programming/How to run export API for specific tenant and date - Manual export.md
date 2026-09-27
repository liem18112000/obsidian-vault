---
ai_hash: 4e3e99571b01e18b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 2
depth: 3
entities: []
relevance: 0.786
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47081652666/How+to+run+export+API+for+specific+tenant+and+date+-+Manual+export
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: How to run export API for specific tenant and date - Manual export
topic: programming
type: source
updated: 2022-03-25
---

# How to run export API for specific tenant and date - Manual export

> [!info] Imported from Confluence
> Space **LUZ** · updated 2022-03-25 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47081652666/How+to+run+export+API+for+specific+tenant+and+date+-+Manual+export)
> Relevance 0.786 · topic `programming`

## 1. Get service tenant access token

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f11c28db-928f-45b2-b0f5-af700a5ecf96" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
curl --location --request POST 'http://localhost:8080/luzsec/api/{service-tenant-id}/service-tenant-tokens' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'client_id={service-tenant-id}' \
--data-urlencode 'grant_type=client_credentials' \
--data-urlencode 'client_secret={service-tenant-client-secret}'
```

</div>

</div>

  
Example:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9789e297-5c2f-49d2-97e6-bfda17fbd020" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
curl --location --request POST 'http://localhost:8082/luzsec/api/dd8a25cb-a90e-4099-ab0d-64d07d8a17d8/service-tenant-tokens' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'client_id=dd8a25cb-a90e-4099-ab0d-64d07d8a17d8' \
--data-urlencode 'grant_type=client_credentials' \
--data-urlencode 'client_secret=b63a28d9-2e55-4d50-829c-9b43b0ea9604'
```

</div>

</div>

**API response:**


![[47081652666-image-20220325-033936.png]]



## 2. Call API export manually for tenant and date

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8150aaad-4d23-4cab-a90d-49b1978f7bea" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
curl --location --request POST 'http://localhost:8080/luz_audit/{service-tenant-id}/jobs/export/{export-tenant-id}?export-date={export-date}' \
--header 'Authorization: Bearer {service-tenant-access-token}'
```

</div>

</div>

**Note**: Please input query param “*export-date*“ with format yyyy-MM-dd

  
Example:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="d4497471-7cbe-4a14-845c-65215655f839" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
curl --location --request POST 'http://localhost:8088/luz_audit/api/23bc4385-8eb2-4b30-a959-50382e3440cf/jobs/export/0a3a64d9-cd03-45ff-bf54-2dc170a05715?export-date=2022-03-24' \
--header 'Authorization: Bearer eyJhbGciOiJSUzUxMiJ9.eyJzdWIiOiJsdXotYXVkaXQiLCJzZXJ2aWNlLXRlbmFudCI6eyJyb2xlcyI6WyJsdXotYXVkaXQiXSwidGVuYW50SWQiOiIyM2JjNDM4NS04ZWIyLTRiMzAtYTk1OS01MDM4MmUzNDQwY2YiLCJ0eXBlIjoiU0VSVklDRV9URU5BTlQifSwiaXNzIjoiY29tLmF4b25pdnkiLCJ0ZW5hbnRJZCI6IjIzYmM0Mzg1LThlYjItNGIzMC1hOTU5LTUwMzgyZTM0NDBjZiIsImV4cCI6MTY0ODIxNzk1NywiaWF0IjoxNjQ4MTc0NzU3fQ.QTvu1N5P23ZKIQl23NoqgnHJoZiamc5iSDpq8SjC7Hk9lCHzYySLFw-AnRsCkao7iUHodsvo4AoXNoXoEOgGWfTIzkoTbRv2SmJKXgM4_ebtQ2EGVV9aKRSOGdOvY5vNoEmKVwVUjUGmA1_wq47XCS7Q8wDL058O8BTbCRt9w0C4aGRaAjldkqPFiZ_cMsDLz3mzA5tCSKmkLFA8V8Rjz8wT3Y67jQyGfRQH-2v7PuKdN47Y-BYZoepmQpl4Ep0B3qPq5RBQEYt9QMmhbyy-PXXOmLICMHV8401EKKF9Kav-ofNEs2EoXVZSB_4pGHprOXVk36coF3KQi_yi3nm6dg'
```

</div>

</div>


![[47081652666-image-20220325-040150.png]]

%% ai-graph-start %%

**Related notes:**
- [[Download user audit logs export files]]
- [[Export CRM statistics by API]]
- [[Rerun Own domain migration api for all tenant]]
- [[How to execute API to create sync event for post from tenant schemas to public table]]
- [[Empty Trash APIs]]

%% ai-graph-end %%