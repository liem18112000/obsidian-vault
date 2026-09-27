---
title: "Security Classes updating measurement"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47285240040/Security+Classes+updating+measurement
space: "LUZ"
topic: security
relevance: 0.721
depth: 2.8
updated: 2023-02-03
attachments: 6
tags:
  - confluence
  - security
  - space/luz
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
