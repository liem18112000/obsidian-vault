---
ai_hash: 5aa83493aefc3b33
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 2
depth: 2.67
entities: []
relevance: 0.81
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47566880791/luz_docs+Improvement+-+Document+Reliable+Delivery+Proof+Of+Concept
space: LUZ
status: reference
tags:
- confluence
- architecture
- space/luz
title: luz_docs Improvement - Document Reliable Delivery Proof Of Concept
topic: architecture
type: source
updated: 2023-11-23
---

# luz_docs Improvement - Document Reliable Delivery Proof Of Concept

> [!info] Imported from Confluence
> Space **LUZ** · updated 2023-11-23 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47566880791/luz_docs+Improvement+-+Document+Reliable+Delivery+Proof+Of+Concept)
> Relevance 0.81 · topic `architecture`

<div class="panel conf-macro output-block" hasbody="true" macro-id="65766f13-0511-49e0-98bc-1f9c5d1aeb0d" macro-name="panel" style="background-color: #DEEBFF;border-width: 1px;">

<div class="panelContent" style="background-color: #DEEBFF;">

1.  **Objective**

- Find a solution to support OneAPI’s reliable delivery of documents to recipients.

- Make luz_docs automatically retry when it fails to store documents due to any error occurring or other services being unavailable.

</div>

</div>

<div class="panel conf-macro output-block" hasbody="true" macro-id="8af57dce-267d-498c-a59e-bfb839c5e522" macro-name="panel" style="background-color: #E3FCEF;border-width: 1px;">

<div class="panelContent" style="background-color: #E3FCEF;">

2.  **Acceptance Criteria**

- The user sends 100K documents and expects every document to be delivered to the recipients

</div>

</div>

<div class="panel conf-macro output-block" hasbody="true" macro-id="567afdcb-f399-485d-bb15-ff6be87cc45f" macro-name="panel" style="background-color: #E3FCEF;border-width: 1px;">

<div class="panelContent" style="background-color: #E3FCEF;">

3.  **Sample concept**

</div>

</div>

> *Introduce a new luz_docs API, (e.g.* `createDocumentAsync`*), handle asynchronous document creation with a try/catch block, and use Google Pub/Sub to retry every time there is a failure.*


![[47566880791-image-20231122-101122.png]]



<div class="panel conf-macro output-block" hasbody="true" macro-id="20d9dc2a-4b8f-4c0d-bf0f-f3dffa792ca3" macro-name="panel" style="background-color: #EAE6FF;border-width: 1px;">

<div class="panelContent" style="background-color: #EAE6FF;">

4.  **General questions**

</div>

</div>

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>QUESTION</strong></p></th>
<th><p><strong>ANSWER</strong></p></th>
</tr>
&#10;<tr>
<td><p>Is the OneAPI needed to know the status of the document sending process in the response?</p></td>
<td><p><em>Specifically, now there are 2 steps that OneAPI using</em> <code>luz_docs</code> <em>to store the documents:</em> <a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47341601109/One+API+Pub+Sub+Flow?atlOrigin=eyJpIjoiN2M3OTYyYzFlYmI0NDQwN2IwNzBiYTJjYWQ1NGMwOWUiLCJwIjoiYyJ9" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47341601109/One+API+Pub+Sub+Flow?atlOrigin=eyJpIjoiN2M3OTYyYzFlYmI0NDQwN2IwNzBiYTJjYWQ1NGMwOWUiLCJwIjoiYyJ9</a></p>
<p><em>For example, the Sender sends a request that includes 100K documents intended for 100K desired recipients.</em></p>
<ul>
<li><p><em>1st step:</em> <code>luz_eletter</code> <em>receives 100K documents (which are temporarily stored in the</em> <code>eletter</code> <em>temp memory and need to be released). Subsequently, the</em> <code>eletter</code> <em>will move 100K files to the sender database of</em> <code>luz_docs</code><em>. The precondition for the entire OneAPI flow is that these files are successfully stored in</em> <code>luz_docs</code><em>, requiring a 200 response.</em></p></li>
<li><p><em>2nd step: Each file in the sender database of</em> <code>luz_docs</code> <em>is delivered to the recipient database of</em> <code>luz_docs</code><em>. This step runs asynchronously, with one file per thread and a maximum of 30 threads. If there is a failure, the process will be retried every 5 minutes, with a maximum of 3 retry attempts. This step not necessary to know the delivery status.</em></p></li>
</ul></td>
</tr>
</tbody>
</table>

</div>

<div hasbody="true" macro-id="79dc7f76-16ff-41b7-8684-255d3497dcb0" macro-name="tip">

<span class="aui-icon aui-icon-small aui-iconfont-approve confluence-information-macro-icon"> </span>

<div>

5.  **Summary**

- Retry every time there is a failure that does not match what OneAPI is expecting

- OneAPI requires a new `luz_docs` implementation that does not cause a bottleneck on `luz_eletter`, because the slow storing of luz_docs is causing eletter to be unable to free the memory.

</div>

</div>

------------------------------------------------------------------------

## Related stories:

<div class="static-jira-issues_count confluence-jim-macro sllv-card" style="display:inline-block;border:1px solid #DFE1E6;border-radius:3px; background-color:#FAFBFC;padding:8px 12px;margin:4px 0; font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',Roboto,sans-serif; line-height:1.4;">

<div class="sllv-card-count" style="font-weight:600;font-size:14px;color:#172B4D;margin-bottom:4px;">

<a href="https://axonivy.atlassian.net/browse/LUZ-110979" class="issue-link" style="color:#172B4D;text-decoration:none;">LUZ-110979: POC: luz_docs - return "failed to store" value</a>

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Research The concept to update the status of delivery instantly after all documents are processed]]
- [[Retry for storing documents from One API to luz_docs_view_controller]]
- [[High-Level Design - ONE API Enricher-First Integration]]
- [[If upstream holds memory until you ack, your write latency is their OOM risk]]
- [[Bottleneck analysis large-recipient deliveries & cross-sender impact]]

%% ai-graph-end %%