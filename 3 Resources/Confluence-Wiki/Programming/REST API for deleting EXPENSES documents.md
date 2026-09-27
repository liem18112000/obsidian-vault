---
title: "REST API for deleting EXPENSES documents"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20474847507/REST+API+for+deleting+EXPENSES+documents
space: "LUZ"
topic: programming
relevance: 0.886
depth: 3
updated: 2018-11-30
attachments: 3
tags:
  - confluence
  - programming
  - space/luz
---

# REST API for deleting EXPENSES documents

> [!info] Imported from Confluence
> Space **LUZ** · updated 2018-11-30 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20474847507/REST+API+for+deleting+EXPENSES+documents)
> Relevance 0.886 · topic `programming`

# **Note:**

- This rest API is only available from Ivy version 7.1 on.  
    

# **End point:**

- Link: **http://{ivy.engine.host}:{ivy.engine.port}/ivy/api/{application-name}/{company-tenant-id}/companies/{company-id}/employees/{employee-id}/documents/{document-id}**

<!-- -->

- Pre-defined:

<div>

<table style="width: 44.3478%;">
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th>No</th>
<th>Element</th>
<th>description</th>
<th>Example value</th>
</tr>
&#10;<tr>
<td>1</td>
<td>{ivy.engine.host}</td>
<td>a host that ivy projects is deployed</td>
<td><ul>
<li>localhost</li>
<li>192.168.80.29</li>
<li>etc,</li>
</ul></td>
</tr>
<tr>
<td>2</td>
<td><span>{ivy.engine.port}</span></td>
<td><span>a port that point to ivy projects</span></td>
<td>8081</td>
</tr>
<tr>
<td>3</td>
<td><span>{application-name}</span></td>
<td><ul>
<li>for localhost: <strong><em>designer</em></strong></li>
<li>for other host: <em><strong>luz</strong></em></li>
</ul></td>
<td><ul>
<li>for localhost: <strong><em>designer</em></strong></li>
<li>for other host: <em><strong>luz</strong></em></li>
</ul></td>
</tr>
<tr>
<td>4</td>
<td>{company-tenant-id}</td>
<td>the company tenant id</td>
<td><span>2148bd05-7802-45ac-9f03-14691ebb2874</span></td>
</tr>
<tr>
<td>5</td>
<td><span>{company-id}</span></td>
<td>the company id</td>
<td>1</td>
</tr>
<tr>
<td>6</td>
<td><span>{employee-id}</span></td>
<td>the employee id who uploaded a document that we want to delete</td>
<td>1</td>
</tr>
<tr>
<td>7</td>
<td><span>{document-id}</span></td>
<td>the document id that we want to delete</td>
<td>123</td>
</tr>
</tbody>
</table>

</div>

# **Method:**

- DELETE

# **Headers:**

<div>

|  |  |  |  |
|----|----|----|----|
| No | Name | Description | Example value |
| 1 | Authorization | Bearer authorization | Bearer abcxyz |
| 2 | X-Requested-By | mandatory header in Ivy Rest API. You can enter any value | next |
| 3 | Content-Type | using: application/json | application/json |

</div>

  

# **Body:**

<div>

|     |          |                      |               |
|-----|----------|----------------------|---------------|
| No  | Key      | Description          | Example value |
| 1   | category | using only: EXPENSES | EXPENSES      |

</div>

#  **Response:**

- Only return status code

<div>

<table style="width: 57.7225%;">
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th>No</th>
<th>Code</th>
<th>Description value</th>
</tr>
&#10;<tr>
<td>1</td>
<td>400</td>
<td><ul>
<li>Category is not "<span>EXPENSES</span>"</li>
</ul></td>
</tr>
<tr>
<td>2</td>
<td>404</td>
<td><ul>
<li>No document exists for the ID</li>
<li>Found a document with the ID. But document do not belong tenant, company, employee, category. Ex: we want to delete existed document with tenant-id = abc, company-id=1, employee-id=1, category=<span>EXPENSES. But one of them does not match with the document's info(Ex: employee-id=0) then we will get 404 code</span></li>
</ul></td>
</tr>
<tr>
<td>3</td>
<td>500</td>
<td><ul>
<li>Internal error</li>
</ul></td>
</tr>
</tbody>
</table>

</div>

#  **Example:**


![[20474847507-image2018-11-30_15-13-59.png]]



Image 1

  


![[20474847507-image2018-11-30_15-14-18.png]]



Image 2

  


![[20474847507-image2018-11-30_15-14-48.png]]



Image 3
