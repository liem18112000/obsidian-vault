---
ai_hash: e376248e75ab61c3
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 8
depth: 3
entities: []
relevance: 0.91
source: https://axonivy.atlassian.net/wiki/spaces/X4/pages/34384316650/import+invoices+via+python+script
space: X4
status: reference
tags:
- confluence
- programming
- space/x4
title: import invoices via python script
topic: programming
type: source
updated: 2025-01-15
---

# import invoices via python script

> [!info] Imported from Confluence
> Space **X4** · updated 2025-01-15 · [open original](https://axonivy.atlassian.net/wiki/spaces/X4/pages/34384316650/import+invoices+via+python+script)
> Relevance 0.91 · topic `programming`

## 

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="42dc1f5d-d7e8-4aac-9e72-342f649ff58f" macro-name="toc" numberedoutline="true">

</div>

# Intallation Guide

Install python 2.7 on host (windows, max, linux) <a href="https://www.python.org/downloads/release/python-2715/" class="external-link" rel="nofollow">https://www.python.org/downloads/release/python-2715/</a>

set environment variable where to find python executable: Most cases on windows hosts: C:\Python27

  

Extract following 7z file anywhere you want

[[34384316650-import-invoices.7z|import-invoices.7z]]

  

For  APF 4.6.11 and higher

TBD

# Run application

call first python main.py --help to get little description of the console application


![[34384316650-import-invoices-app.png]]



import invoice to xapf 4.0

run on the console python main.py (and optinal argues to override default values)

# Source Code:

<a href="https://bitbucket.org/soreco_prod/singleton-tools/src/master/python/tools/importInvoices/" class="external-link" rel="nofollow">https://bitbucket.org/soreco_prod/singleton-tools/src/master/python/tools/importInvoices/</a>

# Binary File Invoice Importer Tool

### v1.0.0

#### Windows

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="b25218ea-eae9-438a-8cc5-f5d82b0e1f2f" macro-name="view-file"><a href="../_attachments/34384316650-invoiceImporterTool.7z" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/34384316650/invoiceImporterTool.7z?version=1&amp;modificationDate=1561652212000&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/octet-stream" data-has-thumbnail="true">

![[34384316650-invoiceImporterTool.7z]]

</a></span>

  

### v1.1.0 for  APF 4.6.11 and higher

#### Linux

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="4e940e3c-fcb0-4d55-94b5-ec384b3fbbf5" macro-name="view-file"><a href="../_attachments/34384316650-import-invoice-tool-linux-1.1.0.tar.gz" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/34384316650/import-invoice-tool-linux-1.1.0.tar.gz?version=1&amp;modificationDate=1736938460589&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/x-gzip" data-has-thumbnail="true">

![[34384316650-import-invoice-tool-linux-1.1.0.tar.gz]]

</a></span>

#### Windows


![[34384316650-image2025-1-15_12-14-48.png]]



  

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="a61b5d66-958c-4f4e-b083-c2a9fba0e1c5" macro-name="view-file"><a href="../_attachments/34384316650-import-invoice-tool-win-amd64-3.10-v1.1.0.7z" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/34384316650/import-invoice-tool-win-amd64-3.10-v1.1.0.7z?version=1&amp;modificationDate=1736940024310&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/octet-stream" data-has-thumbnail="true">

![[34384316650-import-invoice-tool-win-amd64-3.10-v1.1.0.7z]]

</a></span>

# Generate MassImport

<a href="https://axonivy.atlassian.net/wiki/people/60d0185600bdd900687e2cb5?ref=confluence" class="confluence-userlink user-mention" data-account-id="60d0185600bdd900687e2cb5" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Cyrill Naef</a> create a batch-file to generate lot's of imports. It uses the binary \*.exe file

On SE Host it is located here: C:\XAPF_Pgm_T\invoiceImporterTool\\


![[34384316650-image2024-7-24_11-15-17.png]]



  

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="96d04469-034d-4909-82fe-07e14e74e997" macro-name="view-file"><a href="../_attachments/34384316650-MassenTest.bat" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/34384316650/MassenTest.bat?version=1&amp;modificationDate=1721812373989&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/octet-stream" data-has-thumbnail="true">

![[34384316650-MassenTest.bat]]

</a></span>

%% ai-graph-start %%

**Related notes:**
- [[Invoice API Java Client]]
- [[APF ItemLine Import API]]
- [[APF Provided Bookings]]
- [[OCR Command Line Interface]]
- [[Generic Interface JSON file]]

%% ai-graph-end %%