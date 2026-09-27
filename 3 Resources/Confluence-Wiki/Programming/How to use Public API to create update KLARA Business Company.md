---
title: "How to use Public API to create/update KLARA Business Company"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47553806568/How+to+use+Public+API+to+create+update+KLARA+Business+Company
space: "TS"
topic: programming
relevance: 0.731
depth: 2.73
updated: 2023-12-04
attachments: 12
tags:
  - confluence
  - programming
  - space/ts
---

# How to use Public API to create/update KLARA Business Company

> [!info] Imported from Confluence
> Space **TS** · updated 2023-12-04 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47553806568/How+to+use+Public+API+to+create+update+KLARA+Business+Company)
> Relevance 0.731 · topic `programming`

<div class="panel conf-macro output-block" hasbody="true" macro-id="76acd7f4-e234-40e1-ad4b-3038629873f4" macro-name="panel" style="background-color: #E3FCEF;border-width: 1px;">

<div class="panelContent" style="background-color: #E3FCEF;">

# 🔐  **Authentication** 

</div>

</div>

### 1. Tenant id


![[47553806568-information.png]]

 This API helps retrieve all companies associated with the account by authenticating with a username and password.

The URI:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="cd209385-6c53-4f99-bc76-ede5e7c41bf8" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
https://api.klara.ch/core/latest/tenants
```

</div>

</div>

Method: <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" hasbody="false" macro-id="69bde507-516b-48d5-a7cf-d9d48f338e93" macro-name="status">POST</span>

Header:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="59cd9290-b837-40c4-96f1-839db4a77f1d" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Content-type: application/x-www-form-urlencoded
```

</div>

</div>

Body:

<div>

|  |  |  |  |
|----|----|----|----|
| **Field** | **Is required** | **Description** | **Example** |
| username | 

![[47553806568-check.png]]

 | The username of the user who created the tenant. | <a href="mailto:example@axongroupio.ch" class="external-link" rel="nofollow">example@axongroupio.ch</a> |
| password | 

![[47553806568-check.png]]

 | The password of the user who created the tenant. | aabb@123456 |

</div>

Example with postman in **DEV environment**:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5b73e698-1853-40fa-adea-29fafb3eb0fe" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
curl --location 'https://api-dev-vn.klara.tech/core/latest/tenants' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'username=example@gmail.com' \
--data-urlencode 'password=aabb@123456'
```

</div>

</div>

Response example:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="97bb2296-be78-4a1b-aaeb-72232ccd94b5" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
[
    {
        "tenant_id": "611b19f4-bc51-4526-9f63-6abdef380df2",
        "company_id": 1,
        "company_name": "1. First company 11.13.2023"
    },
    {
        "tenant_id": "762890d0-693b-4289-a96e-e88e3197faae",
        "company_id": 1,
        "company_name": "New Company"
    }
]
```

</div>

</div>

Try out in DEV environment: <a href="https://api-dev.klara.tech/docs#/Authentication/post_core_latest_tenants" class="external-link" data-card-appearance="inline" rel="nofollow">https://api-dev.klara.tech/docs#/Authentication/post_core_latest_tenants</a>


![[47553806568-chrome_tir4sbm7eM.png]]



You can also get the tenant id of a tenant from the GUI:


![[47553806568-3YU03LXzDB.png]]



### 2. Token creation


![[47553806568-information.png]]

 This API retrieves the company's token based on its tenant_id.

The URI:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="cfe56ed5-25ff-41e0-8fab-5b81fc53fec7" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
https://api.klara.ch/core/latest/token
```

</div>

</div>

Method: <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" hasbody="false" macro-id="954f2c54-5887-435d-b8c7-c4e0eeca49f5" macro-name="status">POST</span>

Header:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="08280809-6361-404b-874c-bac6a9dcee8f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Content-type: application/x-www-form-urlencoded
```

</div>

</div>

Body:

<div>

|  |  |  |  |
|----|----|----|----|
| **Field** | **Is required** | **Description** | **Example** |
| username | 

![[47553806568-check.png]]

 | The username of the user who created the tenant. | example@gmail.com |
| password | 

![[47553806568-check.png]]

 | The password of the user who created the tenant. | aabb@123456 |
| grant_type | 

![[47553806568-check.png]]

 | Should be password grant-type | password |
| tenant_id | 

![[47553806568-check.png]]

 | The tenant id of the company | 611b19f4-bc51-4526-9f63-6abdef380df2 |
| company_id | 

![[47553806568-check.png]]

 | **MUST** be 1 | 1 |

</div>

Example with postman in **DEV environment**:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="668cd7b9-f2d4-4577-b063-4fb55d1973be" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
curl --location 'https://api-dev.klara.tech/core/latest/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'username=example@gmail.com' \
--data-urlencode 'password=aabb@123456' \
--data-urlencode 'grant_type=password' \
--data-urlencode 'tenant_id=611b19f4-bc51-4526-9f63-6abdef380df2' \
--data-urlencode 'company_id=1'
```

</div>

</div>

Response example:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="44653da1-4a09-4af6-ac3c-c29c73d3496b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
    "access_token": "ewogICJleHAiOiAxNzAwMDE2MDg3LAogICJpYXQiOiAxNzAwMDEyNDg3LAogICJqdGkiOiAiMWRhNDVkZWItZjE4Yy00NDQ5LTgxZDctZTE2M2M0NTA5YzhmIiwKICAiaXNzIjogImh0dHBzOi8vbG9naW4tZGV2LXZuLmtsYXJhLnRlY2gvYXV0aC9yZWFsbXMva2xhcmEiLAogICJzdWIiOiAiODJiNDMzYmUtZmI0Ny00ZGJiLTkxMTgtODM4NzAwMTE4YzdiIiwKICAidHlwIjogIkJlYXJlciIsCiAgImF6cCI6ICJrbGFyYS1wdWJsaWMtYXBpIiwKICAic2Vzc2lvbl9zdGF0ZSI6ICJhNTNhZDM3NS03OWU5LTRmMWMtOWZlYS05YWQ0NDczYTE1ZWYiLAogICJzY29wZSI6ICJwcm9maWxlIGVtYWlsIiwKICAic2lkIjogImE1M2FkMzc1LTc5ZTktNGYxYy05ZmVhLTlhZDQ0NzNhMTVlZiIsCiAgInRlbmFudF9pZCI6ICIyMjJiMTlmNC1iYzUxLTQ1MjYtOWY2My02YWJkZWYzODAyMjIiLAogICJlbWFpbF92ZXJpZmllZCI6IGZhbHNlLAogICJjb21wYW55X2lkIjogIjEiLAogICJuYW1lIjogInRlc3QgYWNjb3VudCIsCiAgInByZWZlcnJlZF91c2VybmFtZSI6ICJleGFtcGxlQGF4b25ncm91cGlvLmNoIiwKICAiZ2l2ZW5fbmFtZSI6ICJ0ZXN0IiwKICAiZmFtaWx5X25hbWUiOiAiYWNjb3VudCIsCiAgImVtYWlsIjogImV4YW1wbGVAYXhvbmdyb3VwaW8uY2giCn0=",
    "expires_in": 3600,
    "refresh_expires_in": 14400,
    "refreshToken": "ewogICJleHAiOiAxNzAwMDE2MDg3LAogICJpYXQiOiAxNzAwMDEyNDg3LAogICJqdGkiOiAiMWRhNDVkZWItZjE4Yy00NDQ5LTgxZDctZTE2M2M0NTA5YzhmIiwKICAiaXNzIjogImh0dHBzOi8vbG9naW4tZGV2LXZuLmtsYXJhLnRlY2gvYXV0aC9yZWFsbXMva2xhcmEiLAogICJzdWIiOiAiODJiNDMzYmUtZmI0Ny00ZGJiLTkxMTgtODM4NzAwMTE4YzdiIiwKICAidHlwIjogIkJlYXJlciIsCiAgImF6cCI6ICJrbGFyYS1wdWJsaWMtYXBpIiwKICAic2Vzc2lvbl9zdGF0ZSI6ICJhNTNhZDM3NS03OWU5LTRmMWMtOWZlYS05YWQ0NDczYTE1ZWYiLAogICJzY29wZSI6ICJwcm9maWxlIGVtYWlsIiwKICAic2lkIjogImE1M2FkMzc1LTc5ZTktNGYxYy05ZmVhLTlhZDQ0NzNhMTVlZiIsCiAgInRlbmFudF9pZCI6ICIyMjJiMTlmNC1iYzUxLTQ1MjYtOWY2My02YWJkZWYzODAyMjIiLAogICJlbWFpbF92ZXJpZmllZCI6IGZhbHNlLAogICJjb21wYW55X2lkIjogIjEiLAogICJuYW1lIjogInRlc3QgYWNjb3VudCIsCiAgInByZWZlcnJlZF91c2VybmFtZSI6ICJleGFtcGxlQGF4b25ncm91cGlvLmNoIiwKICAiZ2l2ZW5fbmFtZSI6ICJ0ZXN0IiwKICAiZmFtaWx5X25hbWUiOiAiYWNjb3VudCIsCiAgImVtYWlsIjogImV4YW1wbGVAYXhvbmdyb3VwaW8uY2giCn0=",
    "token_type": "Bearer"
}
```

</div>

</div>

Try out in DEV environment:<a href="https://api.klara.ch/docs#/Authentication/post_core_latest_token" class="external-link" data-card-appearance="inline" rel="nofollow">https://api.klara.ch/docs#/Authentication/post_core_latest_token</a>


![[47553806568-chrome_T5ArbBf4DF.png]]



------------------------------------------------------------------------

<div class="panel conf-macro output-block" hasbody="true" macro-id="a1b3a814-5720-4481-a19b-6914b0a1e6d7" macro-name="panel" style="background-color: #E3FCEF;border-width: 1px;">

<div class="panelContent" style="background-color: #E3FCEF;">

# 🏢 **Company creation** 

</div>

</div>


![[47553806568-information.png]]

 This API help you to create the company in Klara.

The URI:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f384dcf3-b866-44c1-9acf-6eef08252312" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
https://api.klara.ch/core/v2/tenants/companies
```

</div>

</div>

Method: <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" hasbody="false" macro-id="e6dd702d-8def-4844-b7c8-ada82acd1b88" macro-name="status">POST</span>

Header:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c5e748fc-46dd-44f0-abcc-c7ad33c2be6f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Accept: application/json
Authorization: bearer token -> this token is created with the username, password and tenantId of the Fairgate company in Klara (must be created in the GUI beforehand)
```

</div>

</div>

Body:

<div>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<tbody>
<tr>
<th colspan="2"><p><strong>Field</strong></p></th>
<th><p><strong>Is required</strong></p></th>
<th><p><strong>Description</strong></p></th>
<th><p><strong>Example</strong></p></th>
</tr>
&#10;<tr>
<td colspan="2"><p><code>name</code></p></td>
<td><p>

![[47553806568-check.png]]

</p></td>
<td><p>the name of the company</p></td>
<td><p><code>New Company</code></p></td>
</tr>
<tr>
<td colspan="2"><p><code>legalForm</code></p></td>
<td><p>

![[47553806568-check.png]]

</p></td>
<td><p>the legal form of the company</p></td>
<td><div id="expander-656635744" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="8f3e60d8-f055-4901-ad6f-b44113824422" data-macro-name="expand">
<div id="expander-control-656635744" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">data</span>
</div>
<div id="expander-content-656635744" class="expand-content expand-hidden">
<p>Individually owned company → <code>INDIVIDUALLY_OWNED_COMPANY</code> or <code>EU</code></p>
<p>General partnership → <code>GENERAL_PARTNERSHIP</code> or <code>KG</code></p>
<p>Limited partnership → <code>LIMITED_PARTNERSHIP</code> or <code>KDG</code></p>
<p>Public limited company → <code>CORPORATION</code> or <code>AG</code></p>
<p>Limited liability company → <code>LIMITED_LIABILITY</code> or <code>GmbH</code></p>
<p>Cooperative company → <code>COOPERATIVE_COMPANY</code> or <code>G</code></p>
<p>Association → <code>ASSOCIATION</code> or <code>V</code></p>
<p>Foundation → <code>FOUNDATION</code> or <code>S</code></p>
<p>Simple partnership → <code>SIMPLE_PARTNERSHIP</code> or <code>EG</code></p>
<p>Other → <code>OR</code></p>
</div>
</div></td>
</tr>
<tr>
<td colspan="2"><p><code>language</code></p></td>
<td><p>

![[47553806568-check.png]]

</p></td>
<td><p>German: <strong>de</strong></p>
<p>English: <strong>en</strong></p>
<p>French: <strong>fr</strong></p>
<p>Italian: <strong>it</strong></p></td>
<td><p><code>en</code></p></td>
</tr>
<tr>
<td colspan="2"><p><code>foundingDate</code></p></td>
<td><p>Optional</p></td>
<td><p>Format: <code>YYYY-MM-DD</code></p></td>
<td><p><code>2019-12-20</code></p></td>
</tr>
<tr>
<td rowspan="3"><p><code>phones</code></p></td>
<td><p><code>id</code></p></td>
<td><p>

![[47553806568-error.png]]

</p></td>
<td><p><em><strong>Do not put the ID when create the company</strong></em></p></td>
<td></td>
</tr>
<tr>
<td><p><code>phoneNumber</code></p></td>
<td><p>

![[47553806568-check.png]]

</p></td>
<td><p>The phone number of the company</p></td>
<td><p><code>+41783334444</code></p></td>
</tr>
<tr>
<td><p><code>type</code></p></td>
<td><p>

![[47553806568-check.png]]

</p></td>
<td><p>The phone type, must be <code>OFFICE</code></p></td>
<td><p><code>OFFICE</code></p></td>
</tr>
<tr>
<td rowspan="3"><p><code>emails</code></p></td>
<td><p><code>id</code></p></td>
<td><p>

![[47553806568-error.png]]

</p></td>
<td><p><em><strong>Do not put the ID when create the company</strong></em></p></td>
<td></td>
</tr>
<tr>
<td><p><code>emailAddress</code></p></td>
<td><p>

![[47553806568-check.png]]

</p></td>
<td><p>The email of the company</p></td>
<td><p><code>example@gmail.com</code></p></td>
</tr>
<tr>
<td><p><code>type</code></p></td>
<td><p>

![[47553806568-check.png]]

</p></td>
<td><p>The email type, must be <code>OFFICE</code></p></td>
<td><p><code>OFFICE</code></p></td>
</tr>
<tr>
<td rowspan="9"><p><code>addresses</code></p></td>
<td><p><code>id</code></p></td>
<td><p>

![[47553806568-error.png]]

</p></td>
<td><p><em><strong>Do not put the ID when create the company</strong></em></p></td>
<td></td>
</tr>
<tr>
<td><p><code>addressLines</code></p></td>
<td><p>

![[47553806568-check.png]]

</p></td>
<td><p>Street name and house number</p></td>
<td><p><code>Chemin de la Caquerette 12</code></p></td>
</tr>
<tr>
<td><p><code>addressType</code></p></td>
<td><p>

![[47553806568-check.png]]

</p></td>
<td><p>Must be <code>PRIVATE</code></p></td>
<td><p><code>PRIVATE</code></p></td>
</tr>
<tr>
<td><p><code>cityName</code></p></td>
<td><p>

![[47553806568-check.png]]

</p></td>
<td><p>The name of the city</p></td>
<td><p><code>Bern</code></p></td>
</tr>
<tr>
<td><p><code>cityZipCode</code></p></td>
<td><p>

![[47553806568-check.png]]

</p></td>
<td><p>The postcode</p></td>
<td><p><code>3003</code></p></td>
</tr>
<tr>
<td><p><code>countryIso2Code</code></p></td>
<td><p>

![[47553806568-check.png]]

</p></td>
<td><p>Must be <code>CH</code></p></td>
<td><p><code>CH</code></p></td>
</tr>
<tr>
<td><p><code>countryIso3Code</code></p></td>
<td><p>Optional</p></td>
<td><p>Must be <code>CHE</code></p></td>
<td><p><code>CHE</code></p></td>
</tr>
<tr>
<td><p><code>countryNumericCode</code></p></td>
<td><p>

![[47553806568-check.png]]

</p></td>
<td><p>Must be <code>756</code></p></td>
<td><p><code>756</code></p></td>
</tr>
<tr>
<td><p><code>additionalAddress</code></p></td>
<td><p>Optional</p></td>
<td><p>More information about the address</p></td>
<td><p><code>No. 13, street 123</code></p></td>
</tr>
</tbody>
</table>

</div>

Example with postman in **DEV environment**:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="979a0a47-1ad3-4b88-8d16-8e322c47b088" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
curl --location 'https://api-dev.klara.tech/core/v2/tenants/companies' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer ewogICJleHAiOiAxNzAwMDE2MDg3LAogICJpYXQiOiAxNzAwMDEyNDg3LAogICJqdGkiOiAiMWRhNDVkZWItZjE4Yy00NDQ5LTgxZDctZTE2M2M0NTA5YzhmIiwKICAiaXNzIjogImh0dHBzOi8vbG9naW4tZGV2LXZuLmtsYXJhLnRlY2gvYXV0aC9yZWFsbXMva2xhcmEiLAogICJzdWIiOiAiODJiNDMzYmUtZmI0Ny00ZGJiLTkxMTgtODM4NzAwMTE4YzdiIiwKICAidHlwIjogIkJlYXJlciIsCiAgImF6cCI6ICJrbGFyYS1wdWJsaWMtYXBpIiwKICAic2Vzc2lvbl9zdGF0ZSI6ICJhNTNhZDM3NS03OWU5LTRmMWMtOWZlYS05YWQ0NDczYTE1ZWYiLAogICJzY29wZSI6ICJwcm9maWxlIGVtYWlsIiwKICAic2lkIjogImE1M2FkMzc1LTc5ZTktNGYxYy05ZmVhLTlhZDQ0NzNhMTVlZiIsCiAgInRlbmFudF9pZCI6ICIyMjJiMTlmNC1iYzUxLTQ1MjYtOWY2My02YWJkZWYzODAyMjIiLAogICJlbWFpbF92ZXJpZmllZCI6IGZhbHNlLAogICJjb21wYW55X2lkIjogIjEiLAogICJuYW1lIjogInRlc3QgYWNjb3VudCIsCiAgInByZWZlcnJlZF91c2VybmFtZSI6ICJleGFtcGxlQGF4b25ncm91cGlvLmNoIiwKICAiZ2l2ZW5fbmFtZSI6ICJ0ZXN0IiwKICAiZmFtaWx5X25hbWUiOiAiYWNjb3VudCIsCiAgImVtYWlsIjogImV4YW1wbGVAYXhvbmdyb3VwaW8uY2giCn0=' \
--data-raw '{
  "name": "New Company",
  "legalForm": "KG",
  "phones": [
    {
      "phoneNumber": "+41 78 123 23 43",
      "type": "OFFICE"
    }
  ],
  "emails": [
    {
      "emailAddress": "example@gmail.com",
      "type": "OFFICE"
    }
  ],
  "addresses": [
    {
      "addressLines": "Chemin de la Caquerette 12",
      "addressType": "PRIVATE",
      "cityName": "Bern",
      "cityZipCode": "3003",
      "countryIso2Code": "CH",
      "countryIso3Code": "CHE",
      "countryNumericCode": "756",
      "additionalAddress": "No. 13, street 123"
    }
  ],
  "language": "de"
}'
```

</div>

</div>

Response example:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="560f101c-c093-484e-8b60-456903c85f29" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
    "tenant_id": "1e4b3fa8-6fbb-42af-bfdf-ac65ece1bec1",
    "company_id": 1,
    "company_name": "Sport Club"
}
```

</div>

</div>


![[47553806568-information.png]]

 When using Klara's service for the created company, generate a new token with its tenant ID (not from Fairgate company in Klara).

Try out in DEV environment: <a href="https://api-dev.klara.tech/docs#/Company/post_core_v2_tenants_companies" class="external-link" data-card-appearance="inline" rel="nofollow">https://api-dev.klara.tech/docs#/Company/post_core_v2_tenants_companies</a>


![[47553806568-image-20231117-011614.png]]



------------------------------------------------------------------------

<div class="panel conf-macro output-block" hasbody="true" macro-id="23e82a73-1d15-47d6-b813-626e060863eb" macro-name="panel" style="background-color: #E3FCEF;border-width: 1px;">

<div class="panelContent" style="background-color: #E3FCEF;">

# 🏢 **Get company information**

</div>

</div>


![[47553806568-information.png]]

 This API helps retrieve company information in Klara.

The URI:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="aa277265-3f17-4dec-bdcb-27612607f2a4" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
https://api.klara.ch/core/v2/tenants/companies
```

</div>

</div>

Method: <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-complete conf-macro output-inline" hasbody="false" macro-id="8dd812aa-f045-4a94-bed5-685682111857" macro-name="status">GET</span>

Header:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="179f5054-8598-43e9-a81f-17538a045f5a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Accept: application/json
Authorization: bearer token -> this token is created with the username, password and tenantId of the company that need to retrieve the data
```

</div>

</div>

Example with postman in **DEV environment**:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ecacb732-0740-4197-8bcf-de3f3eb88e0a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
curl --location 'https://api-dev-vn.klara.tech/core/v2/tenants/companies' \
--header 'Authorization: Bearer ewogICJleHAiOiAxNzAwMDE2MDg3LAogICJpYXQiOiAxNzAwMDEyNDg3LAogICJqdGkiOiAiMWRhNDVkZWItZjE4Yy00NDQ5LTgxZDctZTE2M2M0NTA5YzhmIiwKICAiaXNzIjogImh0dHBzOi8vbG9naW4tZGV2LXZuLmtsYXJhLnRlY2gvYXV0aC9yZWFsbXMva2xhcmEiLAogICJzdWIiOiAiODJiNDMzYmUtZmI0Ny00ZGJiLTkxMTgtODM4NzAwMTE4YzdiIiwKICAidHlwIjogIkJlYXJlciIsCiAgImF6cCI6ICJrbGFyYS1wdWJsaWMtYXBpIiwKICAic2Vzc2lvbl9zdGF0ZSI6ICJhNTNhZDM3NS03OWU5LTRmMWMtOWZlYS05YWQ0NDczYTE1ZWYiLAogICJzY29wZSI6ICJwcm9maWxlIGVtYWlsIiwKICAic2lkIjogImE1M2FkMzc1LTc5ZTktNGYxYy05ZmVhLTlhZDQ0NzNhMTVlZiIsCiAgInRlbmFudF9pZCI6ICIyMjJiMTlmNC1iYzUxLTQ1MjYtOWY2My02YWJkZWYzODAyMjIiLAogICJlbWFpbF92ZXJpZmllZCI6IGZhbHNlLAogICJjb21wYW55X2lkIjogIjEiLAogICJuYW1lIjogInRlc3QgYWNjb3VudCIsCiAgInByZWZlcnJlZF91c2VybmFtZSI6ICJleGFtcGxlQGF4b25ncm91cGlvLmNoIiwKICAiZ2l2ZW5fbmFtZSI6ICJ0ZXN0IiwKICAiZmFtaWx5X25hbWUiOiAiYWNjb3VudCIsCiAgImVtYWlsIjogImV4YW1wbGVAYXhvbmdyb3VwaW8uY2giCn0='
```

</div>

</div>

Response example:


![[47553806568-star_yellow.png]]

 About the address: The API will return the list address. The latest address is the address object with **largest id.**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="cdfa4705-f2f3-42a2-8d46-5c014c9b693b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
    "name": "Sport club",
    "legalForm": "INDIVIDUALLY_OWNED_COMPANY",
    "phones": [
        {
            "id": "1",
            "phoneNumber": "+41 78 123 23 43",
            "type": "OFFICE"
        }
    ],
    "emails": [
        {
            "id": "1",
            "emailAddress": "example@gmail.com",
            "type": "OFFICE"
        }
    ],
    "addresses": [
        {
            "id": "1",
            "validFrom": "2023-12-04",
            "validTo": null,
            "addressLines": "Chemin de la Caquerette 12",
            "addressType": "PRIVATE",
            "cityName": "Bern",
            "cityZipCode": "3003",
            "countryIso2Code": "CH",
            "countryIso3Code": "CHE",
            "countryNumericCode": "756",
            "definitionName": null,
            "additionalAddress": "No. 13, street 123",
            "city_href": null
        }
    ],
    "language": "de",
    "foundingDate": "2019-12-20"
}
```

</div>

</div>

Try out in DEV environment: <a href="https://api-dev.klara.tech/docs#/Company/post_core_v2_tenants_companies" class="external-link" data-card-appearance="inline" rel="nofollow">https://api-dev.klara.tech/docs#/Company/post_core_v2_tenants_companies</a>


![[47553806568-image-20231117-011740.png]]



------------------------------------------------------------------------

<div class="panel conf-macro output-block" hasbody="true" macro-id="5b31b559-df13-41d7-aab2-0dfbe95eb89b" macro-name="panel" style="background-color: #E3FCEF;border-width: 1px;">

<div class="panelContent" style="background-color: #E3FCEF;">

# 🏢 **Update company**

</div>

</div>


![[47553806568-information.png]]

 This API help you to update the company in Klara.

The URI:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1b79bb57-339a-4dc6-9169-019fc3cb6209" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
https://api.klara.ch/core/v2/tenants/companies
```

</div>

</div>

Method: <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-current conf-macro output-inline" hasbody="false" macro-id="c474f6f1-ddcc-4e07-bfcc-d1959d9eda0f" macro-name="status">PUT</span>

Header:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="2a228262-e542-430c-ac32-094cabf8d683" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Accept: application/json
Authorization: bearer token -> this token is created with the username, password and tenantId of the company that need to update the data
```

</div>

</div>

Body:


![[47553806568-star_yellow.png]]

 About the body, let’s call API to get the company information and then update the company information of the response.

<div>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<tbody>
<tr>
<th colspan="2"><p><strong>Field</strong></p></th>
<th><p><strong>Is required</strong></p></th>
<th><p><strong>Description</strong></p></th>
<th><p><strong>Example</strong></p></th>
</tr>
&#10;<tr>
<td colspan="2"><p><code>name</code></p></td>
<td><p>

![[47553806568-check.png]]

</p></td>
<td><p>the name of the company</p></td>
<td><p><code>New Company</code></p></td>
</tr>
<tr>
<td colspan="2"><p><code>legalForm</code></p></td>
<td><p>

![[47553806568-check.png]]

</p></td>
<td><p>the legal form of the company</p></td>
<td><div id="expander-1394333351" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="a00abccd-de7c-4fa7-aa0b-102148c66b93" data-macro-name="expand">
<div id="expander-control-1394333351" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">data</span>
</div>
<div id="expander-content-1394333351" class="expand-content expand-hidden">
<p>Individually owned company → <code>INDIVIDUALLY_OWNED_COMPANY</code> or <code>EU</code></p>
<p>General partnership → <code>GENERAL_PARTNERSHIP</code> or <code>KG</code></p>
<p>Limited partnership → <code>LIMITED_PARTNERSHIP</code> or <code>KDG</code></p>
<p>Public limited company → <code>CORPORATION</code> or <code>AG</code></p>
<p>Limited liability company → <code>LIMITED_LIABILITY</code> or <code>GmbH</code></p>
<p>Cooperative company → <code>COOPERATIVE_COMPANY</code> or <code>G</code></p>
<p>Association → <code>ASSOCIATION</code> or <code>V</code></p>
<p>Foundation → <code>FOUNDATION</code> or <code>S</code></p>
<p>Simple partnership → <code>SIMPLE_PARTNERSHIP</code> or <code>EG</code></p>
<p>Other → <code>OR</code></p>
</div>
</div></td>
</tr>
<tr>
<td colspan="2"><p><code>language</code></p></td>
<td><p>

![[47553806568-check.png]]

</p></td>
<td><p>German: <strong>de</strong></p>
<p>English: <strong>en</strong></p>
<p>French: <strong>fr</strong></p>
<p>Italian: <strong>it</strong></p></td>
<td><p><code>en</code></p></td>
</tr>
<tr>
<td colspan="2"><p><code>foundingDate</code></p></td>
<td><p>Optional</p></td>
<td><p>Format: <code>YYYY-MM-DD</code></p></td>
<td><p><code>2019-12-20</code></p></td>
</tr>
<tr>
<td rowspan="3"><p><code>phones</code></p></td>
<td><p><code>id</code></p></td>
<td><p>

![[47553806568-check.png]]

</p></td>
<td><p>The id of the phone object</p></td>
<td></td>
</tr>
<tr>
<td><p><code>phoneNumber</code></p></td>
<td><p>

![[47553806568-check.png]]

</p></td>
<td><p>The phone number of the company</p></td>
<td><p><code>+41783334444</code></p></td>
</tr>
<tr>
<td><p><code>type</code></p></td>
<td><p>

![[47553806568-check.png]]

</p></td>
<td><p>The phone type, must be <code>OFFICE</code></p></td>
<td><p><code>OFFICE</code></p></td>
</tr>
<tr>
<td rowspan="3"><p><code>emails</code></p></td>
<td><p><code>id</code></p></td>
<td><p>

![[47553806568-check.png]]

</p></td>
<td><p>The id of the email object</p></td>
<td></td>
</tr>
<tr>
<td><p><code>emailAddress</code></p></td>
<td><p>

![[47553806568-check.png]]

</p></td>
<td><p>The email of the company</p></td>
<td><p><code>example@gmail.com</code></p></td>
</tr>
<tr>
<td><p><code>type</code></p></td>
<td><p>

![[47553806568-check.png]]

</p></td>
<td><p>The email type, must be <code>OFFICE</code></p></td>
<td><p><code>OFFICE</code></p></td>
</tr>
<tr>
<td rowspan="10"><p><code>addresses</code></p></td>
<td colspan="4"><p>

![[47553806568-star_yellow.png]]

 API get information will return the list address. Let’s update the address object with <strong>largest id</strong></p></td>
</tr>
<tr>
<td><p><code>id</code></p></td>
<td><p>

![[47553806568-check.png]]

</p></td>
<td><p>The id of the address object</p></td>
<td></td>
</tr>
<tr>
<td><p><code>addressLines</code></p></td>
<td><p>

![[47553806568-check.png]]

</p></td>
<td><p>Street name and house number</p></td>
<td><p><code>Chemin de la Caquerette 12</code></p></td>
</tr>
<tr>
<td><p><code>addressType</code></p></td>
<td><p>

![[47553806568-check.png]]

</p></td>
<td><p>Must be <code>PRIVATE</code></p></td>
<td><p><code>PRIVATE</code></p></td>
</tr>
<tr>
<td><p><code>cityName</code></p></td>
<td><p>

![[47553806568-check.png]]

</p></td>
<td><p>The name of the city</p></td>
<td><p><code>Bern</code></p></td>
</tr>
<tr>
<td><p><code>cityZipCode</code></p></td>
<td><p>

![[47553806568-check.png]]

</p></td>
<td><p>The postcode</p></td>
<td><p><code>3003</code></p></td>
</tr>
<tr>
<td><p><code>countryIso2Code</code></p></td>
<td><p>

![[47553806568-check.png]]

</p></td>
<td><p>Must be <code>CH</code></p></td>
<td><p><code>CH</code></p></td>
</tr>
<tr>
<td><p><code>countryIso3Code</code></p></td>
<td><p>Optional</p></td>
<td><p>Must be <code>CHE</code></p></td>
<td><p><code>CHE</code></p></td>
</tr>
<tr>
<td><p><code>countryNumericCode</code></p></td>
<td><p>

![[47553806568-check.png]]

</p></td>
<td><p>Must be <code>756</code></p></td>
<td><p><code>756</code></p></td>
</tr>
<tr>
<td><p><code>additionalAddress</code></p></td>
<td><p>Optional</p></td>
<td><p>More information about the address</p></td>
<td><p><code>No. 13, street 123</code></p></td>
</tr>
</tbody>
</table>

</div>

Example with postman in **DEV environment**:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3d9c74c7-7b38-4b9d-b187-22f3d72a4b0b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
curl --location --request PUT 'https://api-dev.klara.tech/core/v2/tenants/companies' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer ewogICJleHAiOiAxNzAwMDE2MDg3LAogICJpYXQiOiAxNzAwMDEyNDg3LAogICJqdGkiOiAiMWRhNDVkZWItZjE4Yy00NDQ5LTgxZDctZTE2M2M0NTA5YzhmIiwKICAiaXNzIjogImh0dHBzOi8vbG9naW4tZGV2LXZuLmtsYXJhLnRlY2gvYXV0aC9yZWFsbXMva2xhcmEiLAogICJzdWIiOiAiODJiNDMzYmUtZmI0Ny00ZGJiLTkxMTgtODM4NzAwMTE4YzdiIiwKICAidHlwIjogIkJlYXJlciIsCiAgImF6cCI6ICJrbGFyYS1wdWJsaWMtYXBpIiwKICAic2Vzc2lvbl9zdGF0ZSI6ICJhNTNhZDM3NS03OWU5LTRmMWMtOWZlYS05YWQ0NDczYTE1ZWYiLAogICJzY29wZSI6ICJwcm9maWxlIGVtYWlsIiwKICAic2lkIjogImE1M2FkMzc1LTc5ZTktNGYxYy05ZmVhLTlhZDQ0NzNhMTVlZiIsCiAgInRlbmFudF9pZCI6ICIyMjJiMTlmNC1iYzUxLTQ1MjYtOWY2My02YWJkZWYzODAyMjIiLAogICJlbWFpbF92ZXJpZmllZCI6IGZhbHNlLAogICJjb21wYW55X2lkIjogIjEiLAogICJuYW1lIjogInRlc3QgYWNjb3VudCIsCiAgInByZWZlcnJlZF91c2VybmFtZSI6ICJleGFtcGxlQGF4b25ncm91cGlvLmNoIiwKICAiZ2l2ZW5fbmFtZSI6ICJ0ZXN0IiwKICAiZmFtaWx5X25hbWUiOiAiYWNjb3VudCIsCiAgImVtYWlsIjogImV4YW1wbGVAYXhvbmdyb3VwaW8uY2giCn0=' \
--data-raw '{
    "name": "Sport club updated",
    "legalForm": "INDIVIDUALLY_OWNED_COMPANY",
    "phones": [
        {
            "id": "1",
            "phoneNumber": "+41 78 123 23 43",
            "type": "OFFICE"
        }
    ],
    "emails": [
        {
            "id": "1",
            "emailAddress": "example@gmail.com",
            "type": "OFFICE"
        }
    ],
    "addresses": [
        {
            "id": "1",
            "validFrom": "2023-12-04",
            "validTo": null,
            "addressLines": "Chemin de la Caquerette 12",
            "addressType": "PRIVATE",
            "cityName": "Bern",
            "cityZipCode": "3003",
            "countryIso2Code": "CH",
            "countryIso3Code": "CHE",
            "countryNumericCode": "756",
            "definitionName": null,
            "additionalAddress": "No. 13, street 123",
            "city_href": null
        }
    ],
    "language": "de",
    "foundingDate": "2019-12-22"
}'
```

</div>

</div>

Response example:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="00f3ac3c-0ecf-4295-a148-26726cf73d27" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
    "name": "Sport club updated",
    "legalForm": "INDIVIDUALLY_OWNED_COMPANY",
    "phones": [
        {
            "id": "1",
            "phoneNumber": "+41 78 123 23 43",
            "type": "OFFICE"
        }
    ],
    "emails": [
        {
            "id": "1",
            "emailAddress": "example@gmail.com",
            "type": "OFFICE"
        }
    ],
    "addresses": [
        {
            "id": "2",
            "validFrom": "2023-12-04",
            "validTo": null,
            "addressLines": "Chemin de la Caquerette 12",
            "addressType": "PRIVATE",
            "cityName": "Bern",
            "cityZipCode": "3003",
            "countryIso2Code": "CH",
            "countryIso3Code": "CHE",
            "countryNumericCode": "756",
            "definitionName": null,
            "additionalAddress": "No. 13, street 123",
            "city_href": null
        }
    ],
    "language": "de",
    "foundingDate": "2019-12-22"
}
```

</div>

</div>

Try out in DEV environment: <a href="https://api-dev.klara.tech/docs#/Company/post_core_v2_tenants_companies" class="external-link" data-card-appearance="inline" rel="nofollow">https://api-dev.klara.tech/docs#/Company/post_core_v2_tenants_companies</a>


![[47553806568-image-20231117-011650.png]]
