---
ai_hash: 63b41284cfcf5e87
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 3
depth: 2.33
entities: []
relevance: 0.711
source: https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47110291757/Research+on+bulk+removal+of+access+class
space: TP2020
status: reference
tags:
- confluence
- programming
- space/tp2020
title: Research on bulk removal of access class
topic: programming
type: source
updated: 2022-05-13
---

# Research on bulk removal of access class

> [!info] Imported from Confluence
> Space **TP2020** · updated 2022-05-13 · [open original](https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47110291757/Research+on+bulk+removal+of+access+class)
> Relevance 0.711 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="0988771b3b6f17e5ca14b18b0d4b2a65" macro-name="toc">

</div>

## I. Discussion

### 1. UI/UX

- In this case, the user removes a security class that has been assigned to a lot of documents. The removal process will take much time =\> *Do we need to do some UI block to make the user know the process isn’t finished and prevent the user do another activity during this time?*

\- Should we show the processing bar?

\- Should we show the loading spinner during removing time?

- What happens if during time remove security class has the failures?

Do we need to show the notification popup to make the user know what are the document failures?

=\> Option “Try again”: Retrying remove the security class in that failures documents

=\> Option “Skip”: Continue to remove the security class for the remaining documents

=\> If do like this → *Need the design for this failure notification popup*

- After removing a security class successfully, Do we need to show the successful popup?

- If removing a security class takes a lot of time then the application can take the timeout status.

\- Do we need to roll back the data?

\- Do we need to show the error popup to make the user know this status?

- Do we need to support undoing an action?

- Do we need to support the user can stop removing actions immediately?

### 2. Performance

- The number of API calls

<div>

<table>
<tbody>
<tr>
<th colspan="2"><p><strong>Call update letter API</strong></p></th>
</tr>
&#10;<tr>
<td><p>Number of letter</p></td>
<td><p>Time (second)</p></td>
</tr>
<tr>
<td><p>1</p></td>
<td><p>0.45</p></td>
</tr>
<tr>
<td><p>10</p></td>
<td><p>4.5</p></td>
</tr>
<tr>
<td><p>100</p></td>
<td><p>40.5</p></td>
</tr>
<tr>
<td><p>1000</p></td>
<td><p>450</p></td>
</tr>
</tbody>
</table>

</div>

Environment: <a href="https://dev-vn.klara.tech/" class="external-link" data-card-appearance="inline" rel="nofollow">https://dev-vn.klara.tech/</a>

The process will take a lot of time if the user removes a security class that has been assigned to many letters.

=\> *we need to find out the solution to call update letter metadata async*

## II. Solution

#### 1. IvyAsyncRunner

Some documents we can refer to:

\- <a href="https://developer.axonivy.com/doc/8.0/public-api/ch/ivyteam/util/threadcontext/IvyAsyncRunner.html" class="external-link" data-card-appearance="inline" rel="nofollow">https://developer.axonivy.com/doc/8.0/public-api/ch/ivyteam/util/threadcontext/IvyAsyncRunner.html</a>

\- <a href="https://bitbucket.org/axonivy-prod/%7B019bbb66-3470-496e-91c3-762a2fa476f6%7D/pull-requests/1110" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/%7B019bbb66-3470-496e-91c3-762a2fa476f6%7D/pull-requests/1110</a>

#### 2. Patch Operation API

Use Patch Operation API instead of the update letter API: [API "Update Patch metadata" (patch operation)](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47089451966/API+Update+Patch+metadata+patch+operation)

<div id="expander-679842441" class="expand-container conf-macro output-block" hasbody="true" macro-id="d0dc2f1f-ba61-49d3-baa3-fe3f66fd3ed7" macro-name="expand">

<div id="expander-control-679842441" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Request body</span>

</div>

<div id="expander-content-679842441" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="53076e26-da4c-4125-a9c0-81b9c690e263" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
[
    {
        "op": "remove",
        "path": "/securityClassCodes",
        "value": [
            "PRIVATE_86"
        ]
    },
    {
        "op": "add",
        "path": "/securityClassModificationHistoryEntries/-",
        "value": [
            {
                "byUser": "khoa.tang@axonactive.com",
                "atTime": "2022-05-10T04:21:22.021189Z",
                "action": "REMOVE_ELEMENT",
                "securityClassCode": "PRIVATE_86",
                "securityClassName": "Doris Gottlieb"
            }
        ]
    }
]
    
```

</div>

</div>


![[47110291757-image-20220513-045441.png]]



</div>

</div>

- Example implementation: <a href="https://bitbucket.org/axonivy-prod/luz_docs_view_controller/branch/pioneer/LUZ-78190/research-use-patch-operation-to-unassign-security-class" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_docs_view_controller/branch/pioneer/LUZ-78190/research-use-patch-operation-to-unassign-security-class</a>

%% ai-graph-start %%

**Related notes:**
- [[Research on Delete Access class]]
- [[Enhancements for API Delete and Restore]]
- [[Security Classes updating measurement]]
- [[Measure the time-consuming of patch update document API in luz_docs]]
- [[Enhance performance - Research on Parallel]]

%% ai-graph-end %%