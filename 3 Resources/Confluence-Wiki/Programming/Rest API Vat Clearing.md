---
ai_hash: 6e60232de488cde1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 3
entities: []
relevance: 0.934
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20463007053/Rest+API+Vat+Clearing
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: Rest API Vat Clearing
topic: programming
type: source
updated: 2018-04-23
---

# Rest API Vat Clearing

> [!info] Imported from Confluence
> Space **LUZ** · updated 2018-04-23 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20463007053/Rest+API+Vat+Clearing)
> Relevance 0.934 · topic `programming`

~~**REQUIRE:  **~~

- ~~Initialize MasterData for CodeBoxDefinition: see [Luz_accounting - Initialize Master Data](https://axonivy.atlassian.net/wiki/spaces/LUZFIN/pages/20952599270/Luz_accounting+-+Initialize+Master+Data)~~

**REST API:**

- Get Vat Clearing by quarter of year  
  Methods: **POST** <span class="legacy-color-text-blue1">/luz_accounting/{tenant}/companies/{companyId}/vat-clearing-reports/{year}/{quarter}</span>  
  Params:  
  - year: int
  - quarter: (Q1 \| Q2 \| Q3 \| Q4)
  - Accept-language: base on language will generate description

  This API support get Vat Clearing base quarter. If report is existed, it will be automatically recalculate all codeBoxes with all BookingDetails which have booking date in that periods. If not, it generate new report  
    
- Update editable CodeBox:  
  Methods: **PUT** <span class="legacy-color-text-blue1">/luz_accounting/{tenant}/companies/{companyId}/vat-clearing-reports/{id}/code-boxes/{code}</span>  
  Params:  
  - code: code inside each box return by report
  - value: used to update to codebox

  We only update for editable code box which defined in each box

<!-- -->

- Get by Id  
  Methods: **GET** <span class="legacy-color-text-blue1">/luz_accounting/{tenant}/companies/{companyId}/vat-clearing-reports/{vatClearingReportId}</span>  
  This API get existed one in Database to show in GUI

%% ai-graph-start %%

**Related notes:**
- [[List out places calling booking function]]
- [[Employee Report Implementation (10.05.2023)]]
- [[Export CRM statistics by API]]
- [[Luz_google Api Document]]
- [[Generating document from viewgen]]

%% ai-graph-end %%