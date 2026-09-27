---
ai_hash: da2705b2e3eaf854
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 5
depth: 3
entities: []
relevance: 0.835
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/46985019868/Analyze+API+Demo
space: AI
status: reference
tags:
- confluence
- programming
- space/ai
title: Analyze API Demo
topic: programming
type: source
updated: 2023-12-01
---

# Analyze API Demo

> [!info] Imported from Confluence
> Space **AI** · updated 2023-12-01 · [open original](https://axonivy.atlassian.net/wiki/spaces/AI/pages/46985019868/Analyze+API+Demo)
> Relevance 0.835 · topic `programming`

## Test it

The purpose of the Analyze API Demo web application is to demonstrate document analysis features. Find the web application at the following location (requires VPN connection to KLARA GCP ops nonprod):

<div>

|  |
|----|
| <a href="http://analyze-demo.private.klara.tech/ui" class="external-link" rel="nofollow">http://analyze-demo.private.klara.tech/ui</a> |

</div>

## Table of Contents

<div class="toc-macro client-side-toc-macro conf-macro output-block" excludeheaderregex="Test it|Table of Contents" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="da077b96572fa1fafc4c5705e252c1af" macro-name="toc">

</div>

## How to use it?

### 1. Open the app

Establish a VPN connection to KLARA GCP ops nonprod. Then open your favorite web browser and navigate to <a href="http://analyze-demo.private.klara.tech/ui" class="external-link" rel="nofollow">http://analyze-demo.private.klara.tech/ui</a>.


![[46985019868-Screenshot 2021-10-19 at 16.00.29.png]]



### 2. Upload a document

Either press the `Upload File...` button or drag & drop your file to the mentioned place. Document analysis will automatically start. When document analysis completed you can use the tabs on the right hand side to see a selected set of different analysis results.


![[46985019868-Screenshot 2021-10-19 at 16.02.39.png]]



Just upload another document for analysis if you like.

### 3. Bookmark or share analysis results

Use the URL in the browser’s address bar to share the results of document analysis with someone else.

### 4. Test new document analysis features

Go to `Settings` via the menu button on the upper left corner and change analysis behavior. Afterwards navigate back to your document, select the `Analyze` tab and `Run Analysis` again.


![[46985019868-Screenshot 2021-10-19 at 16.20.41.png]]



## Known issues

- You can only upload one single document at the time

## Sample document

Feel free to use the following file:

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="ba449fc3-ca94-4d4a-b922-b387540f49ca" macro-name="view-file"><a href="../_attachments/46985019868-Sample-Invoice.pdf" class="confluence-embedded-file" data-nice-type="PDF Document" data-file-src="/wiki/download/attachments/46985019868/Sample-Invoice.pdf?version=1&amp;modificationDate=1634653613278&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/pdf" data-has-thumbnail="true">

![[46985019868-Sample-Invoice.pdf]]

</a></span>

%% ai-graph-start %%

**Related notes:**
- [[Analyze API]]
- [[Analyze API v2 for Invoice prediction]]
- [[Invoice API]]
- [[Analyze API v2.0]]
- [[Invoice API Reference]]

%% ai-graph-end %%