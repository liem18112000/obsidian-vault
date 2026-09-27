---
ai_hash: d00701aab5d54bda
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 3
entities: []
relevance: 0.773
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38168505596/Uploading+documents
space: Helios
status: reference
tags:
- confluence
- programming
- space/helios
title: Uploading documents
topic: programming
type: source
updated: 2018-07-24
---

# Uploading documents

> [!info] Imported from Confluence
> Space **Helios** · updated 2018-07-24 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38168505596/Uploading+documents)
> Relevance 0.773 · topic `programming`

To support upload a (static) invoice from a device.

### **I. Endpoint **

{protocol}://{ip}:{port}/{applicationName}/api/documents/{tenant-id}/companies/{company-id}

Example: <span class="nolink legacy-color-text-blue4">http://192.168.72.90:8080/luz_mobile/api/documents/e2adffbe-8679-423f-aa02-e7646c1d3d99/companies/1</span>

### **II. Method**

POST

### **III. Request**

Header

<div>

| Attribute name | Attribute value | Description |
|----|----|----|
| Authorization | Bearer eyJhb0lD4IATGWUlfH...EcWF0HOyvXsAKUXjQNo0-BpueGkysOl9ncmd5cfsDfJa6FJOzOzyK3DINIpGXA2ug7jW-MGxQ | <span class="legacy-color-text-default">Bearer token here is the retained refresh token</span> |
| App-Version | 0.0.1 | the current version of myKLARA app |

</div>

Body

<div>

<table style="width: 54.4932%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<thead>
<tr>
<th><p><span>Attribute name</span></p></th>
<th><span>Attribute value</span></th>
<th><p>Description</p></th>
<th><p>Constraint</p></th>
<th>Supported file type</th>
<th><p>Example</p></th>
</tr>
</thead>
<tbody>
<tr>
<td>file</td>
<td><br />
</td>
<td>file to upload</td>
<td>Mandatory size &lt; 25MB</td>
<td><p>JPEG</p>
<p>JPE</p>
<p>PNG</p>
<p>PDF</p></td>
<td>image.png</td>
</tr>
<tr>
<td>category</td>
<td><span>LIABILITIES</span></td>
<td><br />
</td>
<td>default value for invoices</td>
<td><br />
</td>
<td><br />
</td>
</tr>
</tbody>
</table>

</div>

### **IV.Response**

Success

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c5165ef9-3dbd-43ac-a305-480835cbf1c8" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
    "status": "SUCCESS",
    "message": null,
    "description": null,
    "id": "51df367e-57e9-43db-8e0c-2f9809170e4d",
    "refreshToken": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJPVkMyV29PVjdfMGJkR1lqSFJYb1FZVE0yekhNOHlJQVh2YWY2dEhnQXpZIn0.eyJqdGkiOiIyMDdhZDYzZC05MmEzLTRjNmQtYjBiOS1hYmZhYWEzNWRmYjMiLCJleHAiOjAsIm5iZiI6MCwiaWF0IjoxNTMxMTMxODUxLCJpc3MiOiJodHRwczovL2xvZ2luLmtsYXJhLmNoL2F1dGgvcmVhbG1zL2tsYXJhIiwiYXVkIjoia2xhcmEtbW9iaWxlIiwic3ViIjoiYzEwNDU0MDktMzEzNS00MGRiLWJhZDYtM2M5ZGJiODgzNjJkIiwidHlwIjoiT2ZmbGluZSIsImF6cCI6ImtsYXJhLW1vYmlsZSIsImF1dGhfdGltZSI6MCwic2Vzc2lvbl9zdGF0ZSI6IjBmZDYyYjk0LTI0ZTItNDI4OC05NjUyLTg4MzIxYWYzNGRjMiIsImNsaWVudF9zZXNzaW9uIjoiNmFmZWExYTMtMmRkZS00OTY5LWIwOTMtNjc3YTIyZDI1ZWNkIiwicmVhbG1fYWNjZXNzIjp7InJvbGVzIjpbIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50Iiwidmlldy1wcm9maWxlIl19fX0.eOWSobmfbKzQuDgWtigiv9YwTQ0RyH8GcYvFXq37M2nBd7xhhJgTS7hQy-xXNyCh17NH9So4uW6cJZNbzOWM3Uy-ze0-8vdpNT7aMUaFXH9gps3gjJa5eE1xko-cKXUcChoLRJ9xBTwLr4UIh_m_cnz5JESFGr162jmzGlBIzRqAt7e0Mlf-7kgzHsi2hO4bNMNzbfhaC6VleCsHrnhx17v_JextwKm9Fq7jvUzh8dqkD6b7EN6xaYxIX_dJEthbvLNfO1vtYk0deA8h2TGhSsPtppf-yKqFvKHACMUViCzLHYFkWgWnsfMIvqH_MtWq3N3RjRavNw4Llac0w8KVzA"
}
```

</div>

</div>

Error

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th>Code</th>
<th>What went wrong</th>
</tr>
&#10;<tr>
<td>400<span class="legacy-color-text-default"> Bad request</span></td>
<td><span class="legacy-color-text-default">file or companyId or tenantId not found</span></td>
</tr>
<tr>
<td>403 Forbidden</td>
<td><span class="legacy-color-text-default">User has no tenant</span></td>
</tr>
<tr>
<td>404 Not found</td>
<td><span class="legacy-color-text-default">Tenant is not exist or user has no access to tenant</span></td>
</tr>
<tr>
<td>410 Gone</td>
<td><span class="legacy-color-text-default">Refresh token is not valid or expired</span></td>
</tr>
<tr>
<td>413 Request Entity Too Large</td>
<td><br />
</td>
</tr>
<tr>
<td><span class="legacy-color-text-default">415 </span><span class="legacy-color-text-default">Unsupported media type</span></td>
<td><br />
</td>
</tr>
<tr>
<td><span class="legacy-color-text-default">500 </span><span class="legacy-color-text-default">Internal server error</span></td>
<td><br />
</td>
</tr>
</tbody>
</table>

</div>

Example

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1265273a-1f45-4f26-b3b3-b45e1506395b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
    "status": "fail",
    "message": "File not found"
}
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Temporary Restfull APIs in Ivy]]
- [[Login]]
- [[Upload Document API]]
- [[Getting tenant list]]
- [[App Validity]]

%% ai-graph-end %%