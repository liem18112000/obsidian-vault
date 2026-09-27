---
title: "Adapt to support ONE API - Enricher first delivery"
created: 2025-12-31
updated: 2026-01-07
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49010606081/Adapt+to+support+ONE+API+-+Enricher+first+delivery
confluence_id: "49010606081"
confluence_path: "Team Kepler > Test Case Library > LUZ-DOCS"
tags: [confluence, luz-docs, testing, enricher]
---

# Adapt to support ONE API - Enricher first delivery

*Confluence source · Team Kepler › Test Case Library › LUZ-DOCS · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49010606081/Adapt+to+support+ONE+API+-+Enricher+first+delivery) · updated 2026-01-07*

## Test cases

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>**Test Case#:** 1</p></td>
<td><p>**Test Case Name:** Enricher first delivery - Failed second phase</p></td>
</tr>
<tr>
<td><p>**System:** LUZ-DOCS</p></td>
<td><p>**Subsystem:** LUZ-DOCS</p></td>
</tr>
<tr>
<td><p>**Test case designed by:**<br />
[[Kepler] - Liem Doan](https://axonivy.atlassian.net/wiki/people/712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4?ref=confluence)</p></td>
<td><p>**Design Date:** 31 Dec 2025</p></td>
</tr>
<tr>
<td><p>**Short description: **</p></td>
<td><p>Currently, the ONE API has a special delivery flow for marketing letters that requires the enrichment process to be completed **before** the letter is delivered to the recipient.</p>
<p>However, luz-docs is not currently designed for this behavior. In luz-docs, enrichment is not a mandatory finish step for document creation.</p>
<p>To support this marketing letter use case, ONE API and luz-docs need to be aligned so that delivery is not failed due to enrichment process</p></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td><p>**Pre – conditions:**</p></td>
</tr>
<tr>
<td><ul>
<li><p>Have valid token for authentication:</p>
<ul>
<li><p>username: [liem.doanvanthanh@axonactive.com](mailto:liem.doanvanthanh@axonactive.com)</p></li>
<li><p>password: `Liem18112000`</p></li>
</ul></li>
<li><p>Have a company type tenant:</p>
<ul>
<li><p>tenantid: 6b884e9b-2612-47fa-954e-666e7d3bab13</p></li>
</ul></li>
<li><p>Port forward for API Forwarder:</p>
<ul>
<li><p>Run this in cmd: kubectl port-forward --address 0.0.0.0 services/api-forwarder 8080:8080 -n dev</p></li>
</ul></li>
<li><p>Port forward luz-vault:</p>
<ul>
<li><p>Run this in cmd: kubectl port-forward --address 0.0.0.0 services/luz-vault 8200:8200 -n dev</p></li>
</ul></li>
<li><p>Port forward Mongo:</p>
<ul>
<li><p>Run this in cmd: kubectl port-forward luz-mongodb02-cluster-rs-0 5017:27017 -n dev-mongodb-clusters</p></li>
</ul></li>
<li><p>**For test purpose:**</p>
<ul>
<li><p>**AI Service is purposely unavailable to simulate the failure of the Phase 02 in enricher**</p></li>
</ul></li>
</ul></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td><p>**Post – conditions:**</p></td>
</tr>
<tr>
<td><ul>
<li><p>A new document must be created with enricher status is failed</p></li>
</ul></td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

### Test Steps

<table style="width:100%;">
<colgroup>
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
</colgroup>
<tbody>
<tr>
<td rowspan="2"><p>**Step**</p></td>
<td rowspan="2"><p>**Action**</p></td>
<td rowspan="2"><p>**Expected Behavior**</p></td>
<td rowspan="2"><p>**Actual Behavior**</p></td>
<td rowspan="2"><p>**Comment**</p></td>
<td colspan="2"><p>**Results**</p></td>
</tr>
<tr>
<td><p>As expected</p></td>
<td><p>not as expected</p></td>
</tr>
<tr>
<td><p>1</p></td>
<td><p>Run this cURL to get access token</p>
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`curl --location --request POST &#39;http://localhost:8080/luzsec/api/{{tenantid}}/tokens&#39; \
--header &#39;Authorization: Basic bGllbS5kb2FudmFudGhhbmhAYXhvbmFjdGl2ZS5jb206TGllbTE4MTEyMDAw&#39; `</pre>
</td>
<td><p>HTTP Status 201 with valid token</p>
<ul>
<li><p>`"token"`: the access token to use for all below step</p></li>
</ul>
> [!note]- Sample body
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`{
>     "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzUxMiJ9.eyJzdWIiOiJsaWVtLmRvYW52YW50aGFuaEBheG9uYWN0aXZlLmNvbSIsImNvbXBhbnktdGVuYW50Ijp7ImNvbXBhbnlJbmZvSWQiOjg4MTAsImNvbXBhbnlJZCI6MSwiY29tcGFueU5hbWUiOiJMSU5EQSIsInN0YXR1cyI6IkFDVElWQVRFRCIsInN0YXR1c0V4cGlyeURhdGUiOm51bGwsImNvbXBhbnlUeXBlIjoiQlVTSU5FU1MiLCJpZCI6MTg5NTc3LCJyb2xlcyI6WyJvYmplY3RfbWFuYWdlciIsImNvbXBhbnlfYWRtaW5pc3RyYXRvciIsIm15a2xhcmFfdXNlciJdLCJ0ZW5hbnRJZCI6IjZiODg0ZTliLTI2MTItNDdmYS05NTRlLTY2NmU3ZDNiYWIxMyIsInVzZXJuYW1lIjoibGllbS5kb2FudmFudGhhbmhAYXhvbmFjdGl2ZS5jb20iLCJuYW1lIjpudWxsLCJ0eXBlIjoiQ09NUEFOWSIsImNyZWF0ZURhdGUiOm51bGwsInVwZGF0ZURhdGUiOm51bGwsImV4cGlyZURhdGUiOm51bGwsImV4cGlyZVRpbWUiOm51bGx9LCJpc3MiOiJjb20uYXhvbml2eSIsInRlbmFudElkIjoiNmI4ODRlOWItMjYxMi00N2ZhLTk1NGUtNjY2ZTdkM2JhYjEzIiwidXNlcl9yb2xlcyI6WyJFdmVyeWJvZHkiXSwicGVyc29uLXRlbmFudCI6eyJpZCI6MCwicm9sZXMiOlsiY29tcGFueV9hZG1pbmlzdHJhdG9yIl0sInRlbmFudElkIjoiMjc1OTUzYzAtNzdjYS00OGEyLThhZTAtNjhhMGI2MGJlZWZlIiwidXNlcm5hbWUiOiJsaWVtLmRvYW52YW50aGFuaEBheG9uYWN0aXZlLmNvbSIsIm5hbWUiOm51bGwsInR5cGUiOiJQRVJTT04iLCJjcmVhdGVEYXRlIjoiV2VkIE9jdCAyMiAxMDoyNTo0OCBDRVNUIDIwMjUiLCJ1cGRhdGVEYXRlIjpudWxsLCJleHBpcmVEYXRlIjpudWxsLCJleHBpcmVUaW1lIjpudWxsfSwiZXhwIjoxNzY3MjExNTQzLCJpYXQiOjE3NjcxNjgzNDMsInNlY3VyaXR5X2NsYXNzZXMiOlsiUkVBRE9OTFlfMSJdfQ.S3fwP4Ft8dsHYyYEopEDFyJb-XcfKdwcZTWkGRY2Y5JiAtWIBkncZAxPlCIAYmw-2aLsHMxb8yUQNRgaOxTTPvQ5fwRUUqcufHNuRlgTCda6dFas9bVgUXAj1Gqt3J9Fvg_cWW9LbeRSIcPh0q8VoPiqr9xx13bTUugLahA6y86lWftXtjj3jfjvI1IDalUU7JH1OtYZe82vC3yEcrqAd2B94ICQu9AdgHZ4EgprDvFNtAtSJMAJR7Lnft4xofCUNa6qEqDyhipBdw5THzfNYFLpPMsh3cVTHmcjim8poawCK1H45nHvieXIPtdvEEjZMiAipWh61IRFKUfJa13U1g",
>     "publicKey": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAvAROyHbXkbhmx7ZsIwIl2sglqmZKpQKgnwtdytfQ/p5rwx4WX3KVuzt82+nwHUpddGneSlAlG6Ob3aV9kRjVvOfkACg8Wi9KSCbn2qYSCU5SVCVjl0e+rPZZfKlOY3RT3O5JStovB/N26LFwXtgxvcs2toOtnP+E6h6PKjyZrSJrjLjXJvCr6t65BjD9qyJvhEccPOCiPwJuQvjlO+hcfks+Hdlsgagn/b0oSZYvCoLQ9sY8Eu2umXPQu8aBxuD+bISrtbNKoQtao5Z1l8IeI7raxi3kEKy2f5jqdkcJOa2qVlHe6qN7xZpKeLh4jrWmL5lc6j3Cc4Cb1dsb9GYqXwIDAQAB",
>     "refreshToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzUxMiJ9.eyJzdWIiOiJsaWVtLmRvYW52YW50aGFuaEBheG9uYWN0aXZlLmNvbSIsImlzcyI6ImNvbS5heG9uaXZ5IiwidGVuYW50SWQiOiI2Yjg4NGU5Yi0yNjEyLTQ3ZmEtOTU0ZS02NjZlN2QzYmFiMTMiLCJ1c2VyX3JvbGVzIjpbIm9iamVjdF9tYW5hZ2VyIiwiY29tcGFueV9hZG1pbmlzdHJhdG9yIiwibXlrbGFyYV91c2VyIiwiRXZlcnlib2R5Il0sImV4cCI6MTc4MjcyMDM0MywiaWF0IjoxNzY3MTY4MzQzLCJqdGkiOiI4NjYwZGZiYi1iMWI4LTRjNDItODk0OS0yNDI3ODBhNTViNTkifQ.JYA3bElMXVatw-C-5u00S7e-AK3fPwexLJOljTbptc0GbwfODdOTlsNrWPNNZpl1mUrYck_Z3QqwWRpRJYGSnISO2iJ7tdENyDdZ-KG9biFPc_pysEp3p4_yeGzRaFticm1IiP_0bxUi98CGjdC2TvssW3trEDqilyo1lxp22jz3o5KqPndUMlDhkTgFhIHAuxWeMFcI8_sWIzCdscn-9qM-sk6RkkDPBkMiguCmVDeqLMSflA3pKmL-948XvVU1JFZ6VXUbqgs8o4mXU9add9BeXK1hpLf04VrFqxCkio6qINyD26HbJOdytrUUMBNeZEI4ivKyMmqC68vMYxHtHQ"
> }`</pre>
</td>
<td></td>
<td></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/LUZ-DOCS/attachments/adapt-to-support-one-api-enricher-first-delivery/check.png]]</p></td>
<td><p>🚫</p></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>Run this cURL to create docuemnt in tenant</p>
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: powershell; gutter: false; theme: Confluence" data-theme="Confluence">`curl --location  --request POST &#39;http://localhost:8147/luz_docs/api/{tenantid}/documents?enricherPriority=URGENT&#39; \
--header &#39;Authorization: Bearer {token}&#39;\
--form &#39;files=@"postman-cloud:///1f0e1495-c56f-43f0-9112-dacbf35824e4"&#39; \
--form &#39;metadata="{
  \"documentTitle\": \"Document with test rerun enrich\"
}"&#39;`</pre>
</td>
<td><p>Response: </p>
<ul>
<li><p>HTTP Status 201</p></li>
</ul>
<p>Sample Body:</p>
<ul>
<li><p>"_id": "695b36d382d0712643f54486"</p></li>
<li><p>"_isEnriched": false</p></li>
<li><p>"isEnricherFirst": true</p></li>
</ul>
> [!note]- Detail body
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`{
>     "_id": "69525c9c74a6244c3df1931a",
>     "_createdBy": "liem.doanvanthanh@axonactive.com",
>     "_createdDate": "2025-12-29T10:48:59.481Z",
>     "_updatedBy": "liem.doanvanthanh@axonactive.com",
>     "_updatedDate": "2025-12-29T10:48:59.481Z",
>     "_deletionStatus": "false",
>     "_files": {
>         "reference": {
>             "_updatedDate": "2025-12-29T10:48:59.481Z",
>             "_sizeInBytes": 48525,
>             "_createdDate": "2025-12-29T10:48:59.481Z",
>             "_scanningTime": "2025-12-29T10:48:58.899287Z",
>             "_antivirusVersion": "ClamAV 1.4.3/27863/Sun Dec 28 08:26:03 2025",
>             "_scanningResult": "OK",
>             "_link": {
>                 "self": "6b884e9b-2612-47fa-954e-666e7d3bab13/documents/69525c9c74a6244c3df1931a/files/reference"
>             }
>         }
>     },
>     "_isBasicDocument": false,
>     "_isBeingCreated": false,
>     "_isEnriched": false,
>     "_isLargeFile": false,
>     "_isTemporaryDocument": false,
>     "_versionNumber": 1,
>     "folderIds": [],
>     "name": "review_test_plan.pdf",
>     "documentTitle": "Document with test rerun enrich",
>     "isEnricherFirst": true,
>     "_link": {
>         "self": "6b884e9b-2612-47fa-954e-666e7d3bab13/documents/69525c9c74a6244c3df1931a"
>     }
> }`</pre>
> [!note]- Technical Log
> <p>Create document:</p>
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: bash; gutter: false; theme: Confluence" data-theme="Confluence">`04:58:11,276 INFO  [ch.klara.luz.docs.service.DocumentCreatingService] (default task-1) [createDocument] Document created: 695b36d382d0712643f54486 for tenant 6b884e9b-2612-47fa-954e-666e7d3bab13
> 04:58:18,332 INFO  [ch.klara.luz.docs.service.DocumentCreatingService] (default task-1) [createDocument] Successfully create document for tenantId: 6b884e9b-2612-47fa-954e-666e7d3bab13; documentId: 695b36d382d0712643f54486; fileSize: 48525`</pre>
> <p>The enricher first enrichment must be URGENT priority:</p>
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`04:58:18,322 INFO  [ch.klara.luz.docs.service.DocumentService] (default task-1) [fireAsyncEnrichmentEvent] Triggering enrichment for document 695b36d382d0712643f54486 of tenant 6b884e9b-2612-47fa-954e-666e7d3bab13 with priority URGENT`</pre>
> <p>Setup enricher with immediate retry enabled (Email Enricher will not be here as the document we use is not an email)</p>
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`04:58:21,361 INFO  [ch.klara.luz.docs.enricher.service.retry.RetryableEnricherDecorator] (EE-ManagedExecutorService-urgentEnricher-Thread-1) [EnricherFirst] Starting TimeStampEnricher for document 695b36d382d0712643f54486 with retry enabled
> 04:58:22,662 INFO  [ch.klara.luz.docs.enricher.service.retry.RetryableEnricherDecorator] (EE-ManagedExecutorService-urgentEnricher-Thread-1) [EnricherFirst] Starting ContentTypeEnricher for document 695b36d382d0712643f54486 with retry enabled
> 04:58:23,095 INFO  [ch.klara.luz.docs.enricher.service.retry.RetryableEnricherDecorator] (EE-ManagedExecutorService-urgentEnricher-Thread-1) [EnricherFirst] Starting ThumbnailEnricher for document 695b36d382d0712643f54486 with retry enabled
> 04:58:34,560 INFO  [ch.klara.luz.docs.enricher.service.retry.RetryableEnricherDecorator] (default task-1) [EnricherFirst] Starting DocumentDiscoverEnricher for document 695b36d382d0712643f54486 with retry enabled`</pre>
> <p>AI Service (DocumentDiscoverEnricher) get failed as expected:</p>
> <ul>
> <li><p>As it failed, the immediate retry is triggered:</p></li>
> <li><p>It will retry 3 time (delay 1 second, jitter 0.5 second)</p></li>
> <li><p>After 3 time retry, it exhausted</p></li>
> </ul>
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`04:58:36,589 INFO  [ch.klara.luz.docs.enricher.service.retry.RetryableEnricherDecorator$Executor] (default task-1) [EnricherFirst] DocumentDiscoverEnricher FAILED for document 695b36d382d0712643f54486 - triggering retry: RESTEASY004655: Unable to invoke request: org.apache.http.conn.ConnectTimeoutException: Connect to 192.168.1.18:80 [/192.168.1.18] failed: Connect timed out
> 2026-01-05T03:58:37.917899419Z 04:58:37,917 INFO  [ch.klara.luz.docs.enricher.service.retry.RetryableEnricherDecorator$Executor] (default task-1) [EnricherFirst] DocumentDiscoverEnricher FAILED for document 695b36d382d0712643f54486 - triggering retry: RESTEASY004655: Unable to invoke request: java.lang.RuntimeException: java.nio.channels.ClosedChannelException
> 2026-01-05T03:58:39.357258121Z 04:58:39,356 INFO  [ch.klara.luz.docs.enricher.service.retry.RetryableEnricherDecorator$Executor] (default task-1) [EnricherFirst] DocumentDiscoverEnricher FAILED for document 695b36d382d0712643f54486 - triggering retry: RESTEASY004655: Unable to invoke request: java.lang.RuntimeException: java.nio.channels.ClosedChannelException
> 2026-01-05T03:58:40.733085417Z 04:58:40,731 INFO  [ch.klara.luz.docs.enricher.service.retry.RetryableEnricherDecorator$Executor] (default task-1) [EnricherFirst] DocumentDiscoverEnricher FAILED for document 695b36d382d0712643f54486 - triggering retry: RESTEASY004655: Unable to invoke request: java.lang.RuntimeException: java.nio.channels.ClosedChannelException
> 2026-01-05T03:58:40.733236350Z 04:58:40,732 WARNING [ch.klara.luz.docs.enricher.service.retry.RetryableEnricherDecorator] (default task-1) [EnricherFirst] Retry exhausted for document 695b36d382d0712643f54486 in DocumentDiscoverEnricher - removing isEnricherFirst metadata: RESTEASY004655: Unable to invoke request: java.lang.RuntimeException: java.nio.channels.ClosedChannelException`</pre>
> <p>The final status is added to enrichment status:</p>
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`06:09:40,923 INFO  [javax.ws.rs.client.ClientResponseFilter] (default task-1) [PUT] - http://host.docker.internal:8080/luz_jsonstore/api/mdb/6b884e9b-2612-47fa-954e-666e7d3bab13/enrichmentstatus/add headers=[Connection=close,Content-Length=150,Content-Type=application/json,Date=Mon, 05 Jan 2026 05:09:40 GMT,Server=nginx/1.29.4,x-envoy-upstream-service-time=9] status-code=200 time-consuming=1044`</pre>
</td>
<td></td>
<td><p> </p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>3</p></td>
<td><p>Run this cURL to check the final status of document :</p>
<ul>
<li><p>docid: "695b36d382d0712643f54486"</p></li>
<li><p>tenantid: 6b884e9b-2612-47fa-954e-666e7d3bab13</p></li>
</ul>
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`curl --location &#39;http://localhost:8147/luz_docs/api/{tenantid}/documents/{docid}?exclude-total-count=false&folder-id=string&include-deleted-documents=true&include-file=false&include-folder-name=true&skip-security-classes=true&sort=string&#39; \
--header &#39;credential-token: string&#39; \
--header &#39;Accept: application/json&#39; \
--header &#39;Authorization: Bearer {token}&#39;`</pre>
</td>
<td><p> Response: </p>
<ul>
<li><p>HTTP Status 200</p></li>
</ul>
<p>The body must have:</p>
<ul>
<li><p>`reference`: content type enricher success</p></li>
<li><p>`referenceTsq` and `referenceTsr`: Timestamp enricher success</p></li>
<li><p>`thumbnail128` and `thumbnail256` and `thumbnail512`: Thumbnail success</p></li>
</ul>
<p>=> The field of AI Document Analyzer must be not found</p>
<p>Sample body:</p>
<ul>
<li><p>"_id": "695b36d382d0712643f54486"</p></li>
<li><p>"_isEnriched": false</p></li>
<li><p>"isEnricherFirst": true</p></li>
</ul>
> [!note]- Detail body
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`{
>     "_id": "695b36d382d0712643f54486",
>     "_createdBy": "liem.doanvanthanh@axonactive.com",
>     "_createdDate": "2025-12-29T10:48:59.481Z",
>     "_updatedBy": "liem.doanvanthanh@axonactive.com",
>     "_updatedDate": "2025-12-29T10:49:19.945Z",
>     "_deletionStatus": "false",
>     "_files": {
>         "reference": {
>             "_updatedDate": "2025-12-29T10:48:59.481Z",
>             "_sizeInBytes": 48525,
>             "_createdDate": "2025-12-29T10:48:59.481Z",
>             "_scanningTime": "2025-12-29T10:48:58.899287Z",
>             "_antivirusVersion": "ClamAV 1.4.3/27863/Sun Dec 28 08:26:03 2025",
>             "_scanningResult": "OK",
>             "contentType": "application/pdf",
>             "_link": {
>                 "self": "6b884e9b-2612-47fa-954e-666e7d3bab13/documents/69525c9c74a6244c3df1931a/files/reference"
>             }
>         },
>         "referenceTsq": {
>             "_createdDate": "2025-12-29T10:49:13.637Z",
>             "_sizeInBytes": 89,
>             "_updatedDate": "2025-12-29T10:49:13.637Z",
>             "contentType": "application/timestamp-query",
>             "_link": {
>                 "self": "6b884e9b-2612-47fa-954e-666e7d3bab13/documents/69525c9c74a6244c3df1931a/files/referenceTsq"
>             }
>         },
>         "referenceTsr": {
>             "_createdDate": "2025-12-29T10:49:13.638Z",
>             "_sizeInBytes": 3478,
>             "_updatedDate": "2025-12-29T10:49:13.638Z",
>             "contentType": "application/timestamp-reply",
>             "_link": {
>                 "self": "6b884e9b-2612-47fa-954e-666e7d3bab13/documents/69525c9c74a6244c3df1931a/files/referenceTsr"
>             }
>         },
>         "thumbnail128": {
>             "_createdDate": "2025-12-29T10:49:16.302Z",
>             "_sizeInBytes": 6559,
>             "_updatedDate": "2025-12-29T10:49:16.302Z",
>             "contentType": "image/jpeg",
>             "height": 128,
>             "width": 90,
>             "_link": {
>                 "self": "6b884e9b-2612-47fa-954e-666e7d3bab13/documents/69525c9c74a6244c3df1931a/files/thumbnail128"
>             }
>         },
>         "thumbnail256": {
>             "_createdDate": "2025-12-29T10:49:16.302Z",
>             "_sizeInBytes": 19903,
>             "_updatedDate": "2025-12-29T10:49:16.302Z",
>             "contentType": "image/jpeg",
>             "height": 256,
>             "width": 181,
>             "_link": {
>                 "self": "6b884e9b-2612-47fa-954e-666e7d3bab13/documents/69525c9c74a6244c3df1931a/files/thumbnail256"
>             }
>         },
>         "thumbnail512": {
>             "_createdDate": "2025-12-29T10:49:16.302Z",
>             "_sizeInBytes": 67313,
>             "_updatedDate": "2025-12-29T10:49:16.302Z",
>             "contentType": "image/jpeg",
>             "height": 512,
>             "width": 361,
>             "_link": {
>                 "self": "6b884e9b-2612-47fa-954e-666e7d3bab13/documents/69525c9c74a6244c3df1931a/files/thumbnail512"
>             }
>         }
>     },
>     "_isBasicDocument": false,
>     "_isBeingCreated": false,
>     "_isEnriched": false,
>     "_isLargeFile": false,
>     "_isTemporaryDocument": false,
>     "_versionNumber": 2,
>     "folderIds": [],
>     "name": "review_test_plan.pdf",
>     "documentTitle": "Document with test rerun enrich",
>     "isEnricherFirst": true,
>     "_folders": [],
>     "_link": {
>         "self": "6b884e9b-2612-47fa-954e-666e7d3bab13/documents/69525c9c74a6244c3df1931a"
>     }
> }`</pre>
</td>
<td></td>
<td><p> </p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>4</p></td>
<td><p>Run this cURL to check the final status of the document :</p>
<ul>
<li><p>tenantid: 6b884e9b-2612-47fa-954e-666e7d3bab13</p></li>
</ul>
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`curl --location &#39;localhost:8080/luz_jsonstore/api/mdb/6b884e9b-2612-47fa-954e-666e7d3bab13/enrichmentstatus&#39; \
--header &#39;Content-Type: application/json&#39; \
--header &#39;Authorization: Bearer {token}&#39; \
--data &#39;{
}&#39;`</pre>
</td>
<td><p>Response: </p>
<ul>
<li><p>HTTP Status 200</p></li>
</ul>
<p> As the document failed to enrich the body should have:</p>
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`[
    ...
    {
        "_id": "69525cc874a6244c3df1933c",
        "_retryEnrichment": 0,
        "_enrichmentStarted": "2025-12-29T10:49:43.250Z",
        "documentId": "695b36d382d0712643f54486"
    },
    ...
]`</pre>
</td>
<td></td>
<td><p> </p></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

## Test cases

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>**Test Case#:** 2</p></td>
<td><p>**Test Case Name:** Enricher first delivery - Failed first phase</p></td>
</tr>
<tr>
<td><p>**System:** LUZ-DOCS</p></td>
<td><p>**Subsystem:** LUZ-DOCS</p></td>
</tr>
<tr>
<td><p>**Test case designed by:**<br />
[[Kepler] - Liem Doan](https://axonivy.atlassian.net/wiki/people/712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4?ref=confluence)</p></td>
<td><p>**Design Date:** 31 Dec 2025</p></td>
</tr>
<tr>
<td><p>**Short description: **</p></td>
<td><p>Currently, the ONE API has a special delivery flow for marketing letters that requires the enrichment process to be completed **before** the letter is delivered to the recipient.</p>
<p>However, luz-docs is not currently designed for this behavior. In luz-docs, enrichment is not a mandatory finish step for document creation.</p>
<p>To support this marketing letter use case, ONE API and luz-docs need to be aligned so that delivery is not failed due to enrichment process</p></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td><p>**Pre – conditions:**</p></td>
</tr>
<tr>
<td><ul>
<li><p>Have valid token for authentication:</p>
<ul>
<li><p>username: [liem.doanvanthanh@axonactive.com](mailto:liem.doanvanthanh@axonactive.com)</p></li>
<li><p>password: `Liem18112000`</p></li>
</ul></li>
<li><p>Have a company type tenant:</p>
<ul>
<li><p>tenantid: 6b884e9b-2612-47fa-954e-666e7d3bab13</p></li>
</ul></li>
<li><p>Port forward for API Forwarder:</p>
<ul>
<li><p>Run this in cmd: kubectl port-forward --address 0.0.0.0 services/api-forwarder 8080:8080 -n dev</p></li>
</ul></li>
<li><p>Port forward luz-vault:</p>
<ul>
<li><p>Run this in cmd: kubectl port-forward --address 0.0.0.0 services/luz-vault 8200:8200 -n dev</p></li>
</ul></li>
<li><p>**For test purpose:**</p>
<ul>
<li><p>**Thumbnail Service is purposely unavailable to simulate the failure of the Phase 01 in enricher**</p></li>
</ul></li>
</ul></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td><p>**Post – conditions:**</p></td>
</tr>
<tr>
<td><ul>
<li><p>A new document must be created with enricher status is failed</p></li>
</ul></td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

### Test Steps

<table style="width:100%;">
<colgroup>
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
</colgroup>
<tbody>
<tr>
<td rowspan="2"><p>**Step**</p></td>
<td rowspan="2"><p>**Action**</p></td>
<td rowspan="2"><p>**Expected Behavior**</p></td>
<td rowspan="2"><p>**Actual Behavior**</p></td>
<td rowspan="2"><p>**Comment**</p></td>
<td colspan="2"><p>**Results**</p></td>
</tr>
<tr>
<td><p>As expected</p></td>
<td><p>not as expected</p></td>
</tr>
<tr>
<td><p>1</p></td>
<td><p>Run this cURL to get access token</p>
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`curl --location --request POST &#39;http://localhost:8080/luzsec/api/{{tenantid}}/tokens&#39; \
--header &#39;Authorization: Basic bGllbS5kb2FudmFudGhhbmhAYXhvbmFjdGl2ZS5jb206TGllbTE4MTEyMDAw&#39; `</pre>
</td>
<td><p>HTTP Status 201 with valid token</p>
<ul>
<li><p>`"token"`: the access token to use for all below step</p></li>
</ul>
> [!note]- Sample body
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`{
>     "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzUxMiJ9.eyJzdWIiOiJsaWVtLmRvYW52YW50aGFuaEBheG9uYWN0aXZlLmNvbSIsImNvbXBhbnktdGVuYW50Ijp7ImNvbXBhbnlJbmZvSWQiOjg4MTAsImNvbXBhbnlJZCI6MSwiY29tcGFueU5hbWUiOiJMSU5EQSIsInN0YXR1cyI6IkFDVElWQVRFRCIsInN0YXR1c0V4cGlyeURhdGUiOm51bGwsImNvbXBhbnlUeXBlIjoiQlVTSU5FU1MiLCJpZCI6MTg5NTc3LCJyb2xlcyI6WyJvYmplY3RfbWFuYWdlciIsImNvbXBhbnlfYWRtaW5pc3RyYXRvciIsIm15a2xhcmFfdXNlciJdLCJ0ZW5hbnRJZCI6IjZiODg0ZTliLTI2MTItNDdmYS05NTRlLTY2NmU3ZDNiYWIxMyIsInVzZXJuYW1lIjoibGllbS5kb2FudmFudGhhbmhAYXhvbmFjdGl2ZS5jb20iLCJuYW1lIjpudWxsLCJ0eXBlIjoiQ09NUEFOWSIsImNyZWF0ZURhdGUiOm51bGwsInVwZGF0ZURhdGUiOm51bGwsImV4cGlyZURhdGUiOm51bGwsImV4cGlyZVRpbWUiOm51bGx9LCJpc3MiOiJjb20uYXhvbml2eSIsInRlbmFudElkIjoiNmI4ODRlOWItMjYxMi00N2ZhLTk1NGUtNjY2ZTdkM2JhYjEzIiwidXNlcl9yb2xlcyI6WyJFdmVyeWJvZHkiXSwicGVyc29uLXRlbmFudCI6eyJpZCI6MCwicm9sZXMiOlsiY29tcGFueV9hZG1pbmlzdHJhdG9yIl0sInRlbmFudElkIjoiMjc1OTUzYzAtNzdjYS00OGEyLThhZTAtNjhhMGI2MGJlZWZlIiwidXNlcm5hbWUiOiJsaWVtLmRvYW52YW50aGFuaEBheG9uYWN0aXZlLmNvbSIsIm5hbWUiOm51bGwsInR5cGUiOiJQRVJTT04iLCJjcmVhdGVEYXRlIjoiV2VkIE9jdCAyMiAxMDoyNTo0OCBDRVNUIDIwMjUiLCJ1cGRhdGVEYXRlIjpudWxsLCJleHBpcmVEYXRlIjpudWxsLCJleHBpcmVUaW1lIjpudWxsfSwiZXhwIjoxNzY3MjExNTQzLCJpYXQiOjE3NjcxNjgzNDMsInNlY3VyaXR5X2NsYXNzZXMiOlsiUkVBRE9OTFlfMSJdfQ.S3fwP4Ft8dsHYyYEopEDFyJb-XcfKdwcZTWkGRY2Y5JiAtWIBkncZAxPlCIAYmw-2aLsHMxb8yUQNRgaOxTTPvQ5fwRUUqcufHNuRlgTCda6dFas9bVgUXAj1Gqt3J9Fvg_cWW9LbeRSIcPh0q8VoPiqr9xx13bTUugLahA6y86lWftXtjj3jfjvI1IDalUU7JH1OtYZe82vC3yEcrqAd2B94ICQu9AdgHZ4EgprDvFNtAtSJMAJR7Lnft4xofCUNa6qEqDyhipBdw5THzfNYFLpPMsh3cVTHmcjim8poawCK1H45nHvieXIPtdvEEjZMiAipWh61IRFKUfJa13U1g",
>     "publicKey": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAvAROyHbXkbhmx7ZsIwIl2sglqmZKpQKgnwtdytfQ/p5rwx4WX3KVuzt82+nwHUpddGneSlAlG6Ob3aV9kRjVvOfkACg8Wi9KSCbn2qYSCU5SVCVjl0e+rPZZfKlOY3RT3O5JStovB/N26LFwXtgxvcs2toOtnP+E6h6PKjyZrSJrjLjXJvCr6t65BjD9qyJvhEccPOCiPwJuQvjlO+hcfks+Hdlsgagn/b0oSZYvCoLQ9sY8Eu2umXPQu8aBxuD+bISrtbNKoQtao5Z1l8IeI7raxi3kEKy2f5jqdkcJOa2qVlHe6qN7xZpKeLh4jrWmL5lc6j3Cc4Cb1dsb9GYqXwIDAQAB",
>     "refreshToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzUxMiJ9.eyJzdWIiOiJsaWVtLmRvYW52YW50aGFuaEBheG9uYWN0aXZlLmNvbSIsImlzcyI6ImNvbS5heG9uaXZ5IiwidGVuYW50SWQiOiI2Yjg4NGU5Yi0yNjEyLTQ3ZmEtOTU0ZS02NjZlN2QzYmFiMTMiLCJ1c2VyX3JvbGVzIjpbIm9iamVjdF9tYW5hZ2VyIiwiY29tcGFueV9hZG1pbmlzdHJhdG9yIiwibXlrbGFyYV91c2VyIiwiRXZlcnlib2R5Il0sImV4cCI6MTc4MjcyMDM0MywiaWF0IjoxNzY3MTY4MzQzLCJqdGkiOiI4NjYwZGZiYi1iMWI4LTRjNDItODk0OS0yNDI3ODBhNTViNTkifQ.JYA3bElMXVatw-C-5u00S7e-AK3fPwexLJOljTbptc0GbwfODdOTlsNrWPNNZpl1mUrYck_Z3QqwWRpRJYGSnISO2iJ7tdENyDdZ-KG9biFPc_pysEp3p4_yeGzRaFticm1IiP_0bxUi98CGjdC2TvssW3trEDqilyo1lxp22jz3o5KqPndUMlDhkTgFhIHAuxWeMFcI8_sWIzCdscn-9qM-sk6RkkDPBkMiguCmVDeqLMSflA3pKmL-948XvVU1JFZ6VXUbqgs8o4mXU9add9BeXK1hpLf04VrFqxCkio6qINyD26HbJOdytrUUMBNeZEI4ivKyMmqC68vMYxHtHQ"
> }`</pre>
</td>
<td></td>
<td></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/LUZ-DOCS/attachments/adapt-to-support-one-api-enricher-first-delivery/check.png]]</p></td>
<td><p>🚫</p></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>Run this cURL to create docuemnt in tenant</p>
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: powershell; gutter: false; theme: Confluence" data-theme="Confluence">`curl --location  --request POST &#39;http://localhost:8147/luz_docs/api/{tenantid}/documents?enricherPriority=URGENT&#39; \
--header &#39;Authorization: Bearer {token}&#39;\
--form &#39;files=@"postman-cloud:///1f0e1495-c56f-43f0-9112-dacbf35824e4"&#39; \
--form &#39;metadata="{
  \"documentTitle\": \"Document with test rerun enrich\"
}"&#39;`</pre>
</td>
<td><p>Response: </p>
<ul>
<li><p>HTTP Status 201</p></li>
</ul>
<p>Sample Body:</p>
<ul>
<li><p>"_id": "`695b89e945682644ba2e1e6f`"</p></li>
<li><p>"_isEnriched": false</p></li>
<li><p>"isEnricherFirst": true</p></li>
</ul>
> [!note]- Detail body
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`{
>     "_id": "69525c9c74a6244c3df1931a",
>     "_createdBy": "liem.doanvanthanh@axonactive.com",
>     "_createdDate": "2025-12-29T10:48:59.481Z",
>     "_updatedBy": "liem.doanvanthanh@axonactive.com",
>     "_updatedDate": "2025-12-29T10:48:59.481Z",
>     "_deletionStatus": "false",
>     "_files": {
>         "reference": {
>             "_updatedDate": "2025-12-29T10:48:59.481Z",
>             "_sizeInBytes": 48525,
>             "_createdDate": "2025-12-29T10:48:59.481Z",
>             "_scanningTime": "2025-12-29T10:48:58.899287Z",
>             "_antivirusVersion": "ClamAV 1.4.3/27863/Sun Dec 28 08:26:03 2025",
>             "_scanningResult": "OK",
>             "_link": {
>                 "self": "6b884e9b-2612-47fa-954e-666e7d3bab13/documents/69525c9c74a6244c3df1931a/files/reference"
>             }
>         }
>     },
>     "_isBasicDocument": false,
>     "_isBeingCreated": false,
>     "_isEnriched": false,
>     "_isLargeFile": false,
>     "_isTemporaryDocument": false,
>     "_versionNumber": 1,
>     "folderIds": [],
>     "name": "review_test_plan.pdf",
>     "documentTitle": "Document with test rerun enrich",
>     "isEnricherFirst": true,
>     "_link": {
>         "self": "6b884e9b-2612-47fa-954e-666e7d3bab13/documents/69525c9c74a6244c3df1931a"
>     }
> }`</pre>
> [!note]- Technical Log
> <p>Create document:</p>
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: bash; gutter: false; theme: Confluence" data-theme="Confluence">`10:52:41,929 INFO  [ch.klara.luz.docs.service.DocumentCreatingService] (default task-1) [createDocument] Document created: 695b89e945682644ba2e1e6f for tenant 6b884e9b-2612-47fa-954e-666e7d3bab13
> 10:52:48,565 INFO  [ch.klara.luz.docs.service.DocumentCreatingService] (default task-1) [createDocument] Successfully create document for tenantId: 6b884e9b-2612-47fa-954e-666e7d3bab13; documentId: 695b89e945682644ba2e1e6f; fileSize: 48525`</pre>
> <p>The enricher first enrichment must be URGENT priority:</p>
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`10:52:48,563 INFO  [ch.klara.luz.docs.service.DocumentService] (default task-1) [fireAsyncEnrichmentEvent] Triggering enrichment for document 695b89e945682644ba2e1e6f of tenant 6b884e9b-2612-47fa-954e-666e7d3bab13 with priority URGENT`</pre>
> <p>Setup enricher with immediate retry enabled (Email Enricher will not be here as the document we use is not an email)</p>
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`10:52:51,384 INFO  [ch.klara.luz.docs.enricher.service.retry.RetryableEnricherDecorator] (EE-ManagedExecutorService-urgentEnricher-Thread-2) [EnricherFirst] Starting TimeStampEnricher for document 695b89e945682644ba2e1e6f with retry enabled
> 10:52:52,314 INFO  [ch.klara.luz.docs.enricher.service.retry.RetryableEnricherDecorator] (EE-ManagedExecutorService-urgentEnricher-Thread-2) [EnricherFirst] Starting ContentTypeEnricher for document 695b89e945682644ba2e1e6f with retry enabled
> 10:52:52,378 INFO  [ch.klara.luz.docs.enricher.service.retry.RetryableEnricherDecorator] (EE-ManagedExecutorService-urgentEnricher-Thread-2) [EnricherFirst] Starting ThumbnailEnricher for document 695b89e945682644ba2e1e6f with retry enabled`</pre>
> <p>Thumbnail Service get failed as expected:</p>
> <ul>
> <li><p>As it failed, the immediate retry is triggered:</p></li>
> <li><p>It will retry 3 time (delay 1 second, jitter 0.5 second)</p></li>
> <li><p>After 3 time retry, it exhausted</p></li>
> </ul>
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`10:52:53,322 INFO  [ch.klara.luz.docs.enricher.service.retry.RetryableEnricherDecorator$Executor] (EE-ManagedExecutorService-urgentEnricher-Thread-2) [EnricherFirst] ThumbnailEnricher FAILED for document 695b89e945682644ba2e1e6f - triggering retry: ch.klara.luz.docs.exception.LuzThumbnailException: 404
> 10:52:54,626 INFO  [ch.klara.luz.docs.enricher.service.retry.RetryableEnricherDecorator$Executor] (EE-ManagedExecutorService-urgentEnricher-Thread-2) [EnricherFirst] ThumbnailEnricher FAILED for document 695b89e945682644ba2e1e6f - triggering retry: org.eclipse.microprofile.faulttolerance.exceptions.CircuitBreakerOpenException: CircuitBreaker[ch.klara.luz.docs.client.LuzThumbnailRestClient#generateThumbnail] circuit breaker is open
> 2026-01-05T09:52:55.606609940Z 10:52:55,605 INFO  [ch.klara.luz.docs.enricher.service.retry.RetryableEnricherDecorator$Executor] (EE-ManagedExecutorService-urgentEnricher-Thread-2) [EnricherFirst] ThumbnailEnricher FAILED for document 695b89e945682644ba2e1e6f - triggering retry: org.eclipse.microprofile.faulttolerance.exceptions.CircuitBreakerOpenException: CircuitBreaker[ch.klara.luz.docs.client.LuzThumbnailRestClient#generateThumbnail] circuit breaker is open
> 2026-01-05T09:52:56.714939563Z 10:52:56,714 INFO  [ch.klara.luz.docs.enricher.service.retry.RetryableEnricherDecorator$Executor] (EE-ManagedExecutorService-urgentEnricher-Thread-2) [EnricherFirst] ThumbnailEnricher FAILED for document 695b89e945682644ba2e1e6f - triggering retry: org.eclipse.microprofile.faulttolerance.exceptions.CircuitBreakerOpenException: CircuitBreaker[ch.klara.luz.docs.client.LuzThumbnailRestClient#generateThumbnail] circuit breaker is open
> 2026-01-05T09:52:56.715343070Z 10:52:56,714 WARNING [ch.klara.luz.docs.enricher.service.retry.RetryableEnricherDecorator] (EE-ManagedExecutorService-urgentEnricher-Thread-2) [EnricherFirst] Retry exhausted for document 695b89e945682644ba2e1e6f in ThumbnailEnricher - removing isEnricherFirst metadata: org.eclipse.microprofile.faulttolerance.exceptions.CircuitBreakerOpenException: CircuitBreaker[ch.klara.luz.docs.client.LuzThumbnailRestClient#generateThumbnail] circuit breaker is open`</pre>
> <p>The final status is added to enrichment status:</p>
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`10:53:05,308 INFO  [javax.ws.rs.client.ClientResponseFilter] (default task-1) [PUT] - http://host.docker.internal:8080/luz_jsonstore/api/mdb/6b884e9b-2612-47fa-954e-666e7d3bab13/enrichmentstatus/add headers=[Connection=close,Content-Length=150,Content-Type=application/json,Date=Mon, 05 Jan 2026 09:53:05 GMT,Server=nginx/1.29.4,x-envoy-upstream-service-time=72] status-code=200 time-consuming=937`</pre>
</td>
<td></td>
<td><p> </p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>3</p></td>
<td><p>Run this cURL to check the final status of document :</p>
<ul>
<li><p>docid: "69525c9c74a6244c3df1931a"</p></li>
<li><p>tenantid: 6b884e9b-2612-47fa-954e-666e7d3bab13</p></li>
</ul>
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`curl --location &#39;http://localhost:8147/luz_docs/api/{tenantid}/documents/{docid}?exclude-total-count=false&folder-id=string&include-deleted-documents=true&include-file=false&include-folder-name=true&skip-security-classes=true&sort=string&#39; \
--header &#39;credential-token: string&#39; \
--header &#39;Accept: application/json&#39; \
--header &#39;Authorization: Bearer {token}&#39;`</pre>
</td>
<td><p> Response: </p>
<ul>
<li><p>HTTP Status 200</p></li>
</ul>
<p>The body must have:</p>
<ul>
<li><p>`reference`: content type enricher success</p></li>
<li><p>`referenceTsq` and `referenceTsr`: Timestamp enricher success</p></li>
<li><p>`=> thumbnail128` and `thumbnail256` and `thumbnail512`: must be not found</p></li>
</ul>
<p>Sample body:</p>
<ul>
<li><p>"_id": "695b89e945682644ba2e1e6f"</p></li>
<li><p>"_isEnriched": false</p></li>
<li><p>"isEnricherFirst": true</p></li>
</ul></td>
<td></td>
<td><p> </p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>4</p></td>
<td><p>Run this cURL to check the final status of the document :</p>
<ul>
<li><p>tenantid: 6b884e9b-2612-47fa-954e-666e7d3bab13</p></li>
</ul>
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`curl --location &#39;localhost:8080/luz_jsonstore/api/mdb/6b884e9b-2612-47fa-954e-666e7d3bab13/enrichmentstatus&#39; \
--header &#39;Content-Type: application/json&#39; \
--header &#39;Authorization: Bearer {token}&#39; \
--data &#39;{
}&#39;`</pre>
</td>
<td><p>Response: </p>
<ul>
<li><p>HTTP Status 200</p></li>
</ul>
<p> As the document failed to enrich the body should have:</p>
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`[
    ...
    {
        "_id": "69525cc874a6244c3df1933c",
        "_retryEnrichment": 0,
        "_enrichmentStarted": "2025-12-29T10:49:43.250Z",
        "documentId": "69525c9c74a6244c3df1931a"
    },
    ...
]`</pre>
</td>
<td></td>
<td><p> </p></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

## Test cases

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>**Test Case#:** 3</p></td>
<td><p>**Test Case Name:** Enricher first delivery - Happy cases</p></td>
</tr>
<tr>
<td><p>**System:** LUZ-DOCS</p></td>
<td><p>**Subsystem:** LUZ-DOCS</p></td>
</tr>
<tr>
<td><p>**Test case designed by:**<br />
[[Kepler] - Liem Doan](https://axonivy.atlassian.net/wiki/people/712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4?ref=confluence)</p></td>
<td><p>**Design Date:** 31 Dec 2025</p></td>
</tr>
<tr>
<td><p>**Short description: **</p></td>
<td><p>Currently, the ONE API has a special delivery flow for marketing letters that requires the enrichment process to be completed **before** the letter is delivered to the recipient.</p>
<p>However, luz-docs is not currently designed for this behavior. In luz-docs, enrichment is not a mandatory finish step for document creation.</p>
<p>To support this marketing letter use case, ONE API and luz-docs need to be aligned so that delivery is not failed due to enrichment process</p></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td><p>**Pre – conditions:**</p></td>
</tr>
<tr>
<td><ul>
<li><p>Have valid token for authentication:</p>
<ul>
<li><p>username: [liem.doanvanthanh@axonactive.com](mailto:liem.doanvanthanh@axonactive.com)</p></li>
<li><p>password: `Liem18112000`</p></li>
</ul></li>
<li><p>Have a company type tenant:</p>
<ul>
<li><p>tenantid: 6b884e9b-2612-47fa-954e-666e7d3bab13</p></li>
</ul></li>
<li><p>Port forward for API Forwarder:</p>
<ul>
<li><p>Run this in cmd: kubectl port-forward --address 0.0.0.0 services/api-forwarder 8080:8080 -n dev</p></li>
</ul></li>
<li><p>Port forward luz-vault:</p>
<ul>
<li><p>Run this in cmd: kubectl port-forward --address 0.0.0.0 services/luz-vault 8200:8200 -n dev</p></li>
</ul></li>
<li><p>**For test purpose:**</p>
<ul>
<li><p>**Please deploy it to dev env first**</p></li>
</ul></li>
</ul></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td><p>**Post – conditions:**</p></td>
</tr>
<tr>
<td><ul>
<li><p>A new document must be created with enricher status is success</p></li>
</ul></td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

### Test Steps

<table style="width:100%;">
<colgroup>
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
</colgroup>
<tbody>
<tr>
<td rowspan="2"><p>**Step**</p></td>
<td rowspan="2"><p>**Action**</p></td>
<td rowspan="2"><p>**Expected Behavior**</p></td>
<td rowspan="2"><p>**Actual Behavior**</p></td>
<td rowspan="2"><p>**Comment**</p></td>
<td colspan="2"><p>**Results**</p></td>
</tr>
<tr>
<td><p>As expected</p></td>
<td><p>not as expected</p></td>
</tr>
<tr>
<td><p>1</p></td>
<td><p>Run this cURL to get access token</p>
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`curl --location --request POST &#39;http://localhost:8080/luzsec/api/{{tenantid}}/tokens&#39; \
--header &#39;Authorization: Basic bGllbS5kb2FudmFudGhhbmhAYXhvbmFjdGl2ZS5jb206TGllbTE4MTEyMDAw&#39; `</pre>
</td>
<td><p>HTTP Status 201 with valid token</p>
<ul>
<li><p>`"token"`: the access token to use for all below step</p></li>
</ul>
> [!note]- Sample body
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`{
>     "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzUxMiJ9.eyJzdWIiOiJsaWVtLmRvYW52YW50aGFuaEBheG9uYWN0aXZlLmNvbSIsImNvbXBhbnktdGVuYW50Ijp7ImNvbXBhbnlJbmZvSWQiOjg4MTAsImNvbXBhbnlJZCI6MSwiY29tcGFueU5hbWUiOiJMSU5EQSIsInN0YXR1cyI6IkFDVElWQVRFRCIsInN0YXR1c0V4cGlyeURhdGUiOm51bGwsImNvbXBhbnlUeXBlIjoiQlVTSU5FU1MiLCJpZCI6MTg5NTc3LCJyb2xlcyI6WyJvYmplY3RfbWFuYWdlciIsImNvbXBhbnlfYWRtaW5pc3RyYXRvciIsIm15a2xhcmFfdXNlciJdLCJ0ZW5hbnRJZCI6IjZiODg0ZTliLTI2MTItNDdmYS05NTRlLTY2NmU3ZDNiYWIxMyIsInVzZXJuYW1lIjoibGllbS5kb2FudmFudGhhbmhAYXhvbmFjdGl2ZS5jb20iLCJuYW1lIjpudWxsLCJ0eXBlIjoiQ09NUEFOWSIsImNyZWF0ZURhdGUiOm51bGwsInVwZGF0ZURhdGUiOm51bGwsImV4cGlyZURhdGUiOm51bGwsImV4cGlyZVRpbWUiOm51bGx9LCJpc3MiOiJjb20uYXhvbml2eSIsInRlbmFudElkIjoiNmI4ODRlOWItMjYxMi00N2ZhLTk1NGUtNjY2ZTdkM2JhYjEzIiwidXNlcl9yb2xlcyI6WyJFdmVyeWJvZHkiXSwicGVyc29uLXRlbmFudCI6eyJpZCI6MCwicm9sZXMiOlsiY29tcGFueV9hZG1pbmlzdHJhdG9yIl0sInRlbmFudElkIjoiMjc1OTUzYzAtNzdjYS00OGEyLThhZTAtNjhhMGI2MGJlZWZlIiwidXNlcm5hbWUiOiJsaWVtLmRvYW52YW50aGFuaEBheG9uYWN0aXZlLmNvbSIsIm5hbWUiOm51bGwsInR5cGUiOiJQRVJTT04iLCJjcmVhdGVEYXRlIjoiV2VkIE9jdCAyMiAxMDoyNTo0OCBDRVNUIDIwMjUiLCJ1cGRhdGVEYXRlIjpudWxsLCJleHBpcmVEYXRlIjpudWxsLCJleHBpcmVUaW1lIjpudWxsfSwiZXhwIjoxNzY3MjExNTQzLCJpYXQiOjE3NjcxNjgzNDMsInNlY3VyaXR5X2NsYXNzZXMiOlsiUkVBRE9OTFlfMSJdfQ.S3fwP4Ft8dsHYyYEopEDFyJb-XcfKdwcZTWkGRY2Y5JiAtWIBkncZAxPlCIAYmw-2aLsHMxb8yUQNRgaOxTTPvQ5fwRUUqcufHNuRlgTCda6dFas9bVgUXAj1Gqt3J9Fvg_cWW9LbeRSIcPh0q8VoPiqr9xx13bTUugLahA6y86lWftXtjj3jfjvI1IDalUU7JH1OtYZe82vC3yEcrqAd2B94ICQu9AdgHZ4EgprDvFNtAtSJMAJR7Lnft4xofCUNa6qEqDyhipBdw5THzfNYFLpPMsh3cVTHmcjim8poawCK1H45nHvieXIPtdvEEjZMiAipWh61IRFKUfJa13U1g",
>     "publicKey": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAvAROyHbXkbhmx7ZsIwIl2sglqmZKpQKgnwtdytfQ/p5rwx4WX3KVuzt82+nwHUpddGneSlAlG6Ob3aV9kRjVvOfkACg8Wi9KSCbn2qYSCU5SVCVjl0e+rPZZfKlOY3RT3O5JStovB/N26LFwXtgxvcs2toOtnP+E6h6PKjyZrSJrjLjXJvCr6t65BjD9qyJvhEccPOCiPwJuQvjlO+hcfks+Hdlsgagn/b0oSZYvCoLQ9sY8Eu2umXPQu8aBxuD+bISrtbNKoQtao5Z1l8IeI7raxi3kEKy2f5jqdkcJOa2qVlHe6qN7xZpKeLh4jrWmL5lc6j3Cc4Cb1dsb9GYqXwIDAQAB",
>     "refreshToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzUxMiJ9.eyJzdWIiOiJsaWVtLmRvYW52YW50aGFuaEBheG9uYWN0aXZlLmNvbSIsImlzcyI6ImNvbS5heG9uaXZ5IiwidGVuYW50SWQiOiI2Yjg4NGU5Yi0yNjEyLTQ3ZmEtOTU0ZS02NjZlN2QzYmFiMTMiLCJ1c2VyX3JvbGVzIjpbIm9iamVjdF9tYW5hZ2VyIiwiY29tcGFueV9hZG1pbmlzdHJhdG9yIiwibXlrbGFyYV91c2VyIiwiRXZlcnlib2R5Il0sImV4cCI6MTc4MjcyMDM0MywiaWF0IjoxNzY3MTY4MzQzLCJqdGkiOiI4NjYwZGZiYi1iMWI4LTRjNDItODk0OS0yNDI3ODBhNTViNTkifQ.JYA3bElMXVatw-C-5u00S7e-AK3fPwexLJOljTbptc0GbwfODdOTlsNrWPNNZpl1mUrYck_Z3QqwWRpRJYGSnISO2iJ7tdENyDdZ-KG9biFPc_pysEp3p4_yeGzRaFticm1IiP_0bxUi98CGjdC2TvssW3trEDqilyo1lxp22jz3o5KqPndUMlDhkTgFhIHAuxWeMFcI8_sWIzCdscn-9qM-sk6RkkDPBkMiguCmVDeqLMSflA3pKmL-948XvVU1JFZ6VXUbqgs8o4mXU9add9BeXK1hpLf04VrFqxCkio6qINyD26HbJOdytrUUMBNeZEI4ivKyMmqC68vMYxHtHQ"
> }`</pre>
</td>
<td></td>
<td></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/LUZ-DOCS/attachments/adapt-to-support-one-api-enricher-first-delivery/check.png]]</p></td>
<td><p>🚫</p></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>Run this cURL to create docuemnt in tenant</p>
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: powershell; gutter: false; theme: Confluence" data-theme="Confluence">`curl --location  --request POST &#39;http://localhost:8147/luz_docs/api/{tenantid}/documents?enricherPriority=URGENT&#39; \
--header &#39;Authorization: Bearer {token}&#39;\
--form &#39;files=@"postman-cloud:///1f0e1495-c56f-43f0-9112-dacbf35824e4"&#39; \
--form &#39;metadata="{
  \"documentTitle\": \"Document with test rerun enrich\"
}"&#39;`</pre>
</td>
<td><p>Response: </p>
<ul>
<li><p>HTTP Status 201</p></li>
</ul>
<p>Sample Body:</p>
<ul>
<li><p>"_id": "69525c9c74a6244c3df1931a"</p></li>
<li><p>"_isEnriched": false</p></li>
<li><p>"isEnricherFirst": true</p></li>
</ul>
> [!note]- Detail body
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`{
>     "_id": "69525c9c74a6244c3df1931a",
>     "_createdBy": "liem.doanvanthanh@axonactive.com",
>     "_createdDate": "2025-12-29T10:48:59.481Z",
>     "_updatedBy": "liem.doanvanthanh@axonactive.com",
>     "_updatedDate": "2025-12-29T10:48:59.481Z",
>     "_deletionStatus": "false",
>     "_files": {
>         "reference": {
>             "_updatedDate": "2025-12-29T10:48:59.481Z",
>             "_sizeInBytes": 48525,
>             "_createdDate": "2025-12-29T10:48:59.481Z",
>             "_scanningTime": "2025-12-29T10:48:58.899287Z",
>             "_antivirusVersion": "ClamAV 1.4.3/27863/Sun Dec 28 08:26:03 2025",
>             "_scanningResult": "OK",
>             "_link": {
>                 "self": "6b884e9b-2612-47fa-954e-666e7d3bab13/documents/69525c9c74a6244c3df1931a/files/reference"
>             }
>         }
>     },
>     "_isBasicDocument": false,
>     "_isBeingCreated": false,
>     "_isEnriched": false,
>     "_isLargeFile": false,
>     "_isTemporaryDocument": false,
>     "_versionNumber": 1,
>     "folderIds": [],
>     "name": "review_test_plan.pdf",
>     "documentTitle": "Document with test rerun enrich",
>     "isEnricherFirst": true,
>     "_link": {
>         "self": "6b884e9b-2612-47fa-954e-666e7d3bab13/documents/69525c9c74a6244c3df1931a"
>     }
> }`</pre>
</td>
<td></td>
<td><p> </p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>3</p></td>
<td><p>Run this cURL to check the final status of document :</p>
<ul>
<li><p>docid: "69525c9c74a6244c3df1931a"</p></li>
<li><p>tenantid: 6b884e9b-2612-47fa-954e-666e7d3bab13</p></li>
</ul>
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`curl --location &#39;http://localhost:8147/luz_docs/api/{tenantid}/documents/{docid}?exclude-total-count=false&folder-id=string&include-deleted-documents=true&include-file=false&include-folder-name=true&skip-security-classes=true&sort=string&#39; \
--header &#39;credential-token: string&#39; \
--header &#39;Accept: application/json&#39; \
--header &#39;Authorization: Bearer {token}&#39;`</pre>
</td>
<td><p> Response: </p>
<ul>
<li><p>HTTP Status 200</p></li>
</ul>
<p>The body must have:</p>
<ul>
<li><p>`reference`: content type enricher success</p></li>
<li><p>`referenceTsq` and `referenceTsr`: Timestamp enricher success</p></li>
<li><p>`thumbnail128` and `thumbnail256` and `thumbnail512`: Thumbnail success</p></li>
</ul>
<p>Sample body:</p>
<ul>
<li><p>"_id": "69525c9c74a6244c3df1931a"</p></li>
<li><p>"_isEnriched": true</p></li>
<li><p>"isEnricherFirst": true</p></li>
</ul>
> [!note]- Detail body
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`{
>     "_id": "69525c9c74a6244c3df1931a",
>     "_createdBy": "liem.doanvanthanh@axonactive.com",
>     "_createdDate": "2025-12-29T10:48:59.481Z",
>     "_updatedBy": "liem.doanvanthanh@axonactive.com",
>     "_updatedDate": "2025-12-29T10:49:19.945Z",
>     "_deletionStatus": "false",
>     "_files": {
>         "reference": {
>             "_updatedDate": "2025-12-29T10:48:59.481Z",
>             "_sizeInBytes": 48525,
>             "_createdDate": "2025-12-29T10:48:59.481Z",
>             "_scanningTime": "2025-12-29T10:48:58.899287Z",
>             "_antivirusVersion": "ClamAV 1.4.3/27863/Sun Dec 28 08:26:03 2025",
>             "_scanningResult": "OK",
>             "contentType": "application/pdf",
>             "_link": {
>                 "self": "6b884e9b-2612-47fa-954e-666e7d3bab13/documents/69525c9c74a6244c3df1931a/files/reference"
>             }
>         },
>         "referenceTsq": {
>             "_createdDate": "2025-12-29T10:49:13.637Z",
>             "_sizeInBytes": 89,
>             "_updatedDate": "2025-12-29T10:49:13.637Z",
>             "contentType": "application/timestamp-query",
>             "_link": {
>                 "self": "6b884e9b-2612-47fa-954e-666e7d3bab13/documents/69525c9c74a6244c3df1931a/files/referenceTsq"
>             }
>         },
>         "referenceTsr": {
>             "_createdDate": "2025-12-29T10:49:13.638Z",
>             "_sizeInBytes": 3478,
>             "_updatedDate": "2025-12-29T10:49:13.638Z",
>             "contentType": "application/timestamp-reply",
>             "_link": {
>                 "self": "6b884e9b-2612-47fa-954e-666e7d3bab13/documents/69525c9c74a6244c3df1931a/files/referenceTsr"
>             }
>         },
>         "thumbnail128": {
>             "_createdDate": "2025-12-29T10:49:16.302Z",
>             "_sizeInBytes": 6559,
>             "_updatedDate": "2025-12-29T10:49:16.302Z",
>             "contentType": "image/jpeg",
>             "height": 128,
>             "width": 90,
>             "_link": {
>                 "self": "6b884e9b-2612-47fa-954e-666e7d3bab13/documents/69525c9c74a6244c3df1931a/files/thumbnail128"
>             }
>         },
>         "thumbnail256": {
>             "_createdDate": "2025-12-29T10:49:16.302Z",
>             "_sizeInBytes": 19903,
>             "_updatedDate": "2025-12-29T10:49:16.302Z",
>             "contentType": "image/jpeg",
>             "height": 256,
>             "width": 181,
>             "_link": {
>                 "self": "6b884e9b-2612-47fa-954e-666e7d3bab13/documents/69525c9c74a6244c3df1931a/files/thumbnail256"
>             }
>         },
>         "thumbnail512": {
>             "_createdDate": "2025-12-29T10:49:16.302Z",
>             "_sizeInBytes": 67313,
>             "_updatedDate": "2025-12-29T10:49:16.302Z",
>             "contentType": "image/jpeg",
>             "height": 512,
>             "width": 361,
>             "_link": {
>                 "self": "6b884e9b-2612-47fa-954e-666e7d3bab13/documents/69525c9c74a6244c3df1931a/files/thumbnail512"
>             }
>         }
>     },
>     "_isBasicDocument": false,
>     "_isBeingCreated": false,
>     "_isEnriched": true,
>     "_isLargeFile": false,
>     "_isTemporaryDocument": false,
>     "_versionNumber": 2,
>     "folderIds": [],
>     "name": "review_test_plan.pdf",
>     "documentTitle": "Document with test rerun enrich",
>     "isEnricherFirst": true,
>     "_folders": [],
>     "_link": {
>         "self": "6b884e9b-2612-47fa-954e-666e7d3bab13/documents/69525c9c74a6244c3df1931a"
>     }
> }`</pre>
> [!note]- Technical Log
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`03:13:51,183 INFO  [ch.klara.luz.docs.service.DocumentService] (default task-21) [fireAsyncEnrichmentEvent] Triggering enrichment for document 695c6fdec71adc00af1aa4f6 of tenant 6b884e9b-2612-47fa-954e-666e7d3bab13 with priority URGENT`</pre>
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`03:13:50,767 INFO  [ch.klara.luz.docs.service.DocumentCreatingService] (default task-21) [createDocument] Document created: 695c6fdec71adc00af1aa4f6 for tenant 6b884e9b-2612-47fa-954e-666e7d3bab13`</pre>
</td>
<td></td>
<td><p> </p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>4</p></td>
<td><p>Run this cURL to check the final status of the document :</p>
<ul>
<li><p>tenantid: 6b884e9b-2612-47fa-954e-666e7d3bab13</p></li>
</ul>
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`curl --location &#39;localhost:8080/luz_jsonstore/api/mdb/6b884e9b-2612-47fa-954e-666e7d3bab13/enrichmentstatus&#39; \
--header &#39;Content-Type: application/json&#39; \
--header &#39;Authorization: Bearer {token}&#39; \
--data &#39;{
}&#39;`</pre>
</td>
<td><p>Response: </p>
<ul>
<li><p>HTTP Status 200</p></li>
</ul>
<p> As the document should not found</p></td>
<td></td>
<td><p> </p></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------
