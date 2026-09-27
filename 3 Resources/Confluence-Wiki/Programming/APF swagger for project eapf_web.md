---
ai_hash: 11d40f0f68ec1ee4
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 11
depth: 2.92
entities: []
relevance: 0.823
source: https://axonivy.atlassian.net/wiki/spaces/X4/pages/34422813508/APF+swagger+for+project+eapf_web
space: X4
status: reference
tags:
- confluence
- programming
- space/x4
title: APF swagger for project eapf_web
topic: programming
type: source
updated: 2020-11-18
---

# APF swagger for project eapf_web

> [!info] Imported from Confluence
> Space **X4** · updated 2020-11-18 · [open original](https://axonivy.atlassian.net/wiki/spaces/X4/pages/34422813508/APF+swagger+for+project+eapf_web)
> Relevance 0.823 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="db8c6298-8c42-4546-8525-1f4cb4f93948" macro-name="toc">

</div>

## Introduction

With story <a href="https://axonivy.atlassian.net/browse/AD-2669" class="external-link" rel="nofollow">AD-2669</a> your are able to build swagger.yaml for the posting the rest API content it to the swagger editor in cloud. This helps for test programmers to create automated tests.

  

## Implementation

We introduce new build properties like the *ivy.base.path* for configuring the relative resource url and the base url with property *ivy.host* and a new maven plugin to generate the swagger content file.*  *

<span class="legacy-color-text-red2">Remark: the swagger access does only work with LTS axon ivy engine version v7.0.13 and higher</span>

  

We use the maven plugin the crate the swagger.yaml file in the building process

You can modify these properties in the build command like follows:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="182b3556-a636-4895-8349-14128b0646b7" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
mvn resources:resources post-clean install -DskipTests=true -DskipITs=true -DskipUTs=true -U -Divy.host=localhost:8081 -Divy.base.path=/ivy/api 
```

</div>

</div>

  

After the building you see the produces swagger.yaml file in folder eapf_web/webContent/generated

You can dowloand the swagger file also from our <a href="http://35.158.101.69/jenkins" class="external-link" rel="nofollow">swiss soad jenkins</a>. Choose latest build (after ticket is merged into master branch) and click on the *Worspace* menu item on the left. Navigate into folder as mentioned above.


![[34422813508-image2020-11-6_15-21-12.png]]



After downloading the swagger file, you have to replace occurencies of {appicationName} by the application name of ivy instance name you want the access to the REST API. It depends also from your installation. On the swiss demo server our application name of the second ivy instance is *xapf_dev_7013.*

<span class="legacy-color-text-red2">Remark: It only works with *localhost* host name.</span>

You get access to the rest API by posting swagger.yml content into

<a href="https://editor.swagger.io/" class="external-link" rel="nofollow">https://editor.swagger.io/</a>

Post following  swagger.yaml into editor field left black field

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="07d517be-a03c-49c7-9d18-513d234f6d26" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">

**swagger.yaml for the eapf_web ivy application**<span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>

</div>

<div class="codeContent panelContent pdl hide-toolbar">

``` syntaxhighlighter-pre
---
swagger: "2.0"
info:
  description: "The digital and the more efficient resolution"
  version: "v1"
  title: "RESTFul API of Xpert.APF"
  contact:
    name: "adeon soreco work flow"
    url: "https://www.xpertapf.ch"
host: "localhost:38080"
basePath: "/ivy/api"
tags:
- name: "project"
  description: "get project meta data"
- name: "system"
  description: "get configured menu entries for user"
- name: "test"
  description: "test workflow service"
- name: "user"
  description: "test login and logout"
schemes:
- "http"
- "https"
paths:
  /xapf_dev_7013/getmenu:
    get:
      tags:
      - "system"
      summary: "getmenu"
      description: ""
      operationId: "getMenu"
      produces:
      - "application/json"
      parameters:
      - name: "userName"
        in: "query"
        description: "username"
        required: false
        type: "string"
      - name: "languageCode"
        in: "query"
        description: "language code like en, de or fr."
        required: false
        type: "string"
      responses:
        200:
          description: "return model of menues"
          schema:
            $ref: "#/definitions/MenuModel"
        400:
          description: "Bad request for getting menu"
  /xapf_dev_7013/project/metadata:
    get:
      tags:
      - "project"
      summary: "metadata"
      description: ""
      operationId: "getProjectMetaData"
      produces:
      - "application/json"
      parameters: []
      responses:
        200:
          description: "returns project meta data"
          schema:
            $ref: "#/definitions/ProjectMetadata"
        500:
          description: "general internal server error"
  /xapf_dev_7013/test/config/workflow/foureyesprinciple:
    post:
      tags:
      - "test"
      summary: "four eyes principle feature enable"
      description: ""
      operationId: "fourEyesPrinciple"
      produces:
      - "application/json"
      parameters:
      - name: "enable"
        in: "query"
        required: false
        type: "string"
      - name: "itemHeadId"
        in: "query"
        required: false
        type: "string"
      - name: "processStepName"
        in: "query"
        required: false
        type: "string"
      responses:
        200:
          description: "enable disable of four eyes principle succeeded"
        304:
          description: "four eyes principle feature configuration not modified"
        404:
          description: "http method or process step not found"
        406:
          description: "bad request"
        500:
          description: "general server error"
  /xapf_dev_7013/test/config/workflow/foureyesprinciple/role:
    post:
      tags:
      - "test"
      summary: "feature overflow role enable or disable"
      description: ""
      operationId: "fourEyesPrincipleRole"
      produces:
      - "application/json"
      parameters:
      - name: "enable"
        in: "query"
        description: "enable"
        required: true
        type: "string"
      - name: "itemHeadId"
        in: "query"
        description: "itemHeadId"
        required: true
        type: "string"
      - name: "processStepName"
        in: "query"
        description: "processStepName"
        required: true
        type: "string"
      responses:
        200:
          description: "enable or disable of the four eyes principle overflow feature\
            \ succeeded"
        304:
          description: "four eyes principle feature configuration not modified"
        404:
          description: "http method or process step not found"
        406:
          description: "bad request"
        500:
          description: "general server error"
  /xapf_dev_7013/test/config/workflow/foureyesprinciple/role/name:
    get:
      tags:
      - "test"
      summary: "/foureyesprinciple/role/name"
      description: ""
      operationId: "getFourEyesPrincipleExceptionRole"
      produces:
      - "application/json"
      parameters:
      - name: "itemHeadId"
        in: "query"
        description: "itemHeadId"
        required: true
        type: "string"
      - name: "processStepName"
        in: "query"
        description: "processStepName"
        required: true
        type: "string"
      responses:
        200:
          description: "get current exception role succeeded"
        404:
          description: "Http method or process step not found"
        500:
          description: "general server error"
    post:
      tags:
      - "test"
      summary: "/foureyesprinciple/role/name"
      description: ""
      operationId: "updateFourEyesPrincipleRole"
      produces:
      - "application/json"
      parameters:
      - name: "role"
        in: "query"
        description: "role"
        required: true
        type: "string"
      - name: "itemHeadId"
        in: "query"
        description: "itemHeadId"
        required: true
        type: "string"
      - name: "processStepName"
        in: "query"
        description: "processStepName"
        required: true
        type: "string"
      responses:
        200:
          description: "update of exception role succeeded"
        304:
          description: "exception role not modified!"
        404:
          description: "http method, process step or rolename not found"
        500:
          description: "general server error"
  /xapf_dev_7013/test/config/workflow/foureyesprinciple/role/state:
    get:
      tags:
      - "test"
      summary: "is four eyes principle role overflow feature enabled?"
      description: ""
      operationId: "getfourEyesPrincipleRoleOveflowConfiguration"
      produces:
      - "application/json"
      parameters:
      - name: "itemHeadId"
        in: "query"
        required: false
        type: "string"
      - name: "processStepName"
        in: "query"
        required: false
        type: "string"
      responses:
        200:
          description: "enable disable of four eyes principle succeeded"
        304:
          description: "four eyes principle feature configuration not modified"
        404:
          description: "http method or process step not found"
        406:
          description: "bad request"
        500:
          description: "general server error"
  /xapf_dev_7013/test/config/workflow/foureyesprinciple/state:
    get:
      tags:
      - "test"
      summary: "is four eyes principle feature enabled?"
      description: ""
      operationId: "getFourEyesPrincipleConfiguration"
      produces:
      - "application/json"
      parameters:
      - name: "itemHeadId"
        in: "query"
        required: false
        type: "string"
      - name: "processStepName"
        in: "query"
        required: false
        type: "string"
      responses:
        200:
          description: "enable disable of four eyes principle succeeded"
        304:
          description: "four eyes principle feature configuration not modified"
        404:
          description: "http method or process step not found"
        406:
          description: "bad request"
        500:
          description: "general server error"
  /xapf_dev_7013/test/itemline/add:
    post:
      tags:
      - "test"
      summary: "add"
      description: ""
      operationId: "addItemLine"
      produces:
      - "application/json"
      parameters:
      - name: "itemHeadId"
        in: "query"
        description: "itemHeadId"
        required: true
        type: "string"
      responses:
        200:
          description: "successful add item line"
  /xapf_dev_7013/test/task/finish:
    post:
      tags:
      - "test"
      summary: "finish"
      description: ""
      operationId: "finishTask"
      produces:
      - "application/json"
      parameters:
      - name: "barcode"
        in: "query"
        description: "barcode"
        required: true
        type: "string"
      responses:
        200:
          description: "successful finish task"
        400:
          description: "bad request"
        500:
          description: "internal general server error"
  /xapf_dev_7013/test/task/goback:
    post:
      tags:
      - "test"
      summary: "goback"
      description: ""
      operationId: "goBackToTaskList"
      produces:
      - "application/json"
      parameters:
      - name: "barcode"
        in: "query"
        description: "barcode"
        required: true
        type: "string"
      - name: "saveitemhead"
        in: "query"
        description: "saveitemhead"
        required: true
        type: "boolean"
      responses:
        200:
          description: "successful go back to task list"
        400:
          description: "bad request"
        404:
          description: "method not found"
        500:
          description: "general server error"
  /xapf_dev_7013/test/task/open:
    post:
      tags:
      - "test"
      summary: "open"
      description: ""
      operationId: "openTask"
      produces:
      - "application/json"
      parameters:
      - name: "barcode"
        in: "query"
        description: "barcode"
        required: true
        type: "string"
      - name: "username"
        in: "query"
        description: "username"
        required: true
        type: "string"
      responses:
        200:
          description: "successful open task"
        406:
          description: "bad request"
  /xapf_dev_7013/user/login:
    post:
      tags:
      - "user"
      summary: "login"
      description: ""
      operationId: "login"
      produces:
      - "application/json"
      parameters:
      - name: "user"
        in: "query"
        description: "username"
        required: true
        type: "string"
      - name: "password"
        in: "query"
        description: "password"
        required: true
        type: "string"
      - name: "language"
        in: "query"
        required: false
        type: "string"
      responses:
        200:
          description: "successful login"
        400:
          description: "bad request"
        404:
          description: "requested user not found on ivy system"
        406:
          description: "not acceptable request"
        500:
          description: "general server error"
  /xapf_dev_7013/user/logout:
    post:
      tags:
      - "user"
      summary: "logout"
      description: ""
      operationId: "logout"
      produces:
      - "application/json"
      parameters: []
      responses:
        200:
          description: "successful logout"
        400:
          description: "bad request means user is not logged in"
        406:
          description: "not acceptable"
securityDefinitions:
  basicAuth:
    type: "basic"
definitions:
  MenuModel:
    type: "object"
    properties:
      order:
        type: "integer"
        format: "int32"
      icon:
        type: "string"
      link:
        type: "string"
      menuName:
        type: "string"
      children:
        type: "array"
        items:
          $ref: "#/definitions/MenuModel"
  ProjectMetadata:
    type: "object"
    properties:
      releasedVersion:
        type: "string"
      gitBranch:
        type: "string"
      buildTimeStamp:
        type: "string"
      runningOsOfApplication:
        type: "string"
```

</div>

</div>


![[34422813508-image2020-11-6_15-29-34.png]]



## Work around to use swagger UI also with axon ivy engine v7.0.6

If you install in the the firefox browser the add-In plugin 'CORS everywhere' you can use the swagger also with version v7.0.6


![[34422813508-image2020-11-13_15-49-30.png]]



Enable Cors Everywhere like following picture


![[34422813508-image2020-11-13_15-50-22.png]]



if you want to get access from another WS in your network, please disable connection security in your browser


![[34422813508-image2020-11-18_8-32-30.png]]




![[34422813508-image2020-11-13_16-12-44.png]]




![[34422813508-image2020-11-13_16-13-3.png]]



Following script works for the swiss demo server

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e7a8bd13-e447-4690-a384-1fc114f88c3c" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">

**swagger.yaml currently works on swiss demo server**<span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>

</div>

<div class="codeContent panelContent pdl hide-toolbar">

``` syntaxhighlighter-pre
---
swagger: "2.0"
info:
  description: "The digital and the more efficient resolution"
  version: "v1"
  title: "RESTFul API of Xpert.APF"
  contact:
    name: "adeon soreco work flow"
    url: "https://www.xpertapf.ch"
host: "35.158.101.69"
basePath: "/ivy/api"
tags:
- name: "project"
  description: "get project meta data"
- name: "system"
  description: "get configured menu entries for user"
- name: "test"
  description: "test workflow service"
- name: "user"
  description: "test login and logout"
schemes:
- "http"
- "https"
paths:
  /xapf_dev/getmenu:
    get:
      tags:
      - "system"
      summary: "getmenu"
      description: ""
      operationId: "getMenu"
      produces:
      - "application/json"
      parameters:
      - name: "userName"
        in: "query"
        description: "username"
        required: false
        type: "string"
      - name: "languageCode"
        in: "query"
        description: "language code like en, de or fr."
        required: false
        type: "string"
      responses:
        200:
          description: "return model of menues"
          schema:
            $ref: "#/definitions/MenuModel"
        400:
          description: "Bad request for getting menu"
  /xapf_dev/project-meta-data:
    get:
      tags:
      - "project"
      summary: "metadata"
      description: ""
      operationId: "getProjectMetaData"
      produces:
      - "application/json"
      parameters: []
      responses:
        200:
          description: "returns project meta data"
          schema:
            $ref: "#/definitions/ProjectMetadata"
        500:
          description: "general internal server error"
securityDefinitions:
  basicAuth:
    type: "basic"
definitions:
  MenuModel:
    type: "object"
    properties:
      order:
        type: "integer"
        format: "int32"
      icon:
        type: "string"
      link:
        type: "string"
      menuName:
        type: "string"
      children:
        type: "array"
        items:
          $ref: "#/definitions/MenuModel"
  ProjectMetadata:
    type: "object"
    properties:
      releasedVersion:
        type: "string"
      gitBranch:
        type: "string"
      buildTimeStamp:
        type: "string"
      runningOsOfApplication:
        type: "string"
```

</div>

</div>

<span class="legacy-color-text-red2">Remarks:</span>

- <span class="legacy-color-text-red2">Resource URL with AD-2669 included: /xapf_dev/project/metadata: </span>
- <span class="legacy-color-text-red2">Resource URL without AD-2669 included:  /xapf_dev/project-meta-data:</span>

%% ai-graph-start %%

**Related notes:**
- [[APF archiving via d3 Rest interface]]
- [[Microprofile OpenAPI config]]
- [[APF Provided Bookings]]
- [[REST API for deleting EXPENSES documents]]
- [[Swagger UI]]

%% ai-graph-end %%