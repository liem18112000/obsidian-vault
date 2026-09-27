---
ai_hash: 91fc19bb4120985d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 3
depth: 2.33
entities: []
relevance: 0.711
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/47401730164/Analyze+API+v2.0
space: AI
status: reference
tags:
- confluence
- programming
- space/ai
title: Analyze API v2.0
topic: programming
type: source
updated: 2023-11-30
---

# Analyze API v2.0

> [!info] Imported from Confluence
> Space **AI** · updated 2023-11-30 · [open original](https://axonivy.atlassian.net/wiki/spaces/AI/pages/47401730164/Analyze+API+v2.0)
> Relevance 0.711 · topic `programming`

<div class="plugin-tabmeta-details conf-macro output-block" hasbody="true" macro-id="7571bf1b-40ec-4940-9d5e-7956b17cae78" macro-name="details">

<div>

|  |  |
|----|----|
| Version | 2.0 |
| Status | <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" hasbody="false" macro-id="b4ad811d-2b56-4cc1-9d52-d098bb37e3bd" macro-name="status">RELEASED</span> |
| Release date | August 3, 2023 |
| Link | <a href="https://axonivy.atlassian.net/projects/AI/versions/14478" class="external-link" rel="nofollow">Jira</a> |

</div>

</div>

## Table of Contents

<div class="toc-macro client-side-toc-macro conf-macro output-block" excludeheaderregex="Table of Contents" hasbody="false" headerelements="H1,H2,H3" macro-id="aa85f563-d11b-4113-ae78-1346322398da" macro-name="toc">

</div>

## New and Noteworthy

Analyze API v2.0 is a slightly improved version of Analyze API v1.x that runs on GCP.

As soon as you’ve established the Private Service Connect, you can access the API at `http://<host>:80/api/v2/` or the OpenAPI document at `http://<host>:80/api/v2/openapi.json`.

### API Endpoint

Analyze API v2.0 is not anymore public available, but through <a href="https://cloud.google.com/vpc/docs/private-service-connect" class="external-link" rel="nofollow">Private Service Connect</a>. Note that an **approval** is required in order to establish a connection the first time.

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Stage</strong></p></th>
<th><p><strong>Service Attachment URL</strong></p></th>
</tr>
&#10;<tr>
<td><p>DEV</p></td>
<td><p>projects/klara-ai-dev-01/regions/europe-west6/serviceAttachments/k8s1-sa-9jr5j0ht-analyze-sa-analyze-jujkojuc</p></td>
</tr>
<tr>
<td><p>TEST</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks"><a href="https://axonivy.atlassian.net/wiki/people/557058:45079719-85c5-4946-a074-88c2de9ba725?ref=confluence" class="confluence-userlink user-mention" data-account-id="557058:45079719-85c5-4946-a074-88c2de9ba725" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Tobias Hofer</a> Publish Service Attachment URL for Analyze API v2 on TEST</span></li>
</ul></td>
</tr>
<tr>
<td><p>PROD</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks"><a href="https://axonivy.atlassian.net/wiki/people/557058:45079719-85c5-4946-a074-88c2de9ba725?ref=confluence" class="confluence-userlink user-mention" data-account-id="557058:45079719-85c5-4946-a074-88c2de9ba725" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Tobias Hofer</a> Publish Service Attachment URL for Analyze API v2 on PROD</span></li>
</ul></td>
</tr>
</tbody>
</table>

</div>

### Authentication

Analyze API v2.0 has no proprietary authentication flow anymore. Currently, not even an authentication token is required anymore.

An owner is required as part of the API request’s path. Quotas apply to an owner. Please talk with <a href="https://axonivy.atlassian.net/wiki/people/557058:45079719-85c5-4946-a074-88c2de9ba725?ref=confluence" class="confluence-userlink user-mention" data-account-id="557058:45079719-85c5-4946-a074-88c2de9ba725" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Tobias Hofer</a> to select the correct owner.

### KlaraAccgBct and KlaraAccgTag predictions

The `KlaraAccgBct` and `KlaraAccgTag` labels are no longer part of the `Invoice.Predictions` entity (as documented here [Invoice API Labels](https://axonivy.atlassian.net/wiki/spaces/AI/pages/2488076280/Invoice+API+Labels)). Those labels are now served as separate entities named `Invoice.AccgBct` and `Invoice.AccgTag`.

## JIRA Release Notes

| Key | Summary | Type | Status | Key |
|----|----|----|----|----|
|  | Migrate Tag and BCT predictors | 

![[47401730164-10313.png]]

 | Done | [AI-1165](https://axonivy.atlassian.net/browse/AI-1165) |
|  | Migrate UID Admin lookups | 

![[47401730164-10313.png]]

 | Done | [AI-1166](https://axonivy.atlassian.net/browse/AI-1166) |
|  | Run Analyze API on klara-ai-test-01 | 

![[47401730164-story.svg]]

 | Done | [AI-1197](https://axonivy.atlassian.net/browse/AI-1197) |
|  | Migrate Analyze API to Kubernetes | 

![[47401730164-story.svg]]

 | Done | [AI-1091](https://axonivy.atlassian.net/browse/AI-1091) |
|  | Migrate Document Type detection | 

![[47401730164-10313.png]]

 | Done | [AI-1176](https://axonivy.atlassian.net/browse/AI-1176) |
|  | A lot of JobHandler not being closed properly | 

![[47401730164-10303.png]]

 | Done | [AI-1186](https://axonivy.atlassian.net/browse/AI-1186) |
|  | Migrate Invoice processing unit | 

![[47401730164-10313.png]]

 | Done | [AI-1160](https://axonivy.atlassian.net/browse/AI-1160) |
|  | Support Kubernetes ConfigMaps for Analyze API Quota | 

![[47401730164-story.svg]]

 | Done | [AI-1150](https://axonivy.atlassian.net/browse/AI-1150) |
|  | Migrate Content Type detection | 

![[47401730164-10313.png]]

 | Done | [AI-1157](https://axonivy.atlassian.net/browse/AI-1157) |
|  | Migrate Data Access Layer | 

![[47401730164-10313.png]]

 | Done | [AI-1146](https://axonivy.atlassian.net/browse/AI-1146) |
|  | Migrate plain Web Services | 

![[47401730164-10313.png]]

 | Done | [AI-1092](https://axonivy.atlassian.net/browse/AI-1092) |
|  | Provide Ignite Backend | 

![[47401730164-10313.png]]

 | Done | [AI-1093](https://axonivy.atlassian.net/browse/AI-1093) |
|  | Migrate Facade without Data Access Layer | 

![[47401730164-10313.png]]

 | Done | [AI-1145](https://axonivy.atlassian.net/browse/AI-1145) |
|  | Migrate OCR processing unit | 

![[47401730164-10313.png]]

 | Done | [AI-1156](https://axonivy.atlassian.net/browse/AI-1156) |
|  | Migrate Job Manager | 

![[47401730164-10313.png]]

 | Done | [AI-1154](https://axonivy.atlassian.net/browse/AI-1154) |
|  | Send metrics to data lake | 

![[47401730164-10313.png]]

 | Done | [AI-1161](https://axonivy.atlassian.net/browse/AI-1161) |
|  | Migrate BasicPmtInfo processing | 

![[47401730164-10313.png]]

 | Done | [AI-1137](https://axonivy.atlassian.net/browse/AI-1137) |
|  | Migrate and run Load Tests | 

![[47401730164-10313.png]]

 | Done | [AI-1158](https://axonivy.atlassian.net/browse/AI-1158) |
|  | Migrate Event Service | 

![[47401730164-10313.png]]

 | Done | [AI-1152](https://axonivy.atlassian.net/browse/AI-1152) |
|  | Use dedicated node pool | 

![[47401730164-10313.png]]

 | Done | [AI-1162](https://axonivy.atlassian.net/browse/AI-1162) |
|  | Migrate Image, PDF and Tika processing units | 

![[47401730164-10313.png]]

 | Done | [AI-1163](https://axonivy.atlassian.net/browse/AI-1163) |
|  | Migrate Facade with Data Access Layer | 

![[47401730164-10313.png]]

 | Done | [AI-1149](https://axonivy.atlassian.net/browse/AI-1149) |
|  | Migrate Quota Service | 

![[47401730164-10313.png]]

 | Done | [AI-1153](https://axonivy.atlassian.net/browse/AI-1153) |
|  | No readable string representation of SwissQrBillAnnotator | 

![[47401730164-10303.png]]

 | Done | [AI-811](https://axonivy.atlassian.net/browse/AI-811) |

## Known Issues

| Priority | Issuekey | Summary | Status | Key |
|----------|----------|---------|--------|-----|

%% ai-graph-start %%

**Related notes:**
- [[Analyze API]]
- [[Analyze API v2 for Invoice prediction]]
- [[Invoice API]]
- [[Analyze API Demo]]
- [[SchemaRegistry API v2.0]]

%% ai-graph-end %%