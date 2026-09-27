---
title: "Understanding Keycloak Authorization Code flow"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/47337506247/Understanding+Keycloak+Authorization+Code+flow
space: "Helios"
topic: security
relevance: 0.701
depth: 2.4
updated: 2023-03-27
attachments: 0
tags:
  - confluence
  - security
  - space/helios
---

# Understanding Keycloak Authorization Code flow

> [!info] Imported from Confluence
> Space **Helios** · updated 2023-03-27 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/47337506247/Understanding+Keycloak+Authorization+Code+flow)
> Relevance 0.701 · topic `security`

In this session, we will have a summarize with examples, easier for us to understand the login flow

There are 2 requests:

1.  **Request to get authorization code**.

2.  **Exchange authorization code to get token**.

------------------------------------------------------------------------

**<u>Step 1:</u>** Request to get access code:

Sample request:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="48804de9-364b-425c-a779-a5b63fc3647b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
https://login-dev.klara.tech/auth/realms/klara/protocol/openid-connect/auth
?client_id=klara-mobile
&response_type=code  -> this is mandatory
&state=fj8o3n7bdy1op5  -> recommended, random value, opaque value used by the client to maintain state between the request and callback
&redirect_uri=http://localhost:8081/callback
&kc_idp_hint=swissid   -> specific to Keycloak's Provider
```

</div>

</div>

Sample Response:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="68d8da08-9e01-4b7f-9e3a-464a1c8487a0" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
http://localhost:8081/callback
?state=fj8o3n7bdy1op5
&session_state=f109bb89-cd34-4374-b084-c3c1cf2c8a0b
&code=0aaca7b5-a314-4c07-8212-818cb4b7e8d0.f109bb89-cd34-4374-b084-c3c1cf2c8a0b.1dc15d06-d8b9-4f0f-a042-727eaa6b98f7
```

</div>

</div>

------------------------------------------------------------------------

**<u>Step 2</u>**: Exchange authorization code for token

Sample request:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="44d90b17-3548-44f9-a143-b5d4a9a2b336" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
curl --location --request POST 'https://login-dev.klara.tech/auth/realms/klara/protocol/openid-connect/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'grant_type=authorization_code' \
--data-urlencode 'client_id=photo-app-code-flow-client' \
--data-urlencode 'client_secret=3424193f-4728-4d19-8517-d450d7c6f2f5' \
--data-urlencode 'code=c081f6ca-ae87-40b6-8138-5afd4162d181.f109bb89-cd34-4374-b084-c3c1cf2c8a0b.1dc15d06-d8b9-4f0f-a042-727eaa6b98f7' \
--data-urlencode 'redirect_uri=http://localhost:8081/callback'
```

</div>

</div>

Sample response:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b5efe421-b3a4-40e0-a55c-d1d596d38b9c" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJPVkMyV29PVjdfMGJkR1lqSFJYb1FZVE0yekhNOHlJQVh2YWY2dEhnQXpZIn0.eyJleHAiOjE2Nzk1NTc1MDQsImlhdCI6MTY3OTU1MzkwNCwiYXV0aF90aW1lIjoxNjc5NTUzODk5LCJqdGkiOiJiNDIyMTNjYS04YTEzLTQzZmQtOWFkNC0xNzk5MmY3NTc5YTUiLCJpc3MiOiJodHRwczovL2xvZ2luLWRldi5rbGFyYS50ZWNoL2F1dGgvcmVhbG1zL2tsYXJhIiwiYXVkIjpbImJyb2tlciIsImFjY291bnQiXSwic3ViIjoiZjAwZTQzYWYtNzZjNS00YjQwLWI4NDItMTQxZDNjODM1MWQzIiwidHlwIjoiQmVhcmVyIiwiYXpwIjoia2xhcmEtbW9iaWxlIiwic2Vzc2lvbl9zdGF0ZSI6IjhiODc0ZDIxLWNjYjMtNDc1MC04MzJiLTQ4ZGRiNDNiMDBiZiIsImFjciI6IjEiLCJhbGxvd2VkLW9yaWdpbnMiOlsiaHR0cHM6Ly9rbGFyYS1kZXYtc3RhZ2luZy5heG9uaXZ5LmlvIiwiaW9uaWM6Ly9sb2NhbGhvc3QiLCJodHRwczovL2tsYXJhLWRldi5heG9uaXZ5LmlvLyoiLCJodHRwOi8vbG9jYWxob3N0OjgxMDAiLCJteWFwcDovL3JlZGlyZWN0Il0sInJlYWxtX2FjY2VzcyI6eyJyb2xlcyI6WyJkZWZhdWx0LXJvbGVzLWtsYXJhIiwib2ZmbGluZV9hY2Nlc3MiLCJ1bWFfYXV0aG9yaXphdGlvbiJdfSwicmVzb3VyY2VfYWNjZXNzIjp7ImJyb2tlciI6eyJyb2xlcyI6WyJyZWFkLXRva2VuIl19LCJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6Im9wZW5pZCBwcm9maWxlIHBob25lIGFkZHJlc3MgZW1haWwiLCJzaWQiOiI4Yjg3NGQyMS1jY2IzLTQ3NTAtODMyYi00OGRkYjQzYjAwYmYiLCJhZGRyZXNzIjp7fSwiZW1haWxfdmVyaWZpZWQiOnRydWUsImdlbmRlciI6Im1hbGUiLCJuYW1lIjoiaHVuZyB5b3AxIiwicHJlZmVycmVkX3VzZXJuYW1lIjoic3dpc3NpZC1teWswMDFAeW9wbWFpbC5jb20iLCJnaXZlbl9uYW1lIjoiaHVuZyIsImZhbWlseV9uYW1lIjoieW9wMSIsImVtYWlsIjoic3dpc3NpZC1teWswMDFAeW9wbWFpbC5jb20ifQ.R4Oy3gOGEu7FtEbWkUIpNjhIky7WIQi4tfaBxuphdxnh556sKVICcFkcuEx8vhEcNb0WuNCe-1rIDkuhibGrMs26NGjbXGtEGHxya_GN6CCw9pGPqrVAy6zNSwk1rVAMeyNoAu-yFnbJMNE69oD-7vY2jG9EdVfKOW6FNn2rD4u9Anhp5LOX4xpU85IYD1jAtHL-hqvAQq8ZTr2pI3tlIyPRfSHoqrp0xVFVfQUOkkOBvFVredLEW0enkbvo-HWNHxrq8EkGAJ5c_-JN7s3x8GTC0Co_0yu59EG0R9EzRb1QkJNrXupGyyvINA9_8pS0ekLtK4jn5-xJRr1-4KbV6A",
  "expires_in": 3600,
  "refresh_expires_in": 14400,
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI4NTk5M2UwMy1kOTVkLTRmMDYtOTJkYy0wMmExMjEwZTA0N2YifQ.eyJleHAiOjE2Nzk1NjgzMDQsImlhdCI6MTY3OTU1MzkwNCwianRpIjoiMTU3NGYyODUtNDlmYi00MzY2LWFiMTctMjVkZjBkZWIwMjZiIiwiaXNzIjoiaHR0cHM6Ly9sb2dpbi1kZXYua2xhcmEudGVjaC9hdXRoL3JlYWxtcy9rbGFyYSIsImF1ZCI6Imh0dHBzOi8vbG9naW4tZGV2LmtsYXJhLnRlY2gvYXV0aC9yZWFsbXMva2xhcmEiLCJzdWIiOiJmMDBlNDNhZi03NmM1LTRiNDAtYjg0Mi0xNDFkM2M4MzUxZDMiLCJ0eXAiOiJSZWZyZXNoIiwiYXpwIjoia2xhcmEtbW9iaWxlIiwic2Vzc2lvbl9zdGF0ZSI6IjhiODc0ZDIxLWNjYjMtNDc1MC04MzJiLTQ4ZGRiNDNiMDBiZiIsInNjb3BlIjoib3BlbmlkIHByb2ZpbGUgcGhvbmUgYWRkcmVzcyBlbWFpbCIsInNpZCI6IjhiODc0ZDIxLWNjYjMtNDc1MC04MzJiLTQ4ZGRiNDNiMDBiZiJ9.UjR-ueHBxknyS7tF2X_lgALvVfuTRiJUYqyyHAa28R0",
  "token_type": "Bearer",
  "id_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJPVkMyV29PVjdfMGJkR1lqSFJYb1FZVE0yekhNOHlJQVh2YWY2dEhnQXpZIn0.eyJleHAiOjE2Nzk1NTc1MDQsImlhdCI6MTY3OTU1MzkwNCwiYXV0aF90aW1lIjoxNjc5NTUzODk5LCJqdGkiOiIyMWEyNzM0Ni02ZDYzLTQ1M2YtYmFjZC0yMTJhZjRiMTljMjQiLCJpc3MiOiJodHRwczovL2xvZ2luLWRldi5rbGFyYS50ZWNoL2F1dGgvcmVhbG1zL2tsYXJhIiwiYXVkIjoia2xhcmEtbW9iaWxlIiwic3ViIjoiZjAwZTQzYWYtNzZjNS00YjQwLWI4NDItMTQxZDNjODM1MWQzIiwidHlwIjoiSUQiLCJhenAiOiJrbGFyYS1tb2JpbGUiLCJzZXNzaW9uX3N0YXRlIjoiOGI4NzRkMjEtY2NiMy00NzUwLTgzMmItNDhkZGI0M2IwMGJmIiwiYXRfaGFzaCI6ImRsdTNVckpmUFo1MUxmdEZDdjZfdEEiLCJhY3IiOiIxIiwic2lkIjoiOGI4NzRkMjEtY2NiMy00NzUwLTgzMmItNDhkZGI0M2IwMGJmIiwiYWRkcmVzcyI6e30sImVtYWlsX3ZlcmlmaWVkIjp0cnVlLCJnZW5kZXIiOiJtYWxlIiwibmFtZSI6Imh1bmcgeW9wMSIsInByZWZlcnJlZF91c2VybmFtZSI6InN3aXNzaWQtbXlrMDAxQHlvcG1haWwuY29tIiwiZ2l2ZW5fbmFtZSI6Imh1bmciLCJmYW1pbHlfbmFtZSI6InlvcDEiLCJlbWFpbCI6InN3aXNzaWQtbXlrMDAxQHlvcG1haWwuY29tIn0.Bb4norNizfcZvJ-i5ljf9N6bAy8uLXSCpZvmEXEnXYPwI-qaakHca1x77QeC90qgjKdv03pCJZ9YQLY9VtWOlaTmywbJRIlwQsJ_K8WlT0I0iEN718HQDNaDFbdRfwcblavM5Zau3byoljz0k_Otl5OdicyRI85Ka9oikOS_qUJ5N_Dh9bJJUiUG_lQ4p8-EbOYz83j8y6M1VDhZrz1OrD-kJmJqHAQpocYfpT5sCxyPX4qciPseUcnzkRbHThThKJKfyZJrsfoDoDS_UnZ4uSL0Vbq4EMBtGlWJrvh_rXdBLwrmwCC71kH-KOcSwO3N_f6EwC1E38zzcyWcHEHEZg",
  "not-before-policy": 1544094129,
  "session_state": "8b874d21-ccb3-4750-832b-48ddb43b00bf",
  "scope": "openid profile phone address email"
}
```

</div>

</div>
