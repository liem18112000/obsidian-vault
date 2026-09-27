---
ai_hash: 07000d3ae94f8975
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 3
entities: []
relevance: 0.912
source: https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/47134410557/04_50_Setup+Swagger+UI+for+Finnova+Rest+API
space: GRAVITY
status: reference
tags:
- confluence
- programming
- space/gravity
title: 04_50_Setup Swagger UI for Finnova Rest API
topic: programming
type: source
updated: 2025-12-12
---

# 04_50_Setup Swagger UI for Finnova Rest API

> [!info] Imported from Confluence
> Space **GRAVITY** · updated 2025-12-12 · [open original](https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/47134410557/04_50_Setup+Swagger+UI+for+Finnova+Rest+API)
> Relevance 0.912 · topic `programming`

We have a page to download Finnova Rest API documentation in PDF file, it's well. But it's hard read and use as well.

One more point we should not use PDF documentation because we cannot directly interactive with service to have a quickly look a result from them.

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="e64dda54-dcc9-46ac-bcdc-1b6b9b68e0fc" macro-name="toc">

</div>

# Requirements

Node have to be pre-installed. I wont guide how to install it. This is out of scope of this guide so that please refers to Node.js <a href="https://nodejs.org/en/download/" class="external-link" rel="nofollow">installation guide</a>.

Git have to be pre-installed, the same with Node, please refers to Git <a href="https://git-scm.com/downloads" class="external-link" rel="nofollow">installation guide</a>.

# Setup

## Repair

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9fe5d9c5-390c-4302-ba14-0ddef7341785" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
mkdir ~/repos-git
```

</div>

</div>

## Setup Swagger

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5da3c132-29c3-4fea-a48d-c6b4f022ab49" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
git clone https://github.com/swagger-api/swagger-ui.git
git checkout branch v3.9.3
```

</div>

</div>

## Setup Static web server

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3c90cbaa-ef09-49e5-98f9-47b75146fe02" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
sudo npm install http-server -g
```

</div>

</div>

# Configuration

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="4e18dfc3-affb-4eb9-b41b-6bcd14033b7b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
cd /home/fintech/repos-git/swagger-ui/dist
http-server -p 65000 &
```

</div>

</div>

# Axon Environment

I deployed Finnova API to our delopment server (IP address: 192.168.80.27). We can access at: <a href="http://192.168.80.27:65000/#/" class="external-link" rel="nofollow">http://192.168.80.27:65000</a>

<div>

|            |                                    |
|------------|------------------------------------|
| **Params** | **Value**                          |
| app path   | /home/fintech/repos-git/swagger-ui |
| port       | 65000                              |

</div>

%% ai-graph-start %%

**Related notes:**
- [[Swagger UI]]
- [[Apply OpenAPI and try out API on SwaggerUI]]
- [[2.31 Build & deploy agent review service to k8s (POC)]]
- [[Microprofile OpenAPI config]]
- [[OpenAPI UI (API on SwaggerUI)]]

%% ai-graph-end %%