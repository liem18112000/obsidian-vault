---
title: "OpenAPI UI (API on SwaggerUI)"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47137653591/OpenAPI+UI+API+on+SwaggerUI
space: "LUZ"
topic: programming
relevance: 0.886
depth: 3
updated: 2022-06-29
attachments: 6
tags:
  - confluence
  - programming
  - space/luz
---

# OpenAPI UI (API on SwaggerUI)

> [!info] Imported from Confluence
> Space **LUZ** · updated 2022-06-29 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47137653591/OpenAPI+UI+API+on+SwaggerUI)
> Relevance 0.886 · topic `programming`

Guide how to run Open API UI on local for modules which have applied OpenAPI (see <a href="https://axonivy.atlassian.net/l/c/yH6rkqwB" class="external-link" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/l/c/yH6rkqwB</a> for status). For modules haven’t moved to Open API, guide at [Swagger UI](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20474827711/Swagger+UI)

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="eb21d08f-0201-4196-84aa-cadf88386cdc" macro-name="toc">

</div>

## Steps to run APIs on SwaggerUI (using port-forward)

1.  Start SwaggerUI on browser

    1.  Port-forward luz-api-ui : *kubectl port-forward service/luz-api-ui 8082:8080 -n dev-vn*

    2.  Open on browser: <a href="http://localhost:8082/luz-api-ui/" class="external-link" rel="nofollow">http://localhost:8082/luz-api-ui/</a>

2.  Access APIs of module

    1.  port-forward the module you want to test APIs. *E.g: kubectl port-forward service/luz-store 8081:8080 -n dev-vn*

    2.  Input OpenAPI url. E.g: <a href="http://localhost:8081/luz_store/api/openapi" class="external-link" rel="nofollow">http://localhost:8081/luz_store/api/openapi</a>


![[47137653591-api.PNG]]



3\. Authorize to access APIs

a\. port-forward jwt-service to generate token. *E.g: kubectl port-forward service/jwt-service 8086:8080 -n dev-vn*

b\. Input generic token to authorize


![[47137653591-authorize.PNG]]



4\. Now you can try out the APIs. E.g:


![[47137653591-tryout.PNG]]



**Notes**: If you got error ‘blocked by CORS', you can follow below steps or you can google 'how to bypass cors policy in chrome’ to find another way to solve issue


![[47137653591-Blocked by CORS.PNG]]



1.  Turn off the browser

2.  Disable web security: Add this command on the target of the browser shortcut: (E.g: Chrome)  
    --disable-web-security --disable-gpu --user-data-dir=~/chromeTemp


![[47137653591-bypass.PNG]]



## Steps to run APIs on SwaggerUI (using VPN)

This section describes how to use the Swagger UI on DEV with a VPN connection.

1.  Start the VPN connection (<a href="https://axonivy.atlassian.net/l/c/Bu5WgJNg" class="external-link" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/l/c/Bu5WgJNg</a> )

2.  Open <a href="http://internal-api.dev.private.klara.tech/luz-api-ui" class="external-link" rel="nofollow">http://internal-api.dev.private.klara.tech/luz-api-ui</a> in a browser

3.  Browse to the API you’d like to work with, e.g. <a href="http://internal-api.dev.private.klara.tech/luz_store/api/openapi" class="external-link" rel="nofollow">http://internal-api.dev.private.klara.tech/luz_store/api/openapi</a> for `luz_store`  

    

![[47137653591-screenshot 2022-06-29 um 15.17.58.png]]



4.  Authorize to access APIs. You need to create a generic or a tenant-specific token, e.g. with Postman or with the following command (replace `${username}` with your username, `${password}` with the password and `${tenantid}` with the tenant ID):  
    `curl --location --request POST -u "${username}:${password}" "http://internal-api.dev.private.klara.tech/luzsec/api/${tenantid}/tokens"`  
    Once you have a token, use it to authorize in the Swagger UI:

    

![[47137653591-authorize.PNG]]



5.  Now you’re ready to go and test the APIs.
