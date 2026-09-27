---
ai_hash: 16379983778c84b0
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49118380054'
confluence_path: Team Kepler > Test Case Library > Test Execution / Evidences > Sprint
  149 - Test Report (0.03.14.xx)
created: 2026-02-05
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- luz-docs
- sprint
- testing
- enricher
title: '[LUZ-Docs] - Execution - Trigger enricher regarding document type'
type: source
updated: 2026-02-06
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49118380054/LUZ-Docs+-+Execution+-+Trigger+enricher+regarding+document+type
---

# [LUZ-Docs] - Execution - Trigger enricher regarding document type

*Confluence source · Team Kepler › Test Case Library › Test Execution / Evidences › Sprint 149 - Test Report (0.03.14.xx) · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49118380054/LUZ-Docs+-+Execution+-+Trigger+enricher+regarding+document+type) · updated 2026-02-06*

## Test cases

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>**Test Case#:** 1</p></td>
<td><p>**Test Case Name:** Enrich Invoice document - Happy case 01</p></td>
</tr>
<tr>
<td><p>**System:** LUZ-DOCS</p></td>
<td><p>**Subsystem:** LUZ-DOCS</p></td>
</tr>
<tr>
<td><p>**Test case designed by:**<br />
[[Kepler] - Liem Doan](https://axonivy.atlassian.net/wiki/people/712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4?ref=confluence)</p></td>
<td><p>**Design Date:** 05 Feb 2026</p></td>
</tr>
<tr>
<td><p>**Short description: **</p></td>
<td><p>**As a user,**<br />
I want document data to be enriched automatically by the system regardless of the document type, so that all relevant metadata (including invoice data) is always available for the client to display when needed.</p>
<p>The **luz-docs** backend service enriches documents automatically and independently of the document type selected in the client UI.</p>
<ul>
<li><p>The Enricher is executed as part of the backend processing flow and **is not triggered by document type changes** from the GUI.</p></li>
<li><p>Enrichment is performed **for all documents type**, regardless of whether the document type is set to *Invoice* or another type.</p></li>
<li><p>As a result, invoice-related metadata (e.g. invoice number, total amount, extracted fields) is **always available** for retrieval by the client.</p></li>
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
<td><p>**Pre – conditions:**</p></td>
</tr>
<tr>
<td><ul>
<li><p>Have valid account for authentication:</p>
<ul>
<li><p>username: admin</p></li>
<li><p>password: `admin`</p></li>
</ul></li>
<li><p>Have a company type tenant:</p>
<ul>
<li><p>tenantid: 00a04daf-f2b3-41d5-8c12-2d1b4c48a36a</p></li>
</ul></li>
<li><p>An Invoice document:</p>
<ul>
<li><p>[[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Sprint 149 - Test Report (0.03.14.xx)/attachments/luz-docs-execution-trigger-enricher-regarding-document-type/Invoice_no._407822_1767154264830.pdf|Invoice_no._407822_1767154264830.pdf]]</p></li>
</ul></li>
<li><p>Port forward for API Forwarder:</p>
<ul>
<li><p>Run this in cmd: kubectl port-forward --address 0.0.0.0 services/api-forwarder 8080:8080 -n dev</p></li>
</ul></li>
<li><p>Port forward luz-vault:</p>
<ul>
<li><p>Run this in cmd: kubectl port-forward --address 0.0.0.0 services/luz-vault 8200:8200 -n dev-f2b3-41d5-</p></li>
</ul></li>
<li><p>**For test purpose:**</p>
<ul>
<li><p>**As our team only handle backend so this will be test as API calls**</p></li>
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
<li><p>A new document must be created with enricher status is true and contain “Invoice Data“</p></li>
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
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`curl --location --request POST &#39;http://localhost:8080/luzsec/api/00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/access/tokens?type=all-tenant&#39; \
--header &#39;Authorization: Basic YWRtaW46YWRtaW4=&#39;`</pre>
</td>
<td><p>HTTP Status 201 with valid token</p>
<ul>
<li><p>`"token"`: the access token to use for all below step</p></li>
</ul>
> [!note]- Sample body
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`{
>     "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzUxMiJ9.eyJzdWIiOiJhZG1pbiIsImNvbXBhbnktdGVuYW50Ijp7ImNvbXBhbnlJbmZvSWQiOjIzOTksImNvbXBhbnlJZCI6MSwiY29tcGFueU5hbWUiOiJLTEFSQSBCdXNpbmVzcyBBRyIsInN0YXR1cyI6IkFDVElWQVRFRCIsInN0YXR1c0V4cGlyeURhdGUiOm51bGwsImNvbXBhbnlUeXBlIjoiQlVTSU5FU1MiLCJpZCI6MTUwNDYyLCJyb2xlcyI6WyJjb21wYW55X2FkbWluaXN0cmF0b3IiLCJlbXBsb3llZSIsImV4dGVybmFsX3VzZXIiXSwidGVuYW50SWQiOiIwMGEwNGRhZi1mMmIzLTQxZDUtOGMxMi0yZDFiNGM0OGEzNmEiLCJ1c2VybmFtZSI6IiIsIm5hbWUiOm51bGwsInR5cGUiOiJDT01QQU5ZIiwiY3JlYXRlRGF0ZSI6bnVsbCwidXBkYXRlRGF0ZSI6bnVsbCwiZXhwaXJlRGF0ZSI6bnVsbCwiZXhwaXJlVGltZSI6bnVsbH0sImlzcyI6ImNvbS5heG9uaXZ5IiwidGVuYW50SWQiOiIwMGEwNGRhZi1mMmIzLTQxZDUtOGMxMi0yZDFiNGM0OGEzNmEiLCJ1c2VyX3JvbGVzIjpbImtsYXJhX25ld3NfYWRtaW4iLCJrbGFyYV9hZG1pbmlzdHJhdGlvbl9hZG1pbiIsImtsYXJhX2xvZ3NfYWRtaW4iLCJrbGFyYV9jdXJyZW50X2NvbXBhbnlfYWRtaW4iLCJrbGFyYV93aWRnZXRfc3RvcmVfYWRtaW4iLCJrbGFyYV92YXJpYWJsZXNfYWRtaW4iLCJrbGFyYV9hY2NvdW50aW5nX2FkbWluIiwia2xhcmFfcGF5cm9sbF9hZG1pbiIsImtsYXJhX3N0YXRpc3RpY19hZG1pbiIsIk5ld3NfQWRtaW4iLCJBZG1pbmlzdHJhdG9yIiwiYWxsX3RlbmFudHNfYWNjZXNzIiwiRXZlcnlib2R5Il0sInBlcnNvbi10ZW5hbnQiOnsiaWQiOjAsInJvbGVzIjpbXSwidGVuYW50SWQiOiJhMWY3ZTdiYS0wNmVjLTQ3NGQtYjJmZC04OWFhZjIzNWNhMzciLCJ1c2VybmFtZSI6ImFkbWluIiwibmFtZSI6bnVsbCwidHlwZSI6IlBFUlNPTiIsImNyZWF0ZURhdGUiOiJXZWQgTm92IDMwIDExOjA3OjU5IENFVCAyMDE2IiwidXBkYXRlRGF0ZSI6bnVsbCwiZXhwaXJlRGF0ZSI6bnVsbCwiZXhwaXJlVGltZSI6bnVsbH0sImV4cCI6MTc3MDMwNjE5NCwiaWF0IjoxNzcwMjYyOTk0LCJzZWN1cml0eV9jbGFzc2VzIjpbXX0.cF_cBMlVpOqmcnaLtl3DN8gKixCFUw3uQ9hh3ZL3Lq4EhYx5p4uBIPSIKOXk5jJH00R-ZSSxz3xnCxZXzql2INiO2PQ2tCrTkbJCJ4xeDo7kVY4hhetBEKr-ctkTVzF_ywoxtZlaI3-tNGZvDAAWMI2705ekQBVz9p-ZHNQKbHV1xfMHY9MfdKem0OW3IL9iWy8FvASl9gFkIGkfal36Zxg5A-_OyMqOag0a4wyrirGqmllbQlCM_PhVk6ZoGf4QAC4BVQXx5Cf3BszfFf5zTgS_M_sCp1lcMLelT2gm7n5hrYyJuUcTCO3Xr1hCfMuZ3ekMjlMdgLhCXahL04gaSg",
>     "publicKey": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAvAROyHbXkbhmx7ZsIwIl2sglqmZKpQKgnwtdytfQ/p5rwx4WX3KVuzt82+nwHUpddGneSlAlG6Ob3aV9kRjVvOfkACg8Wi9KSCbn2qYSCU5SVCVjl0e+rPZZfKlOY3RT3O5JStovB/N26LFwXtgxvcs2toOtnP+E6h6PKjyZrSJrjLjXJvCr6t65BjD9qyJvhEccPOCiPwJuQvjlO+hcfks+Hdlsgagn/b0oSZYvCoLQ9sY8Eu2umXPQu8aBxuD+bISrtbNKoQtao5Z1l8IeI7raxi3kEKy2f5jqdkcJOa2qVlHe6qN7xZpKeLh4jrWmL5lc6j3Cc4Cb1dsb9GYqXwIDAQAB"
> }`</pre>
</td>
<td></td>
<td></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Sprint 149 - Test Report (0.03.14.xx)/attachments/luz-docs-execution-trigger-enricher-regarding-document-type/check.png]]</p></td>
<td></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>Run this cURL to create document in tenant with token</p>
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: powershell; gutter: false; theme: Confluence" data-theme="Confluence">`curl --location &#39;http://localhost:8080/luz_docs/api/00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents&#39; \
--header &#39;Authorization: Bearer <token> \
--form &#39;files=@"/C:/Users/dvtliem/Downloads/Invoice_no._407822_1767154264830.pdf"&#39; \
--form &#39;metadata="{
  \"documentTitle\": \"Invoice_no._407822_1767154264830.pdf\",
}"&#39;`</pre>
</td>
<td><p>Response: </p>
<ul>
<li><p>HTTP Status 201</p></li>
</ul>
<p>Sample Body:</p>
<p>"_id": "`6984205271afad05e5d1f273`"</p>
> [!note]- Detail body
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`{
>     "_id": "6984205271afad05e5d1f273",
>     "_createdBy": "admin",
>     "_createdDate": "2026-02-05T03:47:01.284Z",
>     "_updatedBy": "admin",
>     "_updatedDate": "2026-02-05T03:47:01.284Z",
>     "_deletionStatus": "false",
>     "_files": {
>         "reference": {
>             "_updatedDate": "2026-02-05T03:47:01.284Z",
>             "_sizeInBytes": 255911,
>             "_createdDate": "2026-02-05T03:47:01.284Z",
>             "_scanningTime": "2026-02-05T03:47:01.279721Z",
>             "_antivirusVersion": "ClamAV 1.4.3/27902/Wed Feb  4 08:24:02 2026",
>             "_scanningResult": "OK",
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/698412b571afad05e5d1d4a2/files/reference"
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
>     "name": "Invoice_no._407822_1767154264830.pdf",
>     "documentTitle": "Invoice_no._407822_1767154264830.pdf",
>     "enricherPriority": "URGENT",
>     "_link": {
>         "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/698412b571afad05e5d1d4a2"
>     }
> }`</pre>
> [!note]- Technical Log
> <p>Create document:</p>
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: bash; gutter: false; theme: Confluence" data-theme="Confluence">`04:58:11,276 INFO  [ch.klara.luz.docs.service.DocumentCreatingService] (default task-1) [createDocument] Document created: 6984205271afad05e5d1f273 for tenant 00a04daf-f2b3-41d5-8c12-2d1b4c48a36a
> 04:58:18,332 INFO  [ch.klara.luz.docs.service.DocumentCreatingService] (default task-1) [createDocument] Successfully create document for tenantId: 00a04daf-f2b3-41d5-8c12-2d1b4c48a36a; documentId: 6984205271afad05e5d1f273; fileSize: 48525`</pre>
> <p>The enricher process is triggered:</p>
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`04:58:18,322 INFO  [ch.klara.luz.docs.service.DocumentService] (default task-1) [fireAsyncEnrichmentEvent] Triggering enrichment for document 6984205271afad05e5d1f273 of tenant 00a04daf-f2b3-41d5-8c12-2d1b4c48a36a with priority DEFAULT`</pre>
> <p>The status is added to enrichment status:</p>
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`06:09:40,923 INFO  [javax.ws.rs.client.ClientResponseFilter] (default task-1) [PUT] - http://host.docker.internal:8080/luz_jsonstore/api/mdb/6b884e9b-2612-47fa-954e-666e7d3bab13/enrichmentstatus/add headers=[Connection=close,Content-Length=150,Content-Type=application/json,Date=Mon, 05 Jan 2026 05:09:40 GMT,Server=nginx/1.29.4,x-envoy-upstream-service-time=9] status-code=200 time-consuming=1044`</pre>
</td>
<td></td>
<td><p> </p></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Sprint 149 - Test Report (0.03.14.xx)/attachments/luz-docs-execution-trigger-enricher-regarding-document-type/check.png]]</p></td>
<td></td>
</tr>
<tr>
<td><p>3</p></td>
<td><p>Run this cURL to check the final status of document :</p>
<ul>
<li><p>docid: `6984205271afad05e5d1f273`</p></li>
<li><p>tenantid: 00a04daf-f2b3-41d5-8c12-2d1b4c48a36a</p></li>
</ul>
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`curl --location &#39;http://localhost:8080/luz_docs/api/{tenantid}/documents/{docid}?exclude-total-count=false&folder-id=string&include-deleted-documents=true&include-file=false&include-folder-name=true&skip-security-classes=true&sort=string&#39; \
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
<li><p>`_files.reference`: content type enricher success</p></li>
<li><p>`_files.referenceTsq`,`_files`.`referenceTsr`: Timestamp enricher success</p></li>
<li><p>`_files.thumbnail128`,`_files.thumbnail256` ,`_files.thumbnail512`: Thumbnail success</p></li>
<li><p>`_files.layoutAndText`, `documentDescription`, `documentTextContent`: AI enricher success</p></li>
<li><p>invoideData: look like this sample</p></li>
</ul>
> [!note]- Details invoice data sample
> ![[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Sprint 149 - Test Report (0.03.14.xx)/attachments/luz-docs-execution-trigger-enricher-regarding-document-type/fc4aff1f-16e3-4815-9ead-a7f1e707286f.png]]
<p>Sample body:</p>
<ul>
<li><p>"_id": "`698412b571afad05e5d1d4a2`"</p></li>
<li><p>"_isEnriched": true</p></li>
</ul>
> [!note]- Detail body
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`{
>     "_id": "6984205271afad05e5d1f273",
>     "_createdBy": "admin",
>     "_createdDate": "2026-02-05T04:45:06.588Z",
>     "_updatedBy": "admin",
>     "_updatedDate": "2026-02-05T04:45:24.094Z",
>     "_deletionStatus": "false",
>     "_files": {
>         "reference": {
>             "_updatedDate": "2026-02-05T04:45:06.588Z",
>             "_sizeInBytes": 255911,
>             "_createdDate": "2026-02-05T04:45:06.588Z",
>             "_scanningTime": "2026-02-05T04:45:06.585916Z",
>             "_antivirusVersion": "ClamAV 1.4.3/27902/Wed Feb  4 08:24:02 2026",
>             "_scanningResult": "OK",
>             "contentType": "application/pdf",
>             "pdfVersion": "1.4",
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/6984205271afad05e5d1f273/files/reference"
>             }
>         },
>         "referenceTsq": {
>             "_createdDate": "2026-02-05T04:45:07.658Z",
>             "_sizeInBytes": 89,
>             "_updatedDate": "2026-02-05T04:45:07.658Z",
>             "contentType": "application/timestamp-query",
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/6984205271afad05e5d1f273/files/referenceTsq"
>             }
>         },
>         "referenceTsr": {
>             "_createdDate": "2026-02-05T04:45:07.658Z",
>             "_sizeInBytes": 3477,
>             "_updatedDate": "2026-02-05T04:45:07.658Z",
>             "contentType": "application/timestamp-reply",
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/6984205271afad05e5d1f273/files/referenceTsr"
>             }
>         },
>         "thumbnail128": {
>             "_createdDate": "2026-02-05T04:45:08.102Z",
>             "_sizeInBytes": 5727,
>             "_updatedDate": "2026-02-05T04:45:08.102Z",
>             "contentType": "image/jpeg",
>             "height": 128,
>             "width": 90,
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/6984205271afad05e5d1f273/files/thumbnail128"
>             }
>         },
>         "thumbnail256": {
>             "_createdDate": "2026-02-05T04:45:08.102Z",
>             "_sizeInBytes": 13630,
>             "_updatedDate": "2026-02-05T04:45:08.102Z",
>             "contentType": "image/jpeg",
>             "height": 256,
>             "width": 181,
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/6984205271afad05e5d1f273/files/thumbnail256"
>             }
>         },
>         "thumbnail512": {
>             "_createdDate": "2026-02-05T04:45:08.102Z",
>             "_sizeInBytes": 38594,
>             "_updatedDate": "2026-02-05T04:45:08.102Z",
>             "contentType": "image/jpeg",
>             "height": 512,
>             "width": 361,
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/6984205271afad05e5d1f273/files/thumbnail512"
>             }
>         },
>         "layoutAndText": {
>             "_createdDate": "2026-02-05T04:45:23.898Z",
>             "_sizeInBytes": 214735,
>             "_updatedDate": "2026-02-05T04:45:23.898Z",
>             "contentType": "application/x.abbyy.fre10+xml",
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/6984205271afad05e5d1f273/files/layoutAndText"
>             }
>         }
>     },
>     "_isBasicDocument": false,
>     "_isBeingCreated": false,
>     "_isEnriched": true,
>     "_isLargeFile": false,
>     "_isTemporaryDocument": false,
>     "_versionNumber": 3,
>     "folderIds": [],
>     "name": "Invoice_no._407822_1767154264830.pdf",
>     "documentTitle": "Invoice_no._407822_1767154264830.pdf",
>     "_referenceFileCreatedDate": "2025-12-31T04:11:40.000Z",
>     "_referenceFileUpdatedDate": "2025-12-31T04:11:40.000Z",
>     "contentLanguage": "en",
>     "documentDescription": "Rechnung von KLARA Business AG",
>     "documentTextContent": "klara business schlössli schönegg wilhelmshöhe 6003 luzern technical business ringstrasse 3270 aarberg luzern 2025-12-31 che-103.727.240 vat reference reference payable delivery 2026-01-30 invoice 407822 item description pos quantity unit discount price vat % 0000016169 consumption widget store consumption widget store 8.65 8.65 1.00 unit 0.00 8.10 8.65 8.10 0.70 vat % 9.35 0.00 terms payment payable 30 days postfinance chf 3030 bern klara business post company schlössli schönegg wilhelmshöhe 6003 luzern support@klara.ch klara business schlössli schönegg 6003 luzern schweiz technical business ringstrasse 3270 aarberg 2025-12-31 407822 invoice invoice subject payment payment qr iban iban chf 9.35 2026-01-30 ch4530000001162080216 ch9409000000162080216 postfinance 3030 bern klara business schlössli schönegg 6003 luzern schweiz payable reference 000000407822030390511010990 detailed statement invoice 407822 technical business ringstrasse 3270 aarberg 2025-12-31 11 2025 product performance period quantity chf price excl chf price incl vat % provider deleted pcode earchive bus usage wcode earchivebusinessconsumption en earchive consumption klara business 863 8.63 8.1 8.63 0.00 8.63 0.00 2025-12-31 05 11",
>     "documentTypes": [
>         "Invoice"
>     ],
>     "invoiceData": {
>         "amount": "9.35",
>         "currency": "CHF",
>         "dueDate": "2026-01-30",
>         "documentReferenceDate": "2025-12-31",
>         "uid": "CHE-103.727.240",
>         "vatRate": "8.1",
>         "creditor": {
>             "name": "KLARA Business AG",
>             "geoZip": "6003",
>             "geoCity": "Luzern",
>             "iban": "CH4530000001162080216",
>             "email": "support@klara.ch",
>             "geoAddressText": "KLARA Business AG, 6003 Luzern",
>             "geoAddressLines": [
>                 {
>                     "geoAddressLine": "KLARA Business AG"
>                 },
>                 {
>                     "geoAddressLine": "6003 Luzern"
>                 }
>             ]
>         },
>         "swissQrBill": {
>             "qrType": "QRR",
>             "qrReference": "000000407822030390511010990"
>         },
>         "itemList": [
>             {
>                 "item": {
>                     "amount": "8.65",
>                     "index": "0",
>                     "text": "Consumption Widget Store"
>                 }
>             },
>             {
>                 "item": {
>                     "amount": "8.65",
>                     "index": "1"
>                 }
>             }
>         ],
>         "klaraAccgBct": [
>             {
>                 "60_OTHER_COST": "HIGH"
>             },
>             {
>                 "40_MATERIAL_EXP": "MEDIUM"
>             },
>             {
>                 "58_OTHER_LABOR_EXP": "MEDIUM"
>             },
>             {
>                 "CAPEX_TANGIBLE_ASSET": "MEDIUM"
>             }
>         ],
>         "klaraAccgTag": [
>             {
>                 "information technology": "HIGH"
>             },
>             {
>                 "__AMBIGUOUS__": "MEDIUM"
>             },
>             {
>                 "administrative expenses": "MEDIUM"
>             },
>             {
>                 "other operating expenses": "MEDIUM"
>             }
>         ]
>     },
>     "title": "Invoice 407822",
>     "_folders": [],
>     "_link": {
>         "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/6984205271afad05e5d1f273"
>     }
> }`</pre>
</td>
<td></td>
<td><p> </p></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Sprint 149 - Test Report (0.03.14.xx)/attachments/luz-docs-execution-trigger-enricher-regarding-document-type/check.png]]</p></td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>**Test Case#:** 2</p></td>
<td><p>**Test Case Name:** Enrich Invoice document - Happy case 02</p></td>
</tr>
<tr>
<td><p>**System:** LUZ-DOCS</p></td>
<td><p>**Subsystem:** LUZ-DOCS</p></td>
</tr>
<tr>
<td><p>**Test case designed by:**<br />
[[Kepler] - Liem Doan](https://axonivy.atlassian.net/wiki/people/712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4?ref=confluence)</p></td>
<td><p>**Design Date:** 05 Feb 2026</p></td>
</tr>
<tr>
<td><p>**Short description: **</p></td>
<td><p>**As a user,**<br />
I want document data to be enriched automatically by the system regardless of the document type, so that all relevant metadata (including invoice data) is always available for the client to display when needed.</p>
<p>The **luz-docs** backend service enriches documents automatically and independently of the document type selected in the client UI.</p>
<ul>
<li><p>The Enricher is executed as part of the backend processing flow and **is not triggered by document type changes** from the GUI.</p></li>
<li><p>Enrichment is performed **for all documents type**, regardless of whether the document type is set to *Invoice* or another type.</p></li>
<li><p>As a result, invoice-related metadata (e.g. invoice number, total amount, extracted fields) is **always available** for retrieval by the client.</p></li>
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
<td><p>**Pre – conditions:**</p></td>
</tr>
<tr>
<td><ul>
<li><p>Have valid account for authentication:</p>
<ul>
<li><p>username: admin</p></li>
<li><p>password: `admin`</p></li>
</ul></li>
<li><p>Have a company type tenant:</p>
<ul>
<li><p>tenantid: 00a04daf-f2b3-41d5-8c12-2d1b4c48a36a</p></li>
</ul></li>
<li><p>An Invoice document:</p>
<ul>
<li><p>[[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Sprint 149 - Test Report (0.03.14.xx)/attachments/luz-docs-execution-trigger-enricher-regarding-document-type/Invoice_no._407822_1767154264830.pdf|Invoice_no._407822_1767154264830.pdf]]</p></li>
</ul></li>
<li><p>Set document types are Contract</p></li>
<li><p>Port forward for API Forwarder:</p>
<ul>
<li><p>Run this in cmd: kubectl port-forward --address 0.0.0.0 services/api-forwarder 8080:8080 -n dev</p></li>
</ul></li>
<li><p>Port forward luz-vault:</p>
<ul>
<li><p>Run this in cmd: kubectl port-forward --address 0.0.0.0 services/luz-vault 8200:8200 -n dev-f2b3-41d5-</p></li>
</ul></li>
<li><p>**For test purpose:**</p>
<ul>
<li><p>**As our team only handle backend so this will be test as API calls**</p></li>
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
<li><p>A new document must be created with enricher status is true and contain “Invoice Data“</p></li>
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
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`curl --location --request POST &#39;http://localhost:8080/luzsec/api/00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/access/tokens?type=all-tenant&#39; \
--header &#39;Authorization: Basic YWRtaW46YWRtaW4=&#39;`</pre>
</td>
<td><p>HTTP Status 201 with valid token</p>
<ul>
<li><p>`"token"`: the access token to use for all below step</p></li>
</ul>
> [!note]- Sample body
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`{
>     "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzUxMiJ9.eyJzdWIiOiJhZG1pbiIsImNvbXBhbnktdGVuYW50Ijp7ImNvbXBhbnlJbmZvSWQiOjIzOTksImNvbXBhbnlJZCI6MSwiY29tcGFueU5hbWUiOiJLTEFSQSBCdXNpbmVzcyBBRyIsInN0YXR1cyI6IkFDVElWQVRFRCIsInN0YXR1c0V4cGlyeURhdGUiOm51bGwsImNvbXBhbnlUeXBlIjoiQlVTSU5FU1MiLCJpZCI6MTUwNDYyLCJyb2xlcyI6WyJjb21wYW55X2FkbWluaXN0cmF0b3IiLCJlbXBsb3llZSIsImV4dGVybmFsX3VzZXIiXSwidGVuYW50SWQiOiIwMGEwNGRhZi1mMmIzLTQxZDUtOGMxMi0yZDFiNGM0OGEzNmEiLCJ1c2VybmFtZSI6IiIsIm5hbWUiOm51bGwsInR5cGUiOiJDT01QQU5ZIiwiY3JlYXRlRGF0ZSI6bnVsbCwidXBkYXRlRGF0ZSI6bnVsbCwiZXhwaXJlRGF0ZSI6bnVsbCwiZXhwaXJlVGltZSI6bnVsbH0sImlzcyI6ImNvbS5heG9uaXZ5IiwidGVuYW50SWQiOiIwMGEwNGRhZi1mMmIzLTQxZDUtOGMxMi0yZDFiNGM0OGEzNmEiLCJ1c2VyX3JvbGVzIjpbImtsYXJhX25ld3NfYWRtaW4iLCJrbGFyYV9hZG1pbmlzdHJhdGlvbl9hZG1pbiIsImtsYXJhX2xvZ3NfYWRtaW4iLCJrbGFyYV9jdXJyZW50X2NvbXBhbnlfYWRtaW4iLCJrbGFyYV93aWRnZXRfc3RvcmVfYWRtaW4iLCJrbGFyYV92YXJpYWJsZXNfYWRtaW4iLCJrbGFyYV9hY2NvdW50aW5nX2FkbWluIiwia2xhcmFfcGF5cm9sbF9hZG1pbiIsImtsYXJhX3N0YXRpc3RpY19hZG1pbiIsIk5ld3NfQWRtaW4iLCJBZG1pbmlzdHJhdG9yIiwiYWxsX3RlbmFudHNfYWNjZXNzIiwiRXZlcnlib2R5Il0sInBlcnNvbi10ZW5hbnQiOnsiaWQiOjAsInJvbGVzIjpbXSwidGVuYW50SWQiOiJhMWY3ZTdiYS0wNmVjLTQ3NGQtYjJmZC04OWFhZjIzNWNhMzciLCJ1c2VybmFtZSI6ImFkbWluIiwibmFtZSI6bnVsbCwidHlwZSI6IlBFUlNPTiIsImNyZWF0ZURhdGUiOiJXZWQgTm92IDMwIDExOjA3OjU5IENFVCAyMDE2IiwidXBkYXRlRGF0ZSI6bnVsbCwiZXhwaXJlRGF0ZSI6bnVsbCwiZXhwaXJlVGltZSI6bnVsbH0sImV4cCI6MTc3MDMwNjE5NCwiaWF0IjoxNzcwMjYyOTk0LCJzZWN1cml0eV9jbGFzc2VzIjpbXX0.cF_cBMlVpOqmcnaLtl3DN8gKixCFUw3uQ9hh3ZL3Lq4EhYx5p4uBIPSIKOXk5jJH00R-ZSSxz3xnCxZXzql2INiO2PQ2tCrTkbJCJ4xeDo7kVY4hhetBEKr-ctkTVzF_ywoxtZlaI3-tNGZvDAAWMI2705ekQBVz9p-ZHNQKbHV1xfMHY9MfdKem0OW3IL9iWy8FvASl9gFkIGkfal36Zxg5A-_OyMqOag0a4wyrirGqmllbQlCM_PhVk6ZoGf4QAC4BVQXx5Cf3BszfFf5zTgS_M_sCp1lcMLelT2gm7n5hrYyJuUcTCO3Xr1hCfMuZ3ekMjlMdgLhCXahL04gaSg",
>     "publicKey": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAvAROyHbXkbhmx7ZsIwIl2sglqmZKpQKgnwtdytfQ/p5rwx4WX3KVuzt82+nwHUpddGneSlAlG6Ob3aV9kRjVvOfkACg8Wi9KSCbn2qYSCU5SVCVjl0e+rPZZfKlOY3RT3O5JStovB/N26LFwXtgxvcs2toOtnP+E6h6PKjyZrSJrjLjXJvCr6t65BjD9qyJvhEccPOCiPwJuQvjlO+hcfks+Hdlsgagn/b0oSZYvCoLQ9sY8Eu2umXPQu8aBxuD+bISrtbNKoQtao5Z1l8IeI7raxi3kEKy2f5jqdkcJOa2qVlHe6qN7xZpKeLh4jrWmL5lc6j3Cc4Cb1dsb9GYqXwIDAQAB"
> }`</pre>
</td>
<td></td>
<td></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Sprint 149 - Test Report (0.03.14.xx)/attachments/luz-docs-execution-trigger-enricher-regarding-document-type/check.png]]</p></td>
<td></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>Run this cURL to create document in tenant with token</p>
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: powershell; gutter: false; theme: Confluence" data-theme="Confluence">`curl --location &#39;http://localhost:8080/luz_docs/api/00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents&#39; \
--header &#39;Authorization: ••••••&#39; \
--form &#39;files=@"/C:/Users/dvtliem/Downloads/Invoice_no._407822_1767154264830.pdf"&#39; \
--form &#39;metadata="{
  \"documentTitle\": \"Invoice_no._407822_1767154264830.pdf\",
\"documentTypes\": [\"Contract\"]
}"&#39;`</pre>
</td>
<td><p>Response: </p>
<ul>
<li><p>HTTP Status 201</p></li>
</ul>
<p>Sample Body:</p>
<p>"_id": "`69842ac471afad05e5d20c45`"</p>
> [!note]- Detail body
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`{
>     "_id": "69842ac471afad05e5d20c45",
>     "_createdBy": "admin",
>     "_createdDate": "2026-02-05T05:29:40.776Z",
>     "_updatedBy": "admin",
>     "_updatedDate": "2026-02-05T05:29:40.776Z",
>     "_deletionStatus": "false",
>     "_files": {
>         "reference": {
>             "_updatedDate": "2026-02-05T05:29:40.776Z",
>             "_sizeInBytes": 255911,
>             "_createdDate": "2026-02-05T05:29:40.776Z",
>             "_scanningTime": "2026-02-05T05:29:40.772981Z",
>             "_antivirusVersion": "ClamAV 1.4.3/27902/Wed Feb  4 08:24:02 2026",
>             "_scanningResult": "OK",
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/69842ac471afad05e5d20c45/files/reference"
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
>     "name": "Invoice_no._407822_1767154264830.pdf",
>     "documentTitle": "Invoice_no._407822_1767154264830.pdf",
>     "documentTypes": [
>         "Contract"
>     ],
>     "_link": {
>         "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/69842ac471afad05e5d20c45"
>     }
> }`</pre>
</td>
<td></td>
<td><p> </p></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Sprint 149 - Test Report (0.03.14.xx)/attachments/luz-docs-execution-trigger-enricher-regarding-document-type/check.png]]</p></td>
<td></td>
</tr>
<tr>
<td><p>3</p></td>
<td><p>Run this cURL to check the final status of document :</p>
<ul>
<li><p>docid: `69842ac471afad05e5d20c45`</p></li>
<li><p>tenantid: 00a04daf-f2b3-41d5-8c12-2d1b4c48a36a</p></li>
</ul>
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`curl --location &#39;http://localhost:8080/luz_docs/api/{tenantid}/documents/{docid}?exclude-total-count=false&folder-id=string&include-deleted-documents=true&include-file=false&include-folder-name=true&skip-security-classes=true&sort=string&#39; \
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
<li><p>`_files.reference`: content type enricher success</p></li>
<li><p>`_files.referenceTsq`,`_files`.`referenceTsr`: Timestamp enricher success</p></li>
<li><p>`_files.thumbnail128`,`_files.thumbnail256` ,`_files.thumbnail512`: Thumbnail success</p></li>
<li><p>`_files.layoutAndText`, `documentDescription`, `documentTextContent`: AI enricher success</p></li>
<li><p>invoideData: look like this sample</p></li>
</ul>
> [!note]- Details invoice data sample
> ![[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Sprint 149 - Test Report (0.03.14.xx)/attachments/luz-docs-execution-trigger-enricher-regarding-document-type/fc4aff1f-16e3-4815-9ead-a7f1e707286f.png]]
<p>Sample body:</p>
<ul>
<li><p>"_id": "`69842ac471afad05e5d20c45`"</p></li>
<li><p>"_isEnriched": true</p></li>
</ul>
> [!note]- Detail body
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`{
>     "_id": "69842ac471afad05e5d20c45",
>     "_createdBy": "admin",
>     "_createdDate": "2026-02-05T05:29:40.776Z",
>     "_updatedBy": "admin",
>     "_updatedDate": "2026-02-05T05:29:58.671Z",
>     "_deletionStatus": "false",
>     "_files": {
>         "reference": {
>             "_updatedDate": "2026-02-05T05:29:40.776Z",
>             "_sizeInBytes": 255911,
>             "_createdDate": "2026-02-05T05:29:40.776Z",
>             "_scanningTime": "2026-02-05T05:29:40.772981Z",
>             "_antivirusVersion": "ClamAV 1.4.3/27902/Wed Feb  4 08:24:02 2026",
>             "_scanningResult": "OK",
>             "contentType": "application/pdf",
>             "pdfVersion": "1.4",
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/69842ac471afad05e5d20c45/files/reference"
>             }
>         },
>         "referenceTsq": {
>             "_createdDate": "2026-02-05T05:29:41.327Z",
>             "_sizeInBytes": 89,
>             "_updatedDate": "2026-02-05T05:29:41.327Z",
>             "contentType": "application/timestamp-query",
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/69842ac471afad05e5d20c45/files/referenceTsq"
>             }
>         },
>         "referenceTsr": {
>             "_createdDate": "2026-02-05T05:29:41.327Z",
>             "_sizeInBytes": 3478,
>             "_updatedDate": "2026-02-05T05:29:41.327Z",
>             "contentType": "application/timestamp-reply",
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/69842ac471afad05e5d20c45/files/referenceTsr"
>             }
>         },
>         "thumbnail128": {
>             "_createdDate": "2026-02-05T05:29:41.727Z",
>             "_sizeInBytes": 5727,
>             "_updatedDate": "2026-02-05T05:29:41.727Z",
>             "contentType": "image/jpeg",
>             "height": 128,
>             "width": 90,
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/69842ac471afad05e5d20c45/files/thumbnail128"
>             }
>         },
>         "thumbnail256": {
>             "_createdDate": "2026-02-05T05:29:41.727Z",
>             "_sizeInBytes": 13630,
>             "_updatedDate": "2026-02-05T05:29:41.727Z",
>             "contentType": "image/jpeg",
>             "height": 256,
>             "width": 181,
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/69842ac471afad05e5d20c45/files/thumbnail256"
>             }
>         },
>         "thumbnail512": {
>             "_createdDate": "2026-02-05T05:29:41.727Z",
>             "_sizeInBytes": 38594,
>             "_updatedDate": "2026-02-05T05:29:41.727Z",
>             "contentType": "image/jpeg",
>             "height": 512,
>             "width": 361,
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/69842ac471afad05e5d20c45/files/thumbnail512"
>             }
>         },
>         "layoutAndText": {
>             "_createdDate": "2026-02-05T05:29:58.446Z",
>             "_sizeInBytes": 214743,
>             "_updatedDate": "2026-02-05T05:29:58.446Z",
>             "contentType": "application/x.abbyy.fre10+xml",
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/69842ac471afad05e5d20c45/files/layoutAndText"
>             }
>         }
>     },
>     "_isBasicDocument": false,
>     "_isBeingCreated": false,
>     "_isEnriched": true,
>     "_isLargeFile": false,
>     "_isTemporaryDocument": false,
>     "_versionNumber": 3,
>     "folderIds": [],
>     "name": "Invoice_no._407822_1767154264830.pdf",
>     "documentTitle": "Invoice_no._407822_1767154264830.pdf",
>     "documentTypes": [
>         "Contract"
>     ],
>     "_referenceFileCreatedDate": "2025-12-31T04:11:40.000Z",
>     "_referenceFileUpdatedDate": "2025-12-31T04:11:40.000Z",
>     "contentLanguage": "en",
>     "documentTextContent": "klara business schlössli schönegg wilhelmshöhe 6003 luzern technical business ringstrasse 3270 aarberg luzern 2025-12-31 che-103.727.240 vat reference reference 2026-01-30 payable delivery invoice 407822 item description pos quantity unit discount price vat % 0000016169 consumption widget store consumption widget store 8.65 8.65 1.00 unit 0.00 8.10 8.65 8.10 0.70 vat % 9.35 0.00 terms payment payable 30 days postfinance chf 3030 bern klara business post company schlössli schönegg wilhelmshöhe 6003 luzern support@klara.ch klara business schlössli schönegg 6003 luzern schweiz technical business ringstrasse 3270 aarberg 2025-12-31 407822 invoice invoice subject payment payment qr iban iban chf 9.35 2026-01-30 ch4530000001162080216 ch9409000000162080216 postfinance 3030 bern klara business schlössli schönegg 6003 luzern schweiz payable reference 000000407822030390511010990 detailed statement invoice 407822 technical business ringstrasse 3270 aarberg 2025-12-31 11 2025 product performance period quantity chf price excl chf price incl vat % provider deleted pcode earchive bus usage wcode earchivebusinessconsumption en earchive consumption klara business 863 8.63 8.1 8.63 0.00 8.63 0.00 2025-12-31 05 11",
>     "invoiceData": {
>         "amount": "9.35",
>         "currency": "CHF",
>         "dueDate": "2026-01-30",
>         "documentReferenceDate": "2025-12-31",
>         "uid": "CHE-103.727.240",
>         "vatRate": "8.1",
>         "creditor": {
>             "name": "KLARA Business AG",
>             "geoZip": "6003",
>             "geoCity": "Luzern",
>             "iban": "CH4530000001162080216",
>             "email": "support@klara.ch",
>             "geoAddressText": "KLARA Business AG, 6003 Luzern",
>             "geoAddressLines": [
>                 {
>                     "geoAddressLine": "KLARA Business AG"
>                 },
>                 {
>                     "geoAddressLine": "6003 Luzern"
>                 }
>             ]
>         },
>         "swissQrBill": {
>             "qrType": "QRR",
>             "qrReference": "000000407822030390511010990"
>         },
>         "itemList": [
>             {
>                 "item": {
>                     "amount": "8.65",
>                     "index": "0",
>                     "text": "Consumption Widget Store"
>                 }
>             },
>             {
>                 "item": {
>                     "amount": "8.65",
>                     "index": "1"
>                 }
>             }
>         ],
>         "klaraAccgBct": [
>             {
>                 "60_OTHER_COST": "HIGH"
>             },
>             {
>                 "40_MATERIAL_EXP": "MEDIUM"
>             },
>             {
>                 "58_OTHER_LABOR_EXP": "MEDIUM"
>             },
>             {
>                 "CAPEX_TANGIBLE_ASSET": "MEDIUM"
>             }
>         ],
>         "klaraAccgTag": [
>             {
>                 "information technology": "HIGH"
>             },
>             {
>                 "__AMBIGUOUS__": "MEDIUM"
>             },
>             {
>                 "administrative expenses": "MEDIUM"
>             },
>             {
>                 "other operating expenses": "MEDIUM"
>             }
>         ]
>     },
>     "title": "Invoice 407822",
>     "_folders": [],
>     "_link": {
>         "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/69842ac471afad05e5d20c45"
>     }
> }`</pre>
</td>
<td></td>
<td><p> </p></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Sprint 149 - Test Report (0.03.14.xx)/attachments/luz-docs-execution-trigger-enricher-regarding-document-type/check.png]]</p></td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
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
<td><p>**Test Case Name:** Enrich Non-Invoice Document - Negative Case 01</p></td>
</tr>
<tr>
<td><p>**System:** LUZ-DOCS</p></td>
<td><p>**Subsystem:** LUZ-DOCS</p></td>
</tr>
<tr>
<td><p>**Test case designed by:**<br />
[[Kepler] - Liem Doan](https://axonivy.atlassian.net/wiki/people/712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4?ref=confluence)</p></td>
<td><p>**Design Date:** 05 Feb 2026</p></td>
</tr>
<tr>
<td><p>**Short description: **</p></td>
<td><p>**As a user,**<br />
I want document data to be enriched automatically by the system regardless of the document type, so that all relevant metadata (including invoice data) is always available for the client to display when needed.</p>
<p>The **luz-docs** backend service enriches documents automatically and independently of the document type selected in the client UI.</p>
<ul>
<li><p>The Enricher is executed as part of the backend processing flow and **is not triggered by document type changes** from the GUI.</p></li>
<li><p>Enrichment is performed **for all documents type**, regardless of whether the document type is set to *Invoice* or another type.</p></li>
<li><p>As a result, invoice-related metadata (e.g. invoice number, total amount, extracted fields) is **always available** for retrieval by the client.</p></li>
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
<td><p>**Pre – conditions:**</p></td>
</tr>
<tr>
<td><ul>
<li><p>Have valid account for authentication:</p>
<ul>
<li><p>username: admin</p></li>
<li><p>password: `admin`</p></li>
</ul></li>
<li><p>Have a company type tenant:</p>
<ul>
<li><p>tenantid: 00a04daf-f2b3-41d5-8c12-2d1b4c48a36a</p></li>
</ul></li>
<li><p>A Non-Invoice File:</p>
<ul>
<li><p>[[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Sprint 149 - Test Report (0.03.14.xx)/attachments/luz-docs-execution-trigger-enricher-regarding-document-type/review_test_plan.pdf|review_test_plan.pdf]] </p></li>
</ul></li>
<li><p>Port forward for API Forwarder:</p>
<ul>
<li><p>Run this in cmd: kubectl port-forward --address 0.0.0.0 services/api-forwarder 8080:8080 -n dev</p></li>
</ul></li>
<li><p>Port forward luz-vault:</p>
<ul>
<li><p>Run this in cmd: kubectl port-forward --address 0.0.0.0 services/luz-vault 8200:8200 -n dev-f2b3-41d5-</p></li>
</ul></li>
<li><p>**For test purpose:**</p>
<ul>
<li><p>**As our team only handle backend so this will be test as API calls**</p></li>
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
<li><p>A new document must be created with enricher status is true and DO NOT contain “Invoice Data“</p></li>
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
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`curl --location --request POST &#39;http://localhost:8080/luzsec/api/00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/access/tokens?type=all-tenant&#39; \
--header &#39;Authorization: Basic YWRtaW46YWRtaW4=&#39;`</pre>
</td>
<td><p>HTTP Status 201 with valid token</p>
<ul>
<li><p>`"token"`: the access token to use for all below step</p></li>
</ul>
> [!note]- Sample body
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`{
>     "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzUxMiJ9.eyJzdWIiOiJhZG1pbiIsImNvbXBhbnktdGVuYW50Ijp7ImNvbXBhbnlJbmZvSWQiOjIzOTksImNvbXBhbnlJZCI6MSwiY29tcGFueU5hbWUiOiJLTEFSQSBCdXNpbmVzcyBBRyIsInN0YXR1cyI6IkFDVElWQVRFRCIsInN0YXR1c0V4cGlyeURhdGUiOm51bGwsImNvbXBhbnlUeXBlIjoiQlVTSU5FU1MiLCJpZCI6MTUwNDYyLCJyb2xlcyI6WyJjb21wYW55X2FkbWluaXN0cmF0b3IiLCJlbXBsb3llZSIsImV4dGVybmFsX3VzZXIiXSwidGVuYW50SWQiOiIwMGEwNGRhZi1mMmIzLTQxZDUtOGMxMi0yZDFiNGM0OGEzNmEiLCJ1c2VybmFtZSI6IiIsIm5hbWUiOm51bGwsInR5cGUiOiJDT01QQU5ZIiwiY3JlYXRlRGF0ZSI6bnVsbCwidXBkYXRlRGF0ZSI6bnVsbCwiZXhwaXJlRGF0ZSI6bnVsbCwiZXhwaXJlVGltZSI6bnVsbH0sImlzcyI6ImNvbS5heG9uaXZ5IiwidGVuYW50SWQiOiIwMGEwNGRhZi1mMmIzLTQxZDUtOGMxMi0yZDFiNGM0OGEzNmEiLCJ1c2VyX3JvbGVzIjpbImtsYXJhX25ld3NfYWRtaW4iLCJrbGFyYV9hZG1pbmlzdHJhdGlvbl9hZG1pbiIsImtsYXJhX2xvZ3NfYWRtaW4iLCJrbGFyYV9jdXJyZW50X2NvbXBhbnlfYWRtaW4iLCJrbGFyYV93aWRnZXRfc3RvcmVfYWRtaW4iLCJrbGFyYV92YXJpYWJsZXNfYWRtaW4iLCJrbGFyYV9hY2NvdW50aW5nX2FkbWluIiwia2xhcmFfcGF5cm9sbF9hZG1pbiIsImtsYXJhX3N0YXRpc3RpY19hZG1pbiIsIk5ld3NfQWRtaW4iLCJBZG1pbmlzdHJhdG9yIiwiYWxsX3RlbmFudHNfYWNjZXNzIiwiRXZlcnlib2R5Il0sInBlcnNvbi10ZW5hbnQiOnsiaWQiOjAsInJvbGVzIjpbXSwidGVuYW50SWQiOiJhMWY3ZTdiYS0wNmVjLTQ3NGQtYjJmZC04OWFhZjIzNWNhMzciLCJ1c2VybmFtZSI6ImFkbWluIiwibmFtZSI6bnVsbCwidHlwZSI6IlBFUlNPTiIsImNyZWF0ZURhdGUiOiJXZWQgTm92IDMwIDExOjA3OjU5IENFVCAyMDE2IiwidXBkYXRlRGF0ZSI6bnVsbCwiZXhwaXJlRGF0ZSI6bnVsbCwiZXhwaXJlVGltZSI6bnVsbH0sImV4cCI6MTc3MDMwNjE5NCwiaWF0IjoxNzcwMjYyOTk0LCJzZWN1cml0eV9jbGFzc2VzIjpbXX0.cF_cBMlVpOqmcnaLtl3DN8gKixCFUw3uQ9hh3ZL3Lq4EhYx5p4uBIPSIKOXk5jJH00R-ZSSxz3xnCxZXzql2INiO2PQ2tCrTkbJCJ4xeDo7kVY4hhetBEKr-ctkTVzF_ywoxtZlaI3-tNGZvDAAWMI2705ekQBVz9p-ZHNQKbHV1xfMHY9MfdKem0OW3IL9iWy8FvASl9gFkIGkfal36Zxg5A-_OyMqOag0a4wyrirGqmllbQlCM_PhVk6ZoGf4QAC4BVQXx5Cf3BszfFf5zTgS_M_sCp1lcMLelT2gm7n5hrYyJuUcTCO3Xr1hCfMuZ3ekMjlMdgLhCXahL04gaSg",
>     "publicKey": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAvAROyHbXkbhmx7ZsIwIl2sglqmZKpQKgnwtdytfQ/p5rwx4WX3KVuzt82+nwHUpddGneSlAlG6Ob3aV9kRjVvOfkACg8Wi9KSCbn2qYSCU5SVCVjl0e+rPZZfKlOY3RT3O5JStovB/N26LFwXtgxvcs2toOtnP+E6h6PKjyZrSJrjLjXJvCr6t65BjD9qyJvhEccPOCiPwJuQvjlO+hcfks+Hdlsgagn/b0oSZYvCoLQ9sY8Eu2umXPQu8aBxuD+bISrtbNKoQtao5Z1l8IeI7raxi3kEKy2f5jqdkcJOa2qVlHe6qN7xZpKeLh4jrWmL5lc6j3Cc4Cb1dsb9GYqXwIDAQAB"
> }`</pre>
</td>
<td></td>
<td></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Sprint 149 - Test Report (0.03.14.xx)/attachments/luz-docs-execution-trigger-enricher-regarding-document-type/check.png]]</p></td>
<td></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>Run this cURL to create document in tenant</p>
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: powershell; gutter: false; theme: Confluence" data-theme="Confluence">`curl --location &#39;http://localhost:8080/luz_docs/api/00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents&#39; \
--header &#39;Authorization: ••••••&#39; \
--form &#39;files=@"/C:/Users/dvtliem/Downloads/Invoice_no._407822_1767154264830.pdf"&#39; \
--form &#39;metadata="{
  \"documentTitle\": \"Invoice_no._407822_1767154264830.pdf\",
\"documentTypes\": [\"Contract\"]
}"&#39;`</pre>
</td>
<td><p>Response: </p>
<ul>
<li><p>HTTP Status 201</p></li>
</ul>
<p>Sample Body:</p>
<ul>
<li><p>"_id": "`698422d671afad05e5d1f7dc`"</p></li>
</ul>
> [!note]- Detail body
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`{
>     "_id": "69842c9b71afad05e5d20ffa",
>     "_createdBy": "admin",
>     "_createdDate": "2026-02-05T05:37:31.228Z",
>     "_updatedBy": "admin",
>     "_updatedDate": "2026-02-05T05:37:31.228Z",
>     "_deletionStatus": "false",
>     "_files": {
>         "reference": {
>             "_updatedDate": "2026-02-05T05:37:31.228Z",
>             "_sizeInBytes": 48525,
>             "_createdDate": "2026-02-05T05:37:31.228Z",
>             "_scanningTime": "2026-02-05T05:37:31.224182Z",
>             "_antivirusVersion": "ClamAV 1.4.3/27902/Wed Feb  4 08:24:02 2026",
>             "_scanningResult": "OK",
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/69842c9b71afad05e5d20ffa/files/reference"
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
>     "documentTitle": "Invoice_no._407822_1767154264830.pdf",
>     "documentTypes": [
>         "Invoice"
>     ],
>     "_link": {
>         "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/69842c9b71afad05e5d20ffa"
>     }
> }`</pre>
> [!note]- Technical Log
> <p>Create document:</p>
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: bash; gutter: false; theme: Confluence" data-theme="Confluence">`10:52:41,929 INFO  [ch.klara.luz.docs.service.DocumentCreatingService] (default task-1) [createDocument] Document created: 698422d671afad05e5d1f7dc for tenant 6b884e9b-2612-47fa-954e-666e7d3bab13
> 10:52:48,565 INFO  [ch.klara.luz.docs.service.DocumentCreatingService] (default task-1) [createDocument] Successfully create document for tenantId: 00a04daf-f2b3-41d5-8c12-2d1b4c48a36a; documentId: 698422d671afad05e5d1f7dc; fileSize: 48525`</pre>
> <p>The enricher is triggered:</p>
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`10:52:48,563 INFO  [ch.klara.luz.docs.service.DocumentService] (default task-1) [fireAsyncEnrichmentEvent] Triggering enrichment for document 698422d671afad05e5d1f7dc of tenant 00a04daf-f2b3-41d5-8c12-2d1b4c48a36a with priority DEFAULT`</pre>
> <p>The final status is added to enrichment status:</p>
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`10:53:05,308 INFO  [javax.ws.rs.client.ClientResponseFilter] (default task-1) [PUT] - http://host.docker.internal:8080/luz_jsonstore/api/mdb/00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/enrichmentstatus/add headers=[Connection=close,Content-Length=150,Content-Type=application/json,Date=Mon, 05 Jan 2026 09:53:05 GMT,Server=nginx/1.29.4,x-envoy-upstream-service-time=72] status-code=200 time-consuming=937`</pre>
</td>
<td></td>
<td><p> </p></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Sprint 149 - Test Report (0.03.14.xx)/attachments/luz-docs-execution-trigger-enricher-regarding-document-type/check.png]]</p></td>
<td></td>
</tr>
<tr>
<td><p>3</p></td>
<td><p>Run this cURL to check the final status of document :</p>
<ul>
<li><p>docid: "`698422d671afad05e5d1f7dc`"</p></li>
<li><p>tenantid: 00a04daf-f2b3-41d5-8c12-2d1b4c48a36a</p></li>
</ul>
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`curl --location &#39;http://localhost:8080/luz_docs/api/{tenantid}/documents/{docid}?exclude-total-count=false&folder-id=string&include-deleted-documents=true&include-file=false&include-folder-name=true&skip-security-classes=true&sort=string&#39; \
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
<li><p>`_files.reference: content type enricher success`</p></li>
<li><p>`_files.referenceTsq,_files.referenceTsr: Timestamp enricher success`</p></li>
<li><p>`_files.thumbnail128,_files.thumbnail256 ,_files.thumbnail512: Thumbnail success`</p></li>
<li><p>`_files.layoutAndText, documentDescription, documentTextContent: AI enricher success`</p></li>
<li><p>No “invoiceData“ files FOUND</p></li>
</ul>
<p>Sample body:</p>
> [!note]- Details invoice data sample
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`{
>     "_id": "698422d671afad05e5d1f7dc",
>     "_createdBy": "admin",
>     "_createdDate": "2026-02-05T04:55:50.658Z",
>     "_updatedBy": "admin",
>     "_updatedDate": "2026-02-05T04:56:06.297Z",
>     "_deletionStatus": "false",
>     "_files": {
>         "reference": {
>             "_updatedDate": "2026-02-05T04:55:50.658Z",
>             "_sizeInBytes": 48525,
>             "_createdDate": "2026-02-05T04:55:50.658Z",
>             "_scanningTime": "2026-02-05T04:55:50.644388Z",
>             "_antivirusVersion": "ClamAV 1.4.3/27902/Wed Feb  4 08:24:02 2026",
>             "_scanningResult": "OK",
>             "contentType": "application/pdf",
>             "pdfVersion": "1.4",
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/698422d671afad05e5d1f7dc/files/reference"
>             }
>         },
>         "referenceTsq": {
>             "_createdDate": "2026-02-05T04:55:51.719Z",
>             "_sizeInBytes": 89,
>             "_updatedDate": "2026-02-05T04:55:51.719Z",
>             "contentType": "application/timestamp-query",
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/698422d671afad05e5d1f7dc/files/referenceTsq"
>             }
>         },
>         "referenceTsr": {
>             "_createdDate": "2026-02-05T04:55:51.719Z",
>             "_sizeInBytes": 3477,
>             "_updatedDate": "2026-02-05T04:55:51.719Z",
>             "contentType": "application/timestamp-reply",
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/698422d671afad05e5d1f7dc/files/referenceTsr"
>             }
>         },
>         "thumbnail128": {
>             "_createdDate": "2026-02-05T04:55:53.484Z",
>             "_sizeInBytes": 6559,
>             "_updatedDate": "2026-02-05T04:55:53.484Z",
>             "contentType": "image/jpeg",
>             "height": 128,
>             "width": 90,
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/698422d671afad05e5d1f7dc/files/thumbnail128"
>             }
>         },
>         "thumbnail256": {
>             "_createdDate": "2026-02-05T04:55:53.485Z",
>             "_sizeInBytes": 19903,
>             "_updatedDate": "2026-02-05T04:55:53.485Z",
>             "contentType": "image/jpeg",
>             "height": 256,
>             "width": 181,
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/698422d671afad05e5d1f7dc/files/thumbnail256"
>             }
>         },
>         "thumbnail512": {
>             "_createdDate": "2026-02-05T04:55:53.485Z",
>             "_sizeInBytes": 67313,
>             "_updatedDate": "2026-02-05T04:55:53.485Z",
>             "contentType": "image/jpeg",
>             "height": 512,
>             "width": 361,
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/698422d671afad05e5d1f7dc/files/thumbnail512"
>             }
>         },
>         "layoutAndText": {
>             "_createdDate": "2026-02-05T04:56:06.142Z",
>             "_sizeInBytes": 977834,
>             "_updatedDate": "2026-02-05T04:56:06.142Z",
>             "contentType": "application/x.abbyy.fre10+xml",
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/698422d671afad05e5d1f7dc/files/layoutAndText"
>             }
>         }
>     },
>     "_isBasicDocument": false,
>     "_isBeingCreated": false,
>     "_isEnriched": true,
>     "_isLargeFile": false,
>     "_isTemporaryDocument": false,
>     "_versionNumber": 3,
>     "folderIds": [],
>     "name": "review_test_plan.pdf",
>     "documentTitle": "Invoice_no._407822_1767154264830.pdf",
>     "_referenceFileCreatedDate": "2025-10-24T02:07:58.000Z",
>     "_referenceFileUpdatedDate": "2026-02-05T04:56:06.140Z",
>     "contentLanguage": "en",
>     "documentTextContent": "qc leader review plan phase softbank qc leader review plan phase softbank assessment assessment plan demonstrates solid foundation structure comprehensive coverage key testing critical gaps requiring enhancement execution strengths strengths structure organized logical sections industry standards explicit scope management separation scope scope items risk identification proactive risk assessment mitigation strategies multi level testing combination component integration testing authentication focus dedicated security testing token validation structure explicit scope management risk identification multi level testing authentication focus critical issues critical issues missing details missing details issue plan references include 145 mentions detailed spreadsheet testrail xray suite deliverable reference document appendix impact validate coverage actual recommendation add summary minimum count planned endpoint include sample format template appendix reference specific document ids issue impact recommendation incomplete team lines 19 21 128 131 incomplete team lines 19 21 128 131 issue team names placeholders impact medium unclear accountability resource allocation recommendation populate actual names tbd expected assignment issue impact recommendation vague exit criteria 101 vague exit criteria 101 issue 95 % pass rate mentioned defined questions issue questions include runs failed tests remaining blocked tests counted failures recommendation clarify formula pass rate passed tests total tests blocked tests 100 recommendation traceability traceability issue mention requirements traceability matrix rtm impact medium verify requirements tested recommendation add requirement map api specification requirements issue impact recommendation gaps gaps missing data management details missing data management details current lists data types lacks data stored version control data data refresh strategy cycles creates maintains data recommendation add subsection data management covering storage versioning ownership current recommendation insufficient integration coverage insufficient integration coverage issue flows mentioned lines 73 75 missing scenarios cancel completed job delete job running concurrent operations job job list pagination filtering scenarios recommendation expand integration testing complete scenario matrix issue missing scenarios recommendation defect management process defect management process issue mentions logging defects define defect severity priority definitions defect workflow progress fixed verified closed procedures regression testing triggers recommendation add 11 defect management process issue recommendation missing metrics missing metrics issue defined metrics tracking progress recommendation add metrics covering daily execution rate defect detection rate defect turnaround time coverage percentage api availability uptime testing issue recommendation incomplete environment details incomplete environment details missing mention database data persistence layer specification postman collection version location api specification version reference 111 link actual specs mention logging monitoring tools debugging recommendation expand environment complete technical stack missing recommendation regression strategy regression strategy issue 121 mentions regression strategy defined questions issue questions tests regression regression selective regression triggers bug batched recommendation add subsection defining regression scope triggers recommendation moderate issues moderate issues 11 schedule concerns 11 schedule concerns observation day integration testing phase nov short scenarios risk critical defects late schedule compression compromise quality recommendation add day buffer clarify assumptions expected defect density observation risk recommendation 12 limited negative testing examples 12 limited negative testing examples issue mentions negative testing lacks specifics missing scenarios boundary testing shots max min values malformed json payloads sql injection attempts parameters extremely qasm files special characters job ids recommendation add subsection negative scenarios comprehensive list issue missing scenarios recommendation 13 smoke definition 13 smoke definition issue 98 mentions smoke entry criteria define includes recommendation add appendix smoke checklist verify endpoint returns -500 response valid token issue recommendation 14 missing status code validation details 14 missing status code validation details issue 32 mentions verifies status codes explain gap plan reference include status code mapping api spec recommendation add table mapping job status codes meanings submitted queued issue gap recommendation minor improvements minor improvements 15 language consistency 15 language consistency observation mixed vietnamese annotations lines 25 recommendation standardize english formal plan add glossary observation recommendation 16 format inconsistency 16 format inconsistency dates yyyy dd format issue start dates table start placeholders recommendation align schedule mark schedule dates issue recommendation 17 communication plan 17 communication plan missing status communicated daily weekly recommendation add status reporting cadence stakeholders missing recommendation 18 token expiry testing 18 token expiry testing 53 mentions expired token enhancement add time based testing scenarios token expires mid job execution enhancement recommendations summary recommendations summary timeline timeline priority priority action item action item owner owner execution week execution add count summary define exit criteria formula create requirements traceability matrix add defect management process lead lead lead lead week planning expand integration scenarios engineers phase week define regression strategy lead lead complete environment details setup phase devops lead engineers engineers add metrics dashboard document negative scenarios add smoke checklist week execution design execution approval recommendation approval recommendation conditional approval plan proceed design phase conditions status conditional approval status approve approve structure scope definition risk management requires updates requires updates address items execution recommend recommend address items ensure smooth execution track future track future items addressed execution phase steps steps lead update plan items 2025 27 peer review updated plan dev lead product owner finalize design traceability requirements conduct planning review meeting moving setup phase reviewed review reviewed review 2025 24 document version reviewed document version reviewed 1.0",
>     "title": "Markdown To PDF",
>     "_folders": [],
>     "_link": {
>         "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/698422d671afad05e5d1f7dc"
>     }
> }`</pre>
</td>
<td></td>
<td><p> </p></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Sprint 149 - Test Report (0.03.14.xx)/attachments/luz-docs-execution-trigger-enricher-regarding-document-type/check.png]]</p></td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
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
<td><p>**Test Case#:** 4</p></td>
<td><p>**Test Case Name:** Enrich Non-Invoice Document - Negative Case 02</p></td>
</tr>
<tr>
<td><p>**System:** LUZ-DOCS</p></td>
<td><p>**Subsystem:** LUZ-DOCS</p></td>
</tr>
<tr>
<td><p>**Test case designed by:**<br />
[[Kepler] - Liem Doan](https://axonivy.atlassian.net/wiki/people/712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4?ref=confluence)</p></td>
<td><p>**Design Date:** 05 Feb 2026</p></td>
</tr>
<tr>
<td><p>**Short description: **</p></td>
<td><p>**As a user,**<br />
I want document data to be enriched automatically by the system regardless of the document type, so that all relevant metadata (including invoice data) is always available for the client to display when needed.</p>
<p>The **luz-docs** backend service enriches documents automatically and independently of the document type selected in the client UI.</p>
<ul>
<li><p>The Enricher is executed as part of the backend processing flow and **is not triggered by document type changes** from the GUI.</p></li>
<li><p>Enrichment is performed **for all documents type**, regardless of whether the document type is set to *Invoice* or another type.</p></li>
<li><p>As a result, invoice-related metadata (e.g. invoice number, total amount, extracted fields) is **always available** for retrieval by the client.</p></li>
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
<td><p>**Pre – conditions:**</p></td>
</tr>
<tr>
<td><ul>
<li><p>Have valid account for authentication:</p>
<ul>
<li><p>username: admin</p></li>
<li><p>password: `admin`</p></li>
</ul></li>
<li><p>Have a company type tenant:</p>
<ul>
<li><p>tenantid: 00a04daf-f2b3-41d5-8c12-2d1b4c48a36a</p></li>
</ul></li>
<li><p>A Non-Invoice File:</p>
<ul>
<li><p>[[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Sprint 149 - Test Report (0.03.14.xx)/attachments/luz-docs-execution-trigger-enricher-regarding-document-type/review_test_plan.pdf|review_test_plan.pdf]] </p></li>
</ul></li>
<li><p>Document types are “Invoice“</p></li>
<li><p>Port forward for API Forwarder:</p>
<ul>
<li><p>Run this in cmd: kubectl port-forward --address 0.0.0.0 services/api-forwarder 8080:8080 -n dev</p></li>
</ul></li>
<li><p>Port forward luz-vault:</p>
<ul>
<li><p>Run this in cmd: kubectl port-forward --address 0.0.0.0 services/luz-vault 8200:8200 -n dev-f2b3-41d5-</p></li>
</ul></li>
<li><p>**For test purpose:**</p>
<ul>
<li><p>**As our team only handle backend so this will be test as API calls**</p></li>
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
<li><p>A new document must be created with enricher status is true and DO NOT contain “Invoice Data“</p></li>
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
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`curl --location --request POST &#39;http://localhost:8080/luzsec/api/00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/access/tokens?type=all-tenant&#39; \
--header &#39;Authorization: Basic YWRtaW46YWRtaW4=&#39;`</pre>
</td>
<td><p>HTTP Status 201 with valid token</p>
<ul>
<li><p>`"token"`: the access token to use for all below step</p></li>
</ul>
> [!note]- Sample body
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`{
>     "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzUxMiJ9.eyJzdWIiOiJhZG1pbiIsImNvbXBhbnktdGVuYW50Ijp7ImNvbXBhbnlJbmZvSWQiOjIzOTksImNvbXBhbnlJZCI6MSwiY29tcGFueU5hbWUiOiJLTEFSQSBCdXNpbmVzcyBBRyIsInN0YXR1cyI6IkFDVElWQVRFRCIsInN0YXR1c0V4cGlyeURhdGUiOm51bGwsImNvbXBhbnlUeXBlIjoiQlVTSU5FU1MiLCJpZCI6MTUwNDYyLCJyb2xlcyI6WyJjb21wYW55X2FkbWluaXN0cmF0b3IiLCJlbXBsb3llZSIsImV4dGVybmFsX3VzZXIiXSwidGVuYW50SWQiOiIwMGEwNGRhZi1mMmIzLTQxZDUtOGMxMi0yZDFiNGM0OGEzNmEiLCJ1c2VybmFtZSI6IiIsIm5hbWUiOm51bGwsInR5cGUiOiJDT01QQU5ZIiwiY3JlYXRlRGF0ZSI6bnVsbCwidXBkYXRlRGF0ZSI6bnVsbCwiZXhwaXJlRGF0ZSI6bnVsbCwiZXhwaXJlVGltZSI6bnVsbH0sImlzcyI6ImNvbS5heG9uaXZ5IiwidGVuYW50SWQiOiIwMGEwNGRhZi1mMmIzLTQxZDUtOGMxMi0yZDFiNGM0OGEzNmEiLCJ1c2VyX3JvbGVzIjpbImtsYXJhX25ld3NfYWRtaW4iLCJrbGFyYV9hZG1pbmlzdHJhdGlvbl9hZG1pbiIsImtsYXJhX2xvZ3NfYWRtaW4iLCJrbGFyYV9jdXJyZW50X2NvbXBhbnlfYWRtaW4iLCJrbGFyYV93aWRnZXRfc3RvcmVfYWRtaW4iLCJrbGFyYV92YXJpYWJsZXNfYWRtaW4iLCJrbGFyYV9hY2NvdW50aW5nX2FkbWluIiwia2xhcmFfcGF5cm9sbF9hZG1pbiIsImtsYXJhX3N0YXRpc3RpY19hZG1pbiIsIk5ld3NfQWRtaW4iLCJBZG1pbmlzdHJhdG9yIiwiYWxsX3RlbmFudHNfYWNjZXNzIiwiRXZlcnlib2R5Il0sInBlcnNvbi10ZW5hbnQiOnsiaWQiOjAsInJvbGVzIjpbXSwidGVuYW50SWQiOiJhMWY3ZTdiYS0wNmVjLTQ3NGQtYjJmZC04OWFhZjIzNWNhMzciLCJ1c2VybmFtZSI6ImFkbWluIiwibmFtZSI6bnVsbCwidHlwZSI6IlBFUlNPTiIsImNyZWF0ZURhdGUiOiJXZWQgTm92IDMwIDExOjA3OjU5IENFVCAyMDE2IiwidXBkYXRlRGF0ZSI6bnVsbCwiZXhwaXJlRGF0ZSI6bnVsbCwiZXhwaXJlVGltZSI6bnVsbH0sImV4cCI6MTc3MDMwNjE5NCwiaWF0IjoxNzcwMjYyOTk0LCJzZWN1cml0eV9jbGFzc2VzIjpbXX0.cF_cBMlVpOqmcnaLtl3DN8gKixCFUw3uQ9hh3ZL3Lq4EhYx5p4uBIPSIKOXk5jJH00R-ZSSxz3xnCxZXzql2INiO2PQ2tCrTkbJCJ4xeDo7kVY4hhetBEKr-ctkTVzF_ywoxtZlaI3-tNGZvDAAWMI2705ekQBVz9p-ZHNQKbHV1xfMHY9MfdKem0OW3IL9iWy8FvASl9gFkIGkfal36Zxg5A-_OyMqOag0a4wyrirGqmllbQlCM_PhVk6ZoGf4QAC4BVQXx5Cf3BszfFf5zTgS_M_sCp1lcMLelT2gm7n5hrYyJuUcTCO3Xr1hCfMuZ3ekMjlMdgLhCXahL04gaSg",
>     "publicKey": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAvAROyHbXkbhmx7ZsIwIl2sglqmZKpQKgnwtdytfQ/p5rwx4WX3KVuzt82+nwHUpddGneSlAlG6Ob3aV9kRjVvOfkACg8Wi9KSCbn2qYSCU5SVCVjl0e+rPZZfKlOY3RT3O5JStovB/N26LFwXtgxvcs2toOtnP+E6h6PKjyZrSJrjLjXJvCr6t65BjD9qyJvhEccPOCiPwJuQvjlO+hcfks+Hdlsgagn/b0oSZYvCoLQ9sY8Eu2umXPQu8aBxuD+bISrtbNKoQtao5Z1l8IeI7raxi3kEKy2f5jqdkcJOa2qVlHe6qN7xZpKeLh4jrWmL5lc6j3Cc4Cb1dsb9GYqXwIDAQAB"
> }`</pre>
</td>
<td></td>
<td></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Sprint 149 - Test Report (0.03.14.xx)/attachments/luz-docs-execution-trigger-enricher-regarding-document-type/check.png]]</p></td>
<td></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>Run this cURL to create document in tenant</p>
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: powershell; gutter: false; theme: Confluence" data-theme="Confluence">`curl --location &#39;http://localhost:8080/luz_docs/api/00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents&#39; \
--header &#39;Authorization: Bearer <token>&#39; \
--form &#39;files=@"/C:/Users/dvtliem/Documents/review_test_plan.pdf"&#39; \
--form &#39;metadata="{
  \"documentTitle\": \"Invoice_no._407822_1767154264830.pdf\"
}"&#39;`</pre>
</td>
<td><p>Response: </p>
<ul>
<li><p>HTTP Status 201</p></li>
</ul>
<p>Sample Body:</p>
<ul>
<li><p>"_id": "`69843b1671afad05e5d22d2d`"</p></li>
</ul>
> [!note]- Detail body
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`{
>     "_id": "69843b1671afad05e5d22d2d",
>     "_createdBy": "admin",
>     "_createdDate": "2026-02-05T05:37:31.228Z",
>     "_updatedBy": "admin",
>     "_updatedDate": "2026-02-05T05:37:31.228Z",
>     "_deletionStatus": "false",
>     "_files": {
>         "reference": {
>             "_updatedDate": "2026-02-05T05:37:31.228Z",
>             "_sizeInBytes": 48525,
>             "_createdDate": "2026-02-05T05:37:31.228Z",
>             "_scanningTime": "2026-02-05T05:37:31.224182Z",
>             "_antivirusVersion": "ClamAV 1.4.3/27902/Wed Feb  4 08:24:02 2026",
>             "_scanningResult": "OK",
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/69842c9b71afad05e5d20ffa/files/reference"
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
>     "documentTitle": "Invoice_no._407822_1767154264830.pdf",
>     "documentTypes": [
>         "Invoice"
>     ],
>     "_link": {
>         "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/69842c9b71afad05e5d20ffa"
>     }
> }`</pre>
> [!note]- Technical Log
> <p>Create document:</p>
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: bash; gutter: false; theme: Confluence" data-theme="Confluence">`10:52:41,929 INFO  [ch.klara.luz.docs.service.DocumentCreatingService] (default task-1) [createDocument] Document created: 69843b1671afad05e5d22d2d for tenant 6b884e9b-2612-47fa-954e-666e7d3bab13
> 10:52:48,565 INFO  [ch.klara.luz.docs.service.DocumentCreatingService] (default task-1) [createDocument] Successfully create document for tenantId: 00a04daf-f2b3-41d5-8c12-2d1b4c48a36a; documentId: 69843b1671afad05e5d22d2d; fileSize: 48525`</pre>
> <p>The enricher is triggered:</p>
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`10:52:48,563 INFO  [ch.klara.luz.docs.service.DocumentService] (default task-1) [fireAsyncEnrichmentEvent] Triggering enrichment for document 698422d671afad05e5d1f7dc of tenant 00a04daf-f2b3-41d5-8c12-2d1b4c48a36a with priority DEFAULT`</pre>
> <p>The final status is added to enrichment status:</p>
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`10:53:05,308 INFO  [javax.ws.rs.client.ClientResponseFilter] (default task-1) [PUT] - http://host.docker.internal:8080/luz_jsonstore/api/mdb/00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/enrichmentstatus/add headers=[Connection=close,Content-Length=150,Content-Type=application/json,Date=Mon, 05 Jan 2026 09:53:05 GMT,Server=nginx/1.29.4,x-envoy-upstream-service-time=72] status-code=200 time-consuming=937`</pre>
</td>
<td></td>
<td><p> </p></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Sprint 149 - Test Report (0.03.14.xx)/attachments/luz-docs-execution-trigger-enricher-regarding-document-type/check.png]]</p></td>
<td></td>
</tr>
<tr>
<td><p>3</p></td>
<td><p>Run this cURL to check the final status of document :</p>
<ul>
<li><p>docid: "`69843b1671afad05e5d22d2d`"</p></li>
<li><p>tenantid: 00a04daf-f2b3-41d5-8c12-2d1b4c48a36a</p></li>
</ul>
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`curl --location &#39;http://localhost:8080/luz_docs/api/{tenantid}/documents/{docid}?exclude-total-count=false&folder-id=string&include-deleted-documents=true&include-file=false&include-folder-name=true&skip-security-classes=true&sort=string&#39; \
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
<li><p>`_files.reference: content type enricher success`</p></li>
<li><p>`_files.referenceTsq,_files.referenceTsr: Timestamp enricher success`</p></li>
<li><p>`_files.thumbnail128,_files.thumbnail256 ,_files.thumbnail512: Thumbnail success`</p></li>
<li><p>`_files.layoutAndText, documentDescription, documentTextContent: AI enricher success`</p></li>
<li><p>“invoiceData“ found with klaraAccgBct and klaraAccgTag only </p></li>
</ul>
<p>Sample body:</p>
> [!note]- Details invoice data sample
> <pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`{
>     "_id": "69843b1671afad05e5d22d2d",
>     "_createdBy": "admin",
>     "_createdDate": "2026-02-05T06:39:18.767Z",
>     "_updatedBy": "admin",
>     "_updatedDate": "2026-02-05T06:39:32.462Z",
>     "_deletionStatus": "false",
>     "_files": {
>         "reference": {
>             "_updatedDate": "2026-02-05T06:39:18.767Z",
>             "_sizeInBytes": 48525,
>             "_createdDate": "2026-02-05T06:39:18.767Z",
>             "_scanningTime": "2026-02-05T06:39:18.763773Z",
>             "_antivirusVersion": "ClamAV 1.4.3/27902/Wed Feb  4 08:24:02 2026",
>             "_scanningResult": "OK",
>             "contentType": "application/pdf",
>             "pdfVersion": "1.4",
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/69843b1671afad05e5d22d2d/files/reference"
>             }
>         },
>         "referenceTsq": {
>             "_createdDate": "2026-02-05T06:39:19.197Z",
>             "_sizeInBytes": 89,
>             "_updatedDate": "2026-02-05T06:39:19.197Z",
>             "contentType": "application/timestamp-query",
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/69843b1671afad05e5d22d2d/files/referenceTsq"
>             }
>         },
>         "referenceTsr": {
>             "_createdDate": "2026-02-05T06:39:19.197Z",
>             "_sizeInBytes": 3476,
>             "_updatedDate": "2026-02-05T06:39:19.197Z",
>             "contentType": "application/timestamp-reply",
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/69843b1671afad05e5d22d2d/files/referenceTsr"
>             }
>         },
>         "thumbnail128": {
>             "_createdDate": "2026-02-05T06:39:19.619Z",
>             "_sizeInBytes": 6559,
>             "_updatedDate": "2026-02-05T06:39:19.619Z",
>             "contentType": "image/jpeg",
>             "height": 128,
>             "width": 90,
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/69843b1671afad05e5d22d2d/files/thumbnail128"
>             }
>         },
>         "thumbnail256": {
>             "_createdDate": "2026-02-05T06:39:19.619Z",
>             "_sizeInBytes": 19903,
>             "_updatedDate": "2026-02-05T06:39:19.619Z",
>             "contentType": "image/jpeg",
>             "height": 256,
>             "width": 181,
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/69843b1671afad05e5d22d2d/files/thumbnail256"
>             }
>         },
>         "thumbnail512": {
>             "_createdDate": "2026-02-05T06:39:19.619Z",
>             "_sizeInBytes": 67313,
>             "_updatedDate": "2026-02-05T06:39:19.619Z",
>             "contentType": "image/jpeg",
>             "height": 512,
>             "width": 361,
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/69843b1671afad05e5d22d2d/files/thumbnail512"
>             }
>         },
>         "layoutAndText": {
>             "_createdDate": "2026-02-05T06:39:32.238Z",
>             "_sizeInBytes": 977834,
>             "_updatedDate": "2026-02-05T06:39:32.238Z",
>             "contentType": "application/x.abbyy.fre10+xml",
>             "_link": {
>                 "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/69843b1671afad05e5d22d2d/files/layoutAndText"
>             }
>         }
>     },
>     "_isBasicDocument": false,
>     "_isBeingCreated": false,
>     "_isEnriched": true,
>     "_isLargeFile": false,
>     "_isTemporaryDocument": false,
>     "_versionNumber": 3,
>     "folderIds": [],
>     "name": "review_test_plan.pdf",
>     "documentTitle": "Invoice_no._407822_1767154264830.pdf",
>     "documentTypes": [
>         "Invoice"
>     ],
>     "_referenceFileCreatedDate": "2025-10-24T02:07:58.000Z",
>     "_referenceFileUpdatedDate": "2026-02-05T06:39:32.234Z",
>     "contentLanguage": "en",
>     "documentTextContent": "qc leader review plan phase softbank qc leader review plan phase softbank assessment assessment plan demonstrates solid foundation structure comprehensive coverage key testing critical gaps requiring enhancement execution strengths strengths structure organized logical sections industry standards explicit scope management separation scope scope items risk identification proactive risk assessment mitigation strategies multi level testing combination component integration testing authentication focus dedicated security testing token validation structure explicit scope management risk identification multi level testing authentication focus critical issues critical issues missing details missing details issue plan references include 145 mentions detailed spreadsheet testrail xray suite deliverable reference document appendix impact validate coverage actual recommendation add summary minimum count planned endpoint include sample format template appendix reference specific document ids issue impact recommendation incomplete team lines 19 21 128 131 incomplete team lines 19 21 128 131 issue team names placeholders impact medium unclear accountability resource allocation recommendation populate actual names tbd expected assignment issue impact recommendation vague exit criteria 101 vague exit criteria 101 issue 95 % pass rate mentioned defined questions issue questions include runs failed tests remaining blocked tests counted failures recommendation clarify formula pass rate passed tests total tests blocked tests 100 recommendation traceability traceability issue mention requirements traceability matrix rtm impact medium verify requirements tested recommendation add requirement map api specification requirements issue impact recommendation gaps gaps missing data management details missing data management details current lists data types lacks data stored version control data data refresh strategy cycles creates maintains data recommendation add subsection data management covering storage versioning ownership current recommendation insufficient integration coverage insufficient integration coverage issue flows mentioned lines 73 75 missing scenarios cancel completed job delete job running concurrent operations job job list pagination filtering scenarios recommendation expand integration testing complete scenario matrix issue missing scenarios recommendation defect management process defect management process issue mentions logging defects define defect severity priority definitions defect workflow progress fixed verified closed procedures regression testing triggers recommendation add 11 defect management process issue recommendation missing metrics missing metrics issue defined metrics tracking progress recommendation add metrics covering daily execution rate defect detection rate defect turnaround time coverage percentage api availability uptime testing issue recommendation incomplete environment details incomplete environment details missing mention database data persistence layer specification postman collection version location api specification version reference 111 link actual specs mention logging monitoring tools debugging recommendation expand environment complete technical stack missing recommendation regression strategy regression strategy issue 121 mentions regression strategy defined questions issue questions tests regression regression selective regression triggers bug batched recommendation add subsection defining regression scope triggers recommendation moderate issues moderate issues 11 schedule concerns 11 schedule concerns observation day integration testing phase nov short scenarios risk critical defects late schedule compression compromise quality recommendation add day buffer clarify assumptions expected defect density observation risk recommendation 12 limited negative testing examples 12 limited negative testing examples issue mentions negative testing lacks specifics missing scenarios boundary testing shots max min values malformed json payloads sql injection attempts parameters extremely qasm files special characters job ids recommendation add subsection negative scenarios comprehensive list issue missing scenarios recommendation 13 smoke definition 13 smoke definition issue 98 mentions smoke entry criteria define includes recommendation add appendix smoke checklist verify endpoint returns -500 response valid token issue recommendation 14 missing status code validation details 14 missing status code validation details issue 32 mentions verifies status codes explain gap plan reference include status code mapping api spec recommendation add table mapping job status codes meanings submitted queued issue gap recommendation minor improvements minor improvements 15 language consistency 15 language consistency observation mixed vietnamese annotations lines 25 recommendation standardize english formal plan add glossary observation recommendation 16 format inconsistency 16 format inconsistency dates yyyy dd format issue start dates table start placeholders recommendation align schedule mark schedule dates issue recommendation 17 communication plan 17 communication plan missing status communicated daily weekly recommendation add status reporting cadence stakeholders missing recommendation 18 token expiry testing 18 token expiry testing 53 mentions expired token enhancement add time based testing scenarios token expires mid job execution enhancement recommendations summary recommendations summary timeline timeline priority priority action item action item owner owner execution week execution add count summary define exit criteria formula create requirements traceability matrix add defect management process lead lead lead lead week planning expand integration scenarios engineers phase week define regression strategy lead lead complete environment details setup phase devops lead engineers engineers add metrics dashboard document negative scenarios add smoke checklist week execution design execution approval recommendation approval recommendation conditional approval plan proceed design phase conditions status conditional approval status approve approve structure scope definition risk management requires updates requires updates address items execution recommend recommend address items ensure smooth execution track future track future items addressed execution phase steps steps lead update plan items 2025 27 peer review updated plan dev lead product owner finalize design traceability requirements conduct planning review meeting moving setup phase reviewed review reviewed review 2025 24 document version reviewed document version reviewed 1.0",
>     "invoiceData": {
>         "klaraAccgBct": [
>             {
>                 "30_REVENUE": "HIGH"
>             },
>             {
>                 "40_MATERIAL_EXP": "MEDIUM"
>             },
>             {
>                 "60_OTHER_COST": "MEDIUM"
>             },
>             {
>                 "85_NON_RECURRING_EXP": "MEDIUM"
>             }
>         ],
>         "klaraAccgTag": [
>             {
>                 "__AMBIGUOUS__": "HIGH"
>             },
>             {
>                 "personnel expenses": "MEDIUM"
>             },
>             {
>                 "advertising": "MEDIUM"
>             },
>             {
>                 "service": "MEDIUM"
>             }
>         ]
>     },
>     "title": "Markdown To PDF",
>     "_folders": [],
>     "_link": {
>         "self": "00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/documents/69843b1671afad05e5d22d2d"
>     }
> }`</pre>
</td>
<td></td>
<td><p> </p></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Sprint 149 - Test Report (0.03.14.xx)/attachments/luz-docs-execution-trigger-enricher-regarding-document-type/check.png]]</p></td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

%% ai-graph-start %%

**Related notes:**
- [[LUZ-Docs Trigger enricher regarding document type]]
- [[Adapt to support ONE API - Enricher first delivery]]
- [[Duplicate of Adapt to support ONE API - Enricher first delivery]]
- [[Invoice Run V2UAT - No error when luz-store is running with multiple pods]]
- [[Invoice Run V2UAT - Update latest Stimulsoft template - Execution]]

%% ai-graph-end %%