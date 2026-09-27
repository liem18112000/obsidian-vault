---
ai_hash: c02f6717f5779fa5
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 1
depth: 3
entities: []
relevance: 0.792
source: https://axonivy.atlassian.net/wiki/spaces/WOW/pages/47097154141/Document+Creator+API
space: WOW
status: reference
tags:
- confluence
- programming
- space/wow
title: Document Creator API
topic: programming
type: source
updated: 2022-04-25
---

# Document Creator API

> [!info] Imported from Confluence
> Space **WOW** · updated 2022-04-25 · [open original](https://axonivy.atlassian.net/wiki/spaces/WOW/pages/47097154141/Document+Creator+API)
> Relevance 0.792 · topic `programming`

**Luz_docs_creator** is module which provide API for generating document in general way. It only needs 2 required parameters: j**son of data model** and **template**.

Postman api sample:

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="b9a1c2fb-1e38-429a-ac0f-9780afa82d6c" macro-name="view-file"><a href="../_attachments/47097154141-Luz_docs_creator_api.postman_collection.json" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47097154141/Luz_docs_creator_api.postman_collection.json?version=2&amp;modificationDate=1650856923393&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/json" data-has-thumbnail="true">

![[47097154141-Luz_docs_creator_api.postman_collection.json]]

</a></span>

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Name</strong></p></th>
<th><p><strong>Key/Value</strong></p></th>
<th><p><strong>Example</strong></p></th>
</tr>
&#10;<tr>
<td><p>Endpoint</p></td>
<td><p><em>http://{ivy.engine.host}:{ivy.engine.port}/{application-name}/api/create-documents</em></p></td>
<td><p>http://localhost:8080/luz/api/create-documents</p></td>
</tr>
<tr>
<td><p>Method</p></td>
<td><p>POST</p></td>
<td></td>
</tr>
<tr>
<td><p>Consumes</p></td>
<td><p>MediaType.MULTIPART_FORM_DATA</p></td>
<td></td>
</tr>
<tr>
<td><p>Permission</p></td>
<td><p>PermitAll</p></td>
<td></td>
</tr>
<tr>
<td><p>Form-data</p></td>
<td><p>json_object</p></td>
<td><p>{<br />
"order": {<br />
"orderTitle": "Rechnung Nr. 534696",<br />
"company": {<br />
"UID": "CHE-103.727.240",<br />
"fullInformation": "KLARA Business AG"<br />
},<br />
"address": "Wilhelmshöhe 1",<br />
"additionalAddress": "",<br />
"postCodeAndCity": "6003 Luzern",<br />
"customerName": "1. Recurring Effective",<br />
"orderNo": "538453"<br />
}<br />
}</p></td>
</tr>
<tr>
<td><p>Form-data</p></td>
<td><p>file_template</p></td>
<td><p>invoiceTemplateExcludeVat_withFooter.docx</p></td>
</tr>
<tr>
<td><p>Query-Params</p></td>
<td><p>locale</p></td>
<td><p>de-CH</p></td>
</tr>
<tr>
<td><p>Query-Params</p></td>
<td><p>output-name</p></td>
<td><p>invoice</p></td>
</tr>
<tr>
<td><p>Query-Params</p></td>
<td><p>document-format</p></td>
<td><p>pdf</p></td>
</tr>
<tr>
<td><p>Header-Params</p></td>
<td><p>X-Requested-By</p></td>
<td><p>any value</p></td>
</tr>
</tbody>
</table>

</div>

### Implementation Note:

<div>

|  |  |
|----|----|
| **Description** | **Library/Plugin** |
| Using Consumes MediaType.MULTIPART_FORM_DATA for transmit binary data | org.glassfish.jersey.media.jersey-media-multipart.jar in library of Ivy_9.1.1 |
| Using FormDataParam for binding the named body part of Template-InputStream and Json-Object | org.glassfish.jersey.media.multipart.FormDataParam |
| Using MultiPart for adding multiple params in IntegrationTest’s WebClient | org.glassfish.jersey.media.multipart.MultiPart |
| Add Template-InputStream to MultiPart’s BodyPart in IntegrationTest’s WebClient | org.glassfish.jersey.media.multipart.file.StreamDataBodyPart |
| Activate integration test phase | com.axonivy.ivy.ci.project-build-plugin |
| Test coverage report | org.jacoco.jacoco-maven-plugin |

</div>

%% ai-graph-start %%

**Related notes:**
- [[Temporary Restfull APIs in Ivy]]
- [[Upload Document API]]
- [[Invoice API Java Client]]
- [[Generating document from viewgen]]
- [[Invoice API Reference]]

%% ai-graph-end %%