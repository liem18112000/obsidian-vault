---
ai_hash: 9ea21ece5bf8c3d1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.81
entities: []
relevance: 0.724
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38142261633/Task
space: Helios
status: reference
tags:
- confluence
- programming
- space/helios
title: Task
topic: programming
type: source
updated: 2017-01-31
---

# Task

> [!info] Imported from Confluence
> Space **Helios** · updated 2017-01-31 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38142261633/Task)
> Relevance 0.724 · topic `programming`

Returns information about a single task

## Request

GET \[/ivy/api/{application}/workflow/task/{taskId}\]

## Response

 

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="86e55ffa-2f04-4aca-8bfa-9bac8196d8f8" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**Response Payload (Status Code: OK)**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
    "name": "",
    "id": 969,
    "priority": 0,
    "state": 4,
    "description": "",
    "startTimeStamp": "2014-12-04T08:01:58Z",
    "expiryTimeStamp": null,
    "activatorName": "technician",
    "fullRequestPath": "http://localhost:8081/ivy/pro/designer/testProject$1/14C3689E58C6B13D/14C3689E58C6B13D-f3/TaskA.iot?taskId=969",
    "offline": true,
    "case": {
        "id": 970,
        "name": "",
        "description": "",
        "documents": [{
            "id": 735,
            "name": "docA.txt",
            "url": "http://localhost:8081/ivy/api/designer/workflow/case/970/document/735",
            "path": "files/Default/970/Documents/docA.txt"
        }, {
            "id": 738,
            "name": "image.png",
            "url": "http://localhost:8081/ivy/api/designer/workflow/case/970/document/738",
            "path": "files/Default/970/Documents/image.png"
        }]
    }
}
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[REST API for deleting EXPENSES documents]]
- [[Task List Mass Processing technical notes]]
- [[Load test get document id API]]
- [[APF swagger for project eapf_web]]
- [[Git source code and Jenkins]]

%% ai-graph-end %%