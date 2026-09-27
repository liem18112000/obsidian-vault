---
ai_hash: 54042b6b5f1497e1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.81
entities: []
relevance: 0.755
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20436032657/DestroyRunningCases
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: DestroyRunningCases
topic: programming
type: source
updated: 2016-10-17
---

# DestroyRunningCases

> [!info] Imported from Confluence
> Space **LUZ** · updated 2016-10-17 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20436032657/DestroyRunningCases)
> Relevance 0.755 · topic `programming`

# Overview

This page is dedicated for ways to destroy all running cases on LUZ system (mostly to avoid performance hits).

As of now, an official ways to destroy all running cases on LUZ is via an unofficial RESTful API (only works with Axon.ivy 6.2.0 or later):

The source code of the project is at <a href="https://github.com/aavn-backyard/delete-all-cases" class="external-link" rel="nofollow">https://github.com/aavn-backyard/delete-all-cases</a>.

<div hasbody="true" macro-id="4fc12aa0-fd66-441e-a0e0-3099aa590d85" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

The authenticated account must have the role `DeleteAllCaseExecutor`. On several environments, the username is `janitor`.

</div>

</div>

 

# Usages

Since it's a RESTful API, so a HTTP client is enough ( `curl`, PostMan, etc).

### Count running cases

 

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9c3e676c-35b4-467d-867d-07bdda13b3f9" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
GET /ivy/api/{applicationName}/root/cases?running=true
Location: http://localhost:8081
 
 
or 
 
 
curl -i -u Developer:Developer http://localhost:8081/ivy/api/{applicationName}/root/cases?running=true
```

</div>

</div>

### Destroy all running cases

 

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="0c279aec-38e9-4920-86ec-7198113e801a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
POST /ivy/api/{applicationName}/root/cases/destroy
Location: http://localhost:8081
 
 
or 
 
 
curl -i -X POST -u Developer:Developer http://localhost:8081/ivy/api/{applicationName}/root/cases/destroy
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Empty Trash APIs]]
- [[Delete company - Old way]]
- [[Run Re-Index ivy database]]
- [[REST API for deleting EXPENSES documents]]
- [[Research on bulk removal of access class]]

%% ai-graph-end %%