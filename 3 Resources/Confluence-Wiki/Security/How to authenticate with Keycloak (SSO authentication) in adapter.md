---
title: "How to authenticate with Keycloak (SSO authentication) in adapter"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/RT/pages/47449407538/How+to+authenticate+with+Keycloak+SSO+authentication+in+adapter
space: "RT"
topic: security
relevance: 0.731
depth: 2.95
updated: 2023-09-15
attachments: 5
tags:
  - confluence
  - security
  - space/rt
---

# How to authenticate with Keycloak (SSO authentication) in adapter

> [!info] Imported from Confluence
> Space **RT** · updated 2023-09-15 · [open original](https://axonivy.atlassian.net/wiki/spaces/RT/pages/47449407538/How+to+authenticate+with+Keycloak+SSO+authentication+in+adapter)
> Relevance 0.731 · topic `security`

**1.Get the public key of realm from keycloak server:**  
- Go to keycloak server: <a href="https://10.123.1.68:8092/admin/" class="external-link" rel="nofollow">https://10.123.1.68:8092/admin/</a> (username: **admin**, password: **admin**)  
- Switch to realm **6300** → Realm setting → Keys tab → copy public key from “**RS256**“  


![[47449407538-image-20230801-101311.png]]



  


![[47449407538-image-20230801-101509.png]]



\- Set the public key to the “**publicKey.txt**“ external file in **service_mock**.  


![[47449407538-image-20230801-102743.png]]



**2.Change property config key in Adapter connect to Keycloak authentication:**  
%dev.quarkus.oidc-client.token-path = <a href="https://10.123.1.68:8092/realms/6300/protocol/openid-connect/token" class="external-link" rel="nofollow">https://10.123.1.68:8092/realms/6300/protocol/openid-connect/token</a>  
%dev.quarkus.oidc-client.client-id = **client-id**  
%dev.quarkus.oidc-client.credentials.client-secret.value = **client-secret**  
%dev.quarkus.oidc-client.grant.type = **password**  
%dev.quarkus.oidc-client.grant-options.password.username = **sso-service-user**  
%dev.quarkus.oidc-client.grant-options.password.password = **pleasechangeit**  
%dev.quarkus.oidc-client.scopes = **profile**  
%dev.quarkus.oidc-client.credentials.client-secret.method = post  
%dev.quarkus.oidc-client.headers.X-Requested-By = x-requested-by  
%dev.quarkus.oidc-client.tls.trust-store-file=<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="f9c3040b-a215-4654-8126-74fe2e307a62" macro-name="view-file"><a href="../_attachments/47449407538-mykeytool.jks" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47449407538/mykeytool.jks?version=2&amp;modificationDate=1694759887832&amp;cacheVersion=1&amp;api=v2" data-mime-type="binary/octet-stream" data-has-thumbnail="true">

![[47449407538-mykeytool.jks]]

</a></span>  
%dev.quarkus.oidc-client.tls.trust-store-password=**123456**  


![[47449407538-image-20230801-103307.png]]
