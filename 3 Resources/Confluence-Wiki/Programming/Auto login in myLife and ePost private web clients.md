---
ai_hash: 89ad47b9ce6dba9d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 8
depth: 2.89
entities: []
relevance: 0.716
source: https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/46967194388/Auto+login+in+myLife+and+ePost+private+web+clients
space: TP2020
status: reference
tags:
- confluence
- programming
- space/tp2020
title: Auto login in myLife and ePost private web clients
topic: programming
type: source
updated: 2021-10-02
---

# Auto login in myLife and ePost private web clients

> [!info] Imported from Confluence
> Space **TP2020** · updated 2021-10-02 · [open original](https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/46967194388/Auto+login+in+myLife+and+ePost+private+web+clients)
> Relevance 0.716 · topic `programming`

A new REST API resource was introduced in luz-keycloak that returns a URL to log in into KLARA/ePost automatically with the following information:

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
<th></th>
<th><p><strong>Value</strong></p></th>
<th><p><strong>Description</strong></p></th>
<th><p><strong>Example</strong></p></th>
</tr>
&#10;<tr>
<td><p><em>Method</em></p></td>
<td><p>POST</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p><em>URL</em></p></td>
<td><p>{{baseUrl}}/auth/realms/:realm-name/luz-auto-login/url</p></td>
<td></td>
<td><p>https://login-dev-vn.klara.tech/auth/realms/klara/luz-auto-login/url</p></td>
</tr>
<tr>
<td><p><em>Authorization</em></p></td>
<td><p>token</p></td>
<td><p>the <em>keycloak</em> <strong>access_token</strong> or <strong>refresh_token</strong></p></td>
<td></td>
</tr>
<tr>
<td><p><em>Path param</em></p></td>
<td><p>realm-name</p></td>
<td><p>the selected keycloak realm name</p></td>
<td><p>KLARA</p>
<p>master</p></td>
</tr>
<tr>
<td><p><em><span class="inline-comment-marker" data-ref="58d67c6b-8fb0-44f4-8e04-6e1338ed327c">Form param</span></em></p></td>
<td><p><span class="inline-comment-marker" data-ref="58d67c6b-8fb0-44f4-8e04-6e1338ed327c">client_id</span></p></td>
<td><p><span class="inline-comment-marker" data-ref="58d67c6b-8fb0-44f4-8e04-6e1338ed327c">the selected client in the realm</span></p></td>
<td><p><span class="inline-comment-marker" data-ref="58d67c6b-8fb0-44f4-8e04-6e1338ed327c">klara</span></p>
<p><span class="inline-comment-marker" data-ref="58d67c6b-8fb0-44f4-8e04-6e1338ed327c">epost</span></p></td>
</tr>
<tr>
<td><p><em>Form param</em></p></td>
<td><p>redirect_url</p></td>
<td><p>the URL to redirect to after logging in successfully</p></td>
<td><p>https://dev-vn.klara.tech</p></td>
</tr>
<tr>
<td rowspan="3"><p><em>Response</em></p></td>
<td><p>Status: 200 OK</p></td>
<td><p>Auto login URL</p></td>
<td></td>
</tr>
<tr>
<td><p>Status: 400 Bad Request</p></td>
<td><p>“Client id can not be null“</p></td>
<td></td>
</tr>
<tr>
<td><p>Status: 400 Bad Request</p></td>
<td><p>"redirect Url can not be null"</p></td>
<td></td>
</tr>
</tbody>
</table>

</div>

## Notes

- User can generate the auto login URL and use it only when he/she has the valid access token (logged in), otherwise an exception will be thrown.

- The auto login URL can be used only one time, it will be expired once the user has successfully logged in by that URL.

- The URL itself also has an expiration time (the action token expiration time) which can be configured in Keycloak.

## Postman Usage Example

**Step 1: Get access token**

Call the following API to get *keycloak* tokens (**access_token** and **refresh_token**):

**POST** {{keycloakBaseUrl}}/auth/realm/{{realmNames}}/protocol/openid-connect/token

Form params:

- grant_type

- username

- password

- client_id

- client_secret


![[46967194388-image-20211001-104504.png]]



**Step 2: Get auto login URL**

Call to auto login resource using POST method, the URL format is : {{keycloakBaseUrl}}/auth/realm/{{realmNames}}/`autologin/url`

Firstly we need to pass the access token in the authorization


![[46967194388-image-20211001-092454.png]]



Secondly we need client_id and redirect_url in the request form


![[46967194388-image-20211001-092612.png]]



Trigger the call, the response will be the full auto login URL, if the is no exception (token is valid, client_id is not empty, redirect url is not empty)


![[46967194388-image-20211001-092711.png]]



Enter that URL to the browser to process the auto login flow without entering username and password

This is the **keycloak** Postman collection for trying out:

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="f655cb3e-47e6-4266-8a72-98c62c316411" macro-name="view-file"><a href="../_attachments/46967194388-keycloak.postman_collection.json" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/46967194388/keycloak.postman_collection.json?version=4&amp;modificationDate=1633083864868&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/json" data-has-thumbnail="true">

![[46967194388-keycloak.postman_collection.json]]

</a></span>

%% ai-graph-start %%

**Related notes:**
- [[Proof of Concept Auto login with Keycloak]]
- [[Understanding Keycloak Authorization Code flow]]
- [[KLARA Integration (request access token & call API)]]
- [[Onboarding API - Investigation Identity Provider SSO]]
- [[Copy Proof of Concept Passwordless account login with Keycloak]]

%% ai-graph-end %%