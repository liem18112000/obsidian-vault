---
ai_hash: 4645fb5817c60a2b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 3
depth: 2.81
entities: []
relevance: 0.724
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20436759448/Generating+document+from+viewgen
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: Generating document from viewgen
topic: programming
type: source
updated: 2017-01-24
---

# Generating document from viewgen

> [!info] Imported from Confluence
> Space **LUZ** · updated 2017-01-24 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20436759448/Generating+document+from+viewgen)
> Relevance 0.724 · topic `programming`

# Source code on Bitbucket 

<a href="https://bitbucket.org/axonivy-prod/luz_viewgen" class="external-link" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_viewgen</a>

# Resources

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1ab18fbb-373c-4c11-80a9-e8def9dd3fbe" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
/luz_viewgen/api/insurance-reports/{domain}
method: POST
body: xml string
content-type: application/xml
accept: application/pdf
```

</div>

</div>

<a href="../_attachments/20436759448-salary_declaration.txt" data-nice-type="Text File">Download xml</a>

# List of reports

<div>

|                    |             |
|--------------------|-------------|
| Report name        | Domain code |
| Ahv Free Report    | AHV-FREE    |
| Ahv Report         | AHV-AVS     |
| Fak report         | FAK-CAF     |
| Ktg report         | KTG-AMC     |
| Uvg report         | UVG-LAA     |
| Uvgz report        | UVGZ-LAAC   |
| Statistic Report   | Statistic   |
| Tax account report | Tax         |

</div>

# Some generated documents

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="d408499e-cfbd-4b01-9001-1c4dc3c893ff" macro-name="view-file"><a href="../_attachments/20436759448-FAK-CAF.pdf" class="confluence-embedded-file" data-nice-type="PDF Document" data-file-src="/wiki/download/attachments/20436759448/FAK-CAF.pdf?version=1&amp;modificationDate=1485229409000&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/pdf" data-has-thumbnail="true">

![[20436759448-FAK-CAF.pdf]]

</a></span><span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="73f0665c-8ff7-4aff-b171-018a09a12f69" macro-name="view-file"><a href="../_attachments/20436759448-AHV-AVS.pdf" class="confluence-embedded-file" data-nice-type="PDF Document" data-file-src="/wiki/download/attachments/20436759448/AHV-AVS.pdf?version=1&amp;modificationDate=1485229412000&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/pdf" data-has-thumbnail="true">

![[20436759448-AHV-AVS.pdf]]

</a></span>

%% ai-graph-start %%

**Related notes:**
- [[Document Creator API]]
- [[Employee Report Implementation (10.05.2023)]]
- [[Research Design architecture concept for the service to generate the Generic Interface File]]
- [[Programming]]
- [[Invoice API Java Client]]

%% ai-graph-end %%