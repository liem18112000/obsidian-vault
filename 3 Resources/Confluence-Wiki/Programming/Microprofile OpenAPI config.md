---
ai_hash: 389c2b15a79b81b9
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 8
depth: 2.65
entities: []
relevance: 0.738
source: https://axonivy.atlassian.net/wiki/spaces/AVATAR/pages/47039512613/Microprofile+OpenAPI+config
space: AVATAR
status: reference
tags:
- confluence
- programming
- space/avatar
title: Microprofile/OpenAPI config
topic: programming
type: source
updated: 2022-02-16
---

# Microprofile/OpenAPI config

> [!info] Imported from Confluence
> Space **AVATAR** · updated 2022-02-16 · [open original](https://axonivy.atlassian.net/wiki/spaces/AVATAR/pages/47039512613/Microprofile+OpenAPI+config)
> Relevance 0.738 · topic `programming`

This document will guide you how to config and run open API

**1. Config OpenAPI for your project.** Example pull request: <a href="https://bitbucket.org/axonivy-prod/luz_docs_import/pull-requests/53/use-version-wildlfy-env" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_docs_import/pull-requests/53/use-version-wildlfy-env</a>

1.  **Add new dependency lib of openAPI**

2.  

![[47039512613-image-20220120-085419.png]]



    **Add path in the META-INF/microprofile-config.properties like picture below. Remember change path depends on your project**

3.  

![[47039512613-image-20220120-084736.png]]



    **Add @SecurityRequirement(name = "bearerAuth") for each resource in the project. This code will add the authorizations**

4.  

![[47039512613-image-20220120-085511.png]]



    **Add @SecurityScheme(securitySchemeName = "bearerAuth",bearerFormat = "JWT",type = SecuritySchemeType.HTTP, scheme = "bearer") to JAXRSConfiguration**


![[47039512613-image-20220124-033633.png]]



2\. **Run Open API in your local.**

You can download open API file for the current project that you are working on it and check the result before commit to server. If you want to use open API instead of postman, you have to commit code to master to run open API on master, because you have to have the authorizations. See the number 3 for more detail

1.  Port forward to project that you want to download open api file. Example project luz_article

2.  Open this link: <a href="http://localhost:8080/luz_article/api/openapi" class="external-link" rel="nofollow">http://localhost:8080/luz_article/api/openapi</a> You will receive a open api file like this:

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="5e2bf14d-100e-4a2f-875b-678c0ccf7333" macro-name="view-file"><a href="../_attachments/47039512613-luz_article_openAPI" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47039512613/luz_article_openAPI?version=1&amp;modificationDate=1643022400269&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/octet-stream" data-has-thumbnail="true">

![[47039512613-luz_article_openAPI]]

</a></span>

3\. Use <a href="https://editor.swagger.io/" class="external-link" data-card-appearance="inline" rel="nofollow">https://editor.swagger.io/</a> to see the result


![[47039512613-image-20220124-110720.png]]



3\. Run Open API on server

a\. Port forward to server:

*kubectl port-forward --address 0.0.0.0 services/api-forwarder 8080:8080 -n dev*

b\. Run this link

<a href="http://localhost:8080/luz-api-ui/" class="external-link" rel="nofollow">http://localhost:8080/luz-api-ui/</a>

c\. Pass the uri of project you want to be run open API like the picture


![[47039512613-image-20220216-041958.png]]



Here is video to try out

<a href="https://watch.screencastify.com/v/BJ95ZnnB86Iup5QaG80f" class="external-link" rel="nofollow">https://watch.screencastify.com/v/BJ95ZnnB86Iup5QaG80f</a>

%% ai-graph-start %%

**Related notes:**
- [[OpenAPI UI (API on SwaggerUI)]]
- [[Swagger UI]]
- [[Apply OpenAPI and try out API on SwaggerUI]]
- [[Swagger with api explorer]]
- [[Port forward and Docker compose]]

%% ai-graph-end %%