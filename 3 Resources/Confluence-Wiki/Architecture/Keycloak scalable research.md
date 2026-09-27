---
title: "Keycloak scalable research"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47143748565/Keycloak+scalable+research
space: "LUZ"
topic: architecture
relevance: 0.777
depth: 2.72
updated: 2022-08-06
attachments: 25
tags:
  - confluence
  - architecture
  - space/luz
---

# Keycloak scalable research

> [!info] Imported from Confluence
> Space **LUZ** · updated 2022-08-06 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47143748565/Keycloak+scalable+research)
> Relevance 0.777 · topic `architecture`

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="f2031f56c4b0b89e317f776491e070a5" macro-name="toc">

</div>

# Login flow and keycloak’s mission in klara authen flow:

This can be explain short and simple with this sequence diagram:


![[47143748565-image-20220714-040359.png]]



Example access the **dev-vn**:


![[47143748565-image-20220713-144137.png]]



decoded url:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e31583a5-060a-448c-96c5-bc4dc696624a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
https://login-dev-vn.klara.tech/auth/realms/klara/protocol/openid-connect/auth?client_id=klara&response_type=code&state=70cimjgjltsel7u7lrs68o8huj&redirect_uri=https://dev-vn.klara.tech/luz/pro/luz_web/148F53807F153C65/oauth_login.ivp&scope=openid+email+profile
```

</div>

</div>

# Current keycloak situation:

Currently behind **keycloak-service**, we just have **1 pod** to handle incoming requests that defined in **deployment yaml file** (just **1 replica**). For example in **dev** environment:


![[47143748565-image-20220713-145212.png]]



So if a lot of requests come to this pod and make it hang/die then klara also can not login, get tokens,…

That why we decide to make keycloak **scalable**.

# Current research progress:

According to this article: <a href="https://blog.sighup.io/keycloak-ha-on-kubernetes/" class="external-link" data-card-appearance="inline" rel="nofollow">https://blog.sighup.io/keycloak-ha-on-kubernetes/</a>

Also the book “**keycloak-identity-and-access-management-for-modern-applications**” keycloak can start with these **4 modes**:

- Standalone

- **Standalone Clustered**

- Domain Clustered

- Cross-Datacenter Replication Mode

These modes can be explain in this **image**:


![[47143748565-image-20220713-150325.png]]



In this research i follow the **Standalone Clustered** mode**.**

# Applying the research on dev-vn environment:

According to reference from ebook about keycloak, to start keycloak in clustering, ha mode, we must use the config from **standalone-ha.xml**

1.  **Config CACHE_OWNERS_COUNT and CACHE_OWNERS_AUTH_SESSIONS_COUNT**

The main ideal for config these params explained in this image:


![[47143748565-image-20220711-031221.png]]



and our current **standalone-ha.xml** look like this:


![[47143748565-image-20220711-031415.png]]



so in the deployment, i already specify like the book suggest:


![[47143748565-image-20220711-065217.png]]



**2. Config JGROUPS_DISCOVERY_PROTOCOL and JGROUPS_DISCOVERY_PROPERTIES, KUBERNETES_NAMESPACE**


![[47143748565-image-20220714-043941.png]]

![[47143748565-image-20220714-044259.png]]



To make the 2 pod instance of keycloak can communicate with each others, we config these properties, with the result from the pod we can see with this log:


![[47143748565-image-20220711-073809.png]]



These are some reference i refer to:

<a href="https://dzone.com/articles/setup-keycloak-cluster-with-kube-ping-in-kubernete" class="external-link" data-card-appearance="inline" rel="nofollow">https://dzone.com/articles/setup-keycloak-cluster-with-kube-ping-in-kubernete</a>

<a href="https://stackoverflow.com/q/70286956" class="external-link" data-card-appearance="inline" rel="nofollow">https://stackoverflow.com/q/70286956</a>

Also using a service-account (**get** and **list** permission) to make these keycloak pod instance can have permission to communicate.


![[47143748565-image-20220711-080303.png]]

![[47143748565-image-20220711-080317.png]]



**3. Config PROXY_ADDRESS_FORWARDING**

Currently we apply reverse proxy using nginx and already config `X-Forwarded-For` and `X-Forwarded-Proto`


![[47143748565-image-20220714-042626.png]]



According to the book:


![[47143748565-image-20220711-075434.png]]



The **X-Forward-For**, **X-Forward-Proto** is already config in nginx

for **proxy-address-forwarding**, we inject it through env with name `PROXY_ADDRESS_FORWARDING`


![[47143748565-image-20220711-075655.png]]



**4. Session affinity:**

<a href="https://github.com/keycloak/keycloak-documentation/blob/main/server_installation/topics/clustering/sticky-sessions.adoc" class="external-link" data-card-appearance="inline" rel="nofollow">https://github.com/keycloak/keycloak-documentation/blob/main/server_installation/topics/clustering/sticky-sessions.adoc</a>


![[47143748565-image-20220713-151410.png]]



Still in progress with this PR:

<a href="https://bitbucket.org/axonivy-prod/keycloak_klara_theme/pull-requests/215/luz-79396-try-to-apply-session-affinity" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/keycloak_klara_theme/pull-requests/215/luz-79396-try-to-apply-session-affinity</a>

My current branch for this keycloak ha adaption:

<a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/pull-requests/2809/arrow-59-luz-79396-keycloak-high" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/pull-requests/2809/arrow-59-luz-79396-keycloak-high</a>

**5. Tryout in dev-vn environment:**


![[47143748565-image-20220713-152236.png]]



# Testing and demo:

- Change password flow.

- Admin page

# Open points:

- graceful shutdown

- horizontal auto scale

- stress test

# Reference pages:

…

# <span class="inline-comment-marker" ref="f295c62f-9f23-43b0-88c5-4c9e93b6e403">Offline session issue when apply</span>

**What is offline session of keycloak:**


![[47143748565-image-20220729-035154.png]]



**Current issue when apply running 2 pods:**

There is an issue related to this when exchange refresh token, as the log show in 2 pods that neither of them can not return the offline user session when the request come, here is the images:


![[47143748565-image-20220729-102852.png]]

![[47143748565-image-20220729-103005.png]]



After 2 times try to find the offline user-session, it stop but i think it should query to the database also but no and end with return refresh_token_error like the image:


![[47143748565-image-20220729-103308.png]]



Until now I not yet have any solution for this issue.

I attach the postman collection for testing this case

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="42fbb8fb-a554-4168-9100-058c4db43883" macro-name="view-file"><a href="../_attachments/47143748565-keycloak-scale-test.postman_collection.json" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47143748565/keycloak-scale-test.postman_collection.json?version=1&amp;modificationDate=1659094572977&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/json" data-has-thumbnail="true">

![[47143748565-keycloak-scale-test.postman_collection.json]]

</a></span>

**Tryout solution but not work:**

I also tryout a solution to make keycloak load offline session from db when start an instance according to a comment from related research: <a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47008973317/App+flows+and+offline+sessions?focusedCommentId=47024964713" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47008973317/App+flows+and+offline+sessions?focusedCommentId=47024964713</a> but unfortunately also not work.
