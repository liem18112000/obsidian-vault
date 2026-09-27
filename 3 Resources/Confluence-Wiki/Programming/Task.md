---
title: "Task"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38142261633/Task
space: "Helios"
topic: programming
relevance: 0.724
depth: 2.81
updated: 2017-01-31
attachments: 0
tags:
  - confluence
  - programming
  - space/helios
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
