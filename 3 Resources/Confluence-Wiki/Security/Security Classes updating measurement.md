---
ai_hash: eedd6eec4b90b7d5
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 6
depth: 2.8
entities: []
relevance: 0.721
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47285240040/Security+Classes+updating+measurement
space: LUZ
status: reference
tags:
- confluence
- security
- space/luz
title: Security Classes updating measurement
topic: security
type: source
updated: 2023-02-03
---

# Security Classes updating measurement

> [!info] Imported from Confluence
> Space **LUZ** · updated 2023-02-03 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47285240040/Security+Classes+updating+measurement)
> Relevance 0.721 · topic `security`

## **Scenario**:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1810c51d-5998-4e4d-a82d-467a570c8dc0" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
FolderA {securityClassCodes:[“secA1“,”secA2”]}
FolderB {securityClassCodes:[“secA1“,”secB1”]}

        Document1 { 
        folderIds: [“FolderA“,”FolderB”],
        securityClassCodes: [“SecA1”, “SecA2“, “SecB1”]
        }
        … 
        Documentn.(same as above)
```

</div>

</div>

API to measure: Remove security classes of folder.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="68b21d04-d428-42fc-ab15-785fe38625e4" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
http://localhost:8080/luz_docs/api/{{tenantId}}/folders/{folderAId}/security-classes
{
    "op": "remove",
    "value": [
        "SecA1",
        "SecA2"
    ]
}
```

</div>

</div>

<span class="legacy-color-text-inverse">ders/63db88Q7bf237bc323eebc319/security-classes</span>

Note: n is the number of documents located in updated folder (could be in updated folder or its child folders)

<div>

|  |  |  |
|----|----|----|
| **n (number of documents)** | **Time Processing** | **Image** |
| 10 | 2.14s | 

![[47285240040-image-20230203-085143.png]]

 |
| 50 | 11.13s | 

![[47285240040-image-20230203-085519.png]]

 |
| 100 | 11.90s | 

![[47285240040-image-20230203-090537.png]]

 |

</div>

%% ai-graph-start %%

**Related notes:**
- [[Add Remove security class for folder]]
- [[Research on bulk removal of access class]]
- [[Measure the time-consuming of patch update document API in luz_docs]]
- [[Research on Delete Access class]]
- [[luz-docs folder delete verifies document security classes with one limit-1 Mongo query per folder]]

%% ai-graph-end %%