---
ai_hash: 43a704880acc2e42
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 20
depth: 2.4
entities: []
relevance: 0.731
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20525664929/Authentication+and+Authorization+-+PoC+document
space: LUZ
status: reference
tags:
- confluence
- security
- space/luz
title: Authentication and Authorization - PoC document
topic: security
type: source
updated: 2021-04-02
---

# Authentication and Authorization - PoC document

> [!info] Imported from Confluence
> Space **LUZ** · updated 2021-04-02 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20525664929/Authentication+and+Authorization+-+PoC+document)
> Relevance 0.731 · topic `security`

## **Situation** 

  


![[20525664929-image2021-3-24_10-17-21.png]]



We already removed policy from Minio.

**This implementation already deployed on GCP.**

## **Authentication**

We only accept for acces token that get from Keycloak then we can call API of Luz-docs


![[20525664929-image2021-3-26_10-38-33.png]]



## **Authorization**

We will do authorization base on the Tenant-id in JWT token that field is "scope"

  

  


![[20525664929-image2021-3-24_10-21-11.png]]



for the access token that have same scope can call the API that have tenant match the scope (the path parama that I hilghlighted in screenshot below)

  


![[20525664929-image2021-3-26_10-47-18.png]]



## **Authentication and Authorization flow **


![[20525664929-Authentication and Authorization Flow-2.png]]



####  **Request upload/dowload with Token:**

User will get the Token from keycloak then he/she will create request to Luz-docs

Example:  get token from keycloak refer <a href="https://developers.redhat.com/blog/2020/01/29/api-login-and-jwt-token-generation-using-keycloak/" class="external-link" rel="nofollow" style="text-decoration: none;text-align: left;">https://developers.redhat.com/blog/2020/01/29/api-login-and-jwt-token-generation-using-keycloak/</a> for more detail, we will get the access token from response of this request 

After we have token, we will call request for Upload or dowload file  

![[20525664929-image2021-2-23_16-43-46.png]]



  

**Validate Token:**

luz-docs has the filter to validate token  by public key of Keycloak

  

**Create Minio client**

For each request from user, we always create minio client for connecting to Minio server then we will do some action base on Minio credentials.

We store the access key and scret key on environment variable


![[20525664929-image2021-3-26_11-12-46.png]]



**  **

After we create minio client, we will use it for some action (upload/download) of request

**Authorization**

We check the tenant id in access token that match Path param of request API

%% ai-graph-start %%

**Related notes:**
- [[HowToUseNewTokenAPI]]
- [[Vault overview]]
- [[Luz 403 Not allowed means the token has no tenant claim, not a missing permission]]
- [[Authorization]]
- [[Token JWT Security]]

%% ai-graph-end %%