---
title: "Upload Document API"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20468874747/Upload+Document+API
space: "LUZ"
topic: programming
relevance: 0.792
depth: 3
updated: 2021-11-26
attachments: 9
tags:
  - confluence
  - programming
  - space/luz
---

# Upload Document API

> [!info] Imported from Confluence
> Space **LUZ** · updated 2021-11-26 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20468874747/Upload+Document+API)
> Relevance 0.792 · topic `programming`

## 

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="19564a45-c127-4dfb-aec7-0ff50b9cb3a9" macro-name="toc">

</div>

## <span class="legacy-color-text-red2">Note</span>

<span class="legacy-color-text-red2">This rest API is only available on Ivy version 7.1 and later versions</span>

<span class="legacy-color-text-red2"><span class="legacy-color-text-default">IVY 9 changed context path from</span> ***/ivy/api/{application-name}/ ***<span class="legacy-color-text-default">to</span> **/*{application-name}*/api/**</span>

  

## End point

### Upload company document:

Method: **POST**

End point: *http://{ivy.engine.host}:{ivy.engine.port}/ivy/api/{application-name}/{company-tenant-id}/companies/{company-id}/documents*

Consumes: MediaType.MULTIPART_FORM_DATA

Produces: MediaType.APPLICATION_JSON

TokenRolesAllowed: RoleConstant.COMPANY_ADMINISTRATOR, RoleConstant.TRUSTED_USER

<div>

|     |                     |               |                                      |
|-----|---------------------|---------------|--------------------------------------|
| no  | path                | description   | example value                        |
| 1   | {company-tenant-id} | the tenant id | ebd5fc8a-ac00-4bff-a139-faaa0e4c5161 |
| 2   | {company-id}        | company id    | 1                                    |

</div>

### Upload employee document:

Method: **POST**

End point: *http://{ivy.engine.host}:{ivy.engine.port}/ivy/api/{application-name}/{company-tenant-id}/companies/{company-id}/employees/{employee-id}/documents*

Consumes: MediaType.MULTIPART_FORM_DATA

Produces: MediaType.APPLICATION_JSON

TokenRolesAllowed: RoleConstant.COMPANY_ADMINISTRATOR, RoleConstant.TRUSTED_USER, RoleConstant.EMPLOYEE

<div>

|     |                     |               |                                      |
|-----|---------------------|---------------|--------------------------------------|
| no  | path                | description   | example value                        |
| 1   | {company-tenant-id} | the tenant id | ebd5fc8a-ac00-4bff-a139-faaa0e4c5161 |
| 2   | {company-id}        | company id    | 1                                    |
| 3   | {employee-id}       | employee id   | 1                                    |

</div>

### Note:

Note:** ** If you are in your local computer,** **the {application-name} is** designer. **Otherwise, it is** luz**

## Headers

<div>

|  |  |  |  |
|----|----|----|----|
| no | name | description | example value |
| 1 | Authorization | Bearer authorization | Bearer {token} |
| 2 | X-Requested-By | mandatory header in Ivy Rest API. You can enter any value | next |

</div>

## Body (form data)

<div>

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th>no</th>
<th>key</th>
<th><span>description </span></th>
<th>example value</th>
</tr>
&#10;<tr>
<td>1</td>
<td>file</td>
<td>a file to be uploaded</td>
<td><br />
</td>
</tr>
<tr>
<td>2</td>
<td>category</td>
<td>the category of the document</td>
<td>Must be <strong>LIABILITY_UPLOAD</strong></td>
</tr>
</tbody>
</table>

</div>

## Steps to consume Rest API

First, you have to have a Bearer token by consuming this endpoint: 

<span class="inline-comment-marker" ref="3477fe4f-49f1-46de-8f10-bd7fc1f99390">http://{wildfly.host}:{wildfly.port}/luzsec/api/</span><span class="resolvedVariable"><span class="inline-comment-marker" ref="3477fe4f-49f1-46de-8f10-bd7fc1f99390">{tenant_id}</span></span><span class="inline-comment-marker" ref="3477fe4f-49f1-46de-8f10-bd7fc1f99390">/tokens</span>

method: **POST**

<div>

|     |               |                     |                                  |
|-----|---------------|---------------------|----------------------------------|
| no  | name          | description         | example value                    |
| 1   | Authorization | Basic authorization | Basic {endcodedUserNamePassword} |

</div>


![[20468874747-image2018-7-6_10-2-55.png]]



Then use the obtained token to access the document upload API:


![[20468874747-image2018-7-6_10-0-41.png]]



In case the company id or tenant id is wrong, you will get the error code 401:


![[20468874747-image2018-7-6_10-0-9.png]]



In case the given token is incorrect, the request will be rejected with the error code 401 as well:


![[20468874747-error.png]]
