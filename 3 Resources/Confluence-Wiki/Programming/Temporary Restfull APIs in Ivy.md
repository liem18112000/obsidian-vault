---
title: "Temporary Restfull APIs in Ivy"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20463002420/Temporary+Restfull+APIs+in+Ivy
space: "LUZ"
topic: programming
relevance: 0.716
depth: 2.89
updated: 2018-03-13
attachments: 1
tags:
  - confluence
  - programming
  - space/luz
---

# Temporary Restfull APIs in Ivy

> [!info] Imported from Confluence
> Space **LUZ** · updated 2018-03-13 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20463002420/Temporary+Restfull+APIs+in+Ivy)
> Relevance 0.716 · topic `programming`

To support upload a (static) invoice from the device to Klara.

# **I. Endpoint **

**{protocol}://{ip}:{port}/ivy/api/{applicationName}/documents**

**Example: **<a href="http://192.168.80.29:8081/ivy/api/luz/documents" class="external-link" rel="nofollow">http://192.168.80.29:8081/ivy/api/luz/documents</a>****

**  **

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th>Enviroment</th>
<th>Application Name</th>
<th><br />
</th>
</tr>
&#10;<tr>
<td>Local</td>
<td>designer</td>
<td><br />
</td>
</tr>
<tr>
<td>KlaraLocal</td>
<td>luz</td>
<td><br />
</td>
</tr>
<tr>
<td>KlaraDev</td>
<td>luz</td>
<td><br />
</td>
</tr>
</tbody>
</table>

</div>

**  **

# **II. APIs**

- ## **Upload documents**

### **POST**

**Request**

**Header**

Using basic authorization supported by Ivy**  **

<span class="legacy-color-text-default">Example:</span> **Authorization Basic YWRtaW46YWRtaW4=  **

**Body**

<div>

|  |  |  |  |
|----|----|----|----|
| Param | Description | Constraint | Example |
| file | file to upload | Mandatory size \< 25MB, fileName \< 255 chars, support UTF-8, special character will be replaced by underscore, Duplicated file name is added (1) (2) .. as windows file system works | image.png |
| companyId | company Id | Mandatory | 1 |
| tenantId | tenant | Mandatory | 8c63a356-100d-445c-9f72-38a73c66b664 |
| category | Category to upload It should be LIABILITIES for this version | Mandatory | LIABILITIES |

</div>

Return code:

**<span class="legacy-color-text-default">200 </span>**<span class="legacy-color-text-default">OK </span>

<span class="legacy-color-text-default">**401** Unauthorized</span>

**<span class="legacy-color-text-default">415 </span>**<span class="legacy-color-text-default">Unsupported media type, or file's type is not supported</span>

**<span class="legacy-color-text-default">413 </span>**<span class="legacy-color-text-default">Request entity too large</span>

**<span class="legacy-color-text-default">500 </span>**<span class="legacy-color-text-default">Error</span>

**Response**

Update later!

### **Example via Postman**

**

![[20463002420-image2018-3-12_8-57-36.png]]

**

**  **
