---
ai_hash: 57f20b45bf25354d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.73
entities: []
relevance: 0.731
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47456322163/Catalog+for+3rd+party+system+API
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: Catalog for 3rd party system API
topic: programming
type: source
updated: 2023-08-21
---

# Catalog for 3rd party system API

> [!info] Imported from Confluence
> Space **LUZ** · updated 2023-08-21 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47456322163/Catalog+for+3rd+party+system+API)
> Relevance 0.731 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="5d5de170-4389-4d38-8dcc-42831389f4bf" macro-name="toc">

</div>

# Problem

Some KLARA modules consume API of external systems likes: eBill, Avaloq, Exchange Rate, Baumer,…

And, we need to show those relationships in KLARA Dev Portal by adding them to *consumesApis* list in our *catalog-info.yaml* file as below:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5d69c08c-dace-4997-b265-41f291042263" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
  consumesApis:
    - baumer    # The 3rd party system
    - jwt_service
    - luz_docs
```

</div>

</div>

But, KLARA Dev Portal does not show it in the Relations graph of the module. And, there is no API entity for it because they are not defined yet.

# Solution

In *catalog-info.yaml* of a KLARA module that has *consumesApis* relation to a 3rd party system, we can add an API entity to represents that 3rd party system as below:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="42567f99-1d52-466e-adb4-e13d209c5731" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
---
apiVersion: backstage.io/v1alpha1
kind: API
metadata:
  name: sample_name  #Your 3rd party API name, it be same value as in consumesApis
  description: |
    Please describe the 3rd party system
  tags:
    - 3rd-party
spec:
  type: RESTful API  # Do not use: openapi, asyncapi, grpc or graphql. It can be Native API, RESTful API, SOAP API,...
  lifecycle: production
  owner: group:default/sample_team  # Team owns this catalog-info.yaml
  definition: |
    Fill free to add a meaningful message here
```

</div>

</div>

# Example

See the 3rd party system: ‘BAUMER’ in luz_bauner: <a href="https://bitbucket.org/axonivy-prod/luz_baumer/src/master/catalog-info.yaml" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_baumer/src/master/catalog-info.yaml</a>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e73ff03e-78c1-416d-b04c-09f7f194996c" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: backstage.io/v1alpha1
kind: Component
metadata:
  name: luz_baumer
  description: |
    This is a module which provides APIs for sending physical document to Baumer print partner
  tags:
    - oneapi-physical-channel
    - java-8
    - microprofile-4-1
    - wildfly-26-build-1
    - tracing
  annotations:
    sonarqube.org/project-key: ch.klara.luz:luz_baumer
    'jenkins.io/job-full-name': KLARA/gcp-luz-baumer
spec:
  type: service
  owner: group:default/arrow
  lifecycle: production
  providesApis:
    - luz_baumer
  consumesApis:
    - BAUMER
    - jwt_service
    - luz_docs
 
  dependsOn:
    - component:default/luzsec_service
---
apiVersion: backstage.io/v1alpha1
kind: API
metadata:
  name: luz_baumer
  description: APIs for the luz_baumer
  tags:
    - rest
    - public
spec:
  type: openapi
  lifecycle: production
  owner: group:default/arrow
  definition:
    $text: 'http://localhost:7007/luz_baumer/api/openapi'
---
apiVersion: backstage.io/v1alpha1
kind: API
metadata:
  name: baumer
  description: This API represents the Baumer print partner system which is called by KLARA
  tags:
    - 3rd-party
spec:
  type: RESTful API
  lifecycle: production
  owner: group:default/arrow
  definition: |
    There is no API definition available
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[BlueZone, Public API, AI Data Feeds (07.11.2023 - 20.11.2023)]]
- [[Collect all calls FileManager APIs by Klara Modules]]
- [[LUZ-109076 Public API - Create update new tenant (implementation)]]
- [[One API Module Responsibilities]]
- [[Add new database module to deletion list]]

%% ai-graph-end %%