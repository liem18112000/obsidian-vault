---
title: "App Validity"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38174382060/App+Validity
space: "Helios"
topic: programming
relevance: 0.716
depth: 2.89
updated: 2018-07-24
attachments: 0
tags:
  - confluence
  - programming
  - space/helios
---

# App Validity

> [!info] Imported from Confluence
> Space **Helios** · updated 2018-07-24 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38174382060/App+Validity)
> Relevance 0.716 · topic `programming`

validate the validity of myKLARA app.

### **I. Endpoint **

{protocol}://{ip}:{port}/{applicationName}/api/app-validity

Example: <span class="legacy-color-text-blue4">http://192.168.72.90:8080/luz_mobile/api/app-validity</span>

### **II. Method**

POST

### **III. Request**

Header

<div>

| Attribute name | Attribute value | Description |
|----|----|----|
| Authorization | Bearer eyJhb0lD4IATGWUlfH...EcWF0HOyvXsAKUXjQNo0-BpueGkysOl9ncmd5cfsDfJa6FJOzOzyK3DINIpGXA2ug7jW-MGxQ | <span class="legacy-color-text-default">authorization token here is the retained refresh_token in the authentication step</span> |
| App-Version | 0.0.1 | the current version of myKLARA app |

</div>

###  **IV.Response**

Successful case 

<div>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td><p>{</p>
<p>"status": "success",<br />
"message": null,<br />
"refreshToken":"eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJPVkMyV29PVjdfMGJkR1lqSFJYb1FZVE0yekhNOHlJQVh2YWY2dEhnQXpZIn0.eyJqdGkiOiI0OTlkNjEwZC01NGI5LTQzMmYtYTVkZC05NWI5YWNmMTE4ZGEiLCJleHAiOjAsIm5iZiI6MCwiaWF0IjoxNTMyNDAxOTU0LCJpc3MiOiJodHRwczovL2xvZ2luLmtsYXJhLmNoL2F1dGgvcmVhbG1zL2tsYXJhIiwiYXVkIjoia2xhcmEtbW9iaWxlIiwic3ViIjoiYzEwNDU0MDktMzEzNS00MGRiLWJhZDYtM2M5ZGJiODgzNjJkIiwidHlwIjoiT2ZmbGluZSIsImF6cCI6ImtsYXJhLW1vYmlsZSIsImF1dGhfdGltZSI6MCwic2Vzc2lvbl9zdGF0ZSI6ImIzM2Y3OWM2LWRlMTMtNDVhOC04Y2U5LTE5ZWI4N2Y3Y2MxMyIsImNsaWVudF9zZXNzaW9uIjoiMmJhMGNhOTQtZWQxNy00ZDNiLWE1NjAtYmRlZWJkMDZiMWQxIiwicmVhbG1fYWNjZXNzIjp7InJvbGVzIjpbIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50Iiwidmlldy1wcm9maWxlIl19fX0.iZCqn7ylA3eX5FltI2v1GOAZ3Dy5TDzCuWx_pUY1b3SKnK4bBQ5NXMZxDk8ZV2yYz0SUeJnB5QLEB-Brz3PE4KiZ-9br3H2u-CyHGjQdKnalPS35ggkKrAwD_GewP2MtOREoGE8Lpm7ddXzwDpxOuaJiUouQNp7ZBhppWdn_blYlRXl3ohji6KCH00C82hNiVrN0QJIMW7RheI31rnaLAjLb7O2WfxuMy4VmudRV_Q2UJufwiqpSYMHYufVVG45elHWp-IQY3ywKl3hmhFH6_bvm8Lk4Zwm_2MGx1C1FsxjfO7kdFteiGjNLO_EUGgmrrSPeWaI7-_2WgS764QKEmg",<br />
"<strong>appValidity</strong>": true</p>
<p>}</p></td>
</tr>
</tbody>
</table>

</div>

#### Error cases

<div>

| Code | What went wrong |
|----|----|
| UNAUTHORIZED (401) | Bearer token not found |
| BAD REQUEST (400) | App version header not found |
| GONE (410) | The generated refresh_token is not valid or expired. |
| INTERNAL SERVER ERROR (500) | Minimum supported version not found on the wildfly server or an unexpected exception is happening |

</div>

  
Example

<div>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td><p><code class="text plain">{</code><br />
<code class="text spaces">    </code><code class="text plain">"status": "fail",</code><br />
<code class="text spaces">    </code><code class="text plain">"message": "The refreshtoken could not be validated at jwt side : Gone"</code><br />
<code class="text plain">}</code></p></td>
</tr>
</tbody>
</table>

</div>
