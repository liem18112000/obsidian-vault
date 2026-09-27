---
ai_hash: e5815f3a9293185c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 12
depth: 2.45
entities: []
relevance: 0.714
source: https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/48681943256/Regular+Load+Test+Performance+Test+of+OneAPI
space: HACKA
status: reference
tags:
- confluence
- infra
- space/hacka
title: Regular Load Test / Performance Test of OneAPI
topic: infra
type: source
updated: 2025-10-07
---

# Regular Load Test / Performance Test of OneAPI

> [!info] Imported from Confluence
> Space **HACKA** · updated 2025-10-07 · [open original](https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/48681943256/Regular+Load+Test+Performance+Test+of+OneAPI)
> Relevance 0.714 · topic `infra`

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_48681943256_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-140246" macro-id="079b506c-1b73-437f-bb1b-60117b019818" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-140246" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-140246</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

## <span class="inline-comment-marker" ref="e5475be4-4f38-4683-8a81-4d6543adc847">Load test with Grafana K6 17.09.2025 (Sprint \#165</span>)

Link: <a href="https://epost.grafana.net/a/k6-app/runs/5548893?tab=logs" class="external-link" rel="nofollow"><u>https://epost.grafana.net/a/k6-app/runs/5548893?tab=logs</u></a>  
Start time: 09-17-2025 08:52:27 AM  
End time: 09-17-2025 09:22:00 AM  
Total deliveries: 498  
1 delivery has 2 documents and 5 recipients per document.

Last delivery updated: 2025-09-17 04:33:22 (09-17-2025 09:33:22 AM)  
It took 41 minutes to complete all deliveries except for 1 delivery, which had a migration issue


![[48681943256-image-20250924-034915.png]]



==\> We need to improve the test script.


![[48681943256-image-20250924-035339.png]]



<div>

<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>ID</strong></p></th>
<th><p><strong>Start date</strong></p></th>
<th><p><strong>End date</strong></p></th>
<th><p><strong>Sprint</strong></p></th>
<th><p><strong>Q&amp;A</strong></p></th>
<th><p><strong>Note</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p>09-17-2025 08:52:27 AM</p></td>
<td><p>09-17-2025 09:22:00 AM</p></td>
<td><h2 id="RegularLoadTest/PerformanceTestofOneAPI-165"><span class="inline-comment-marker" data-ref="e5475be4-4f38-4683-8a81-4d6543adc847">165</span></h2></td>
<td><ul>
<li><span class="placeholder-inline-tasks">Is OneAPI able to process 600 messages per minute (i.e. 10 messages per second)</span></li>
<li><span class="placeholder-inline-tasks">Did the scaling of all OneAPI modules work correctly?</span></li>
<li><span class="placeholder-inline-tasks"><span class="inline-comment-marker" data-ref="74f7c3b5-fd37-4c12-98ef-42d3bd58ddd1">Is the rate of delivering roughly the same as the rate of receiving</span></span></li>
<li><span class="placeholder-inline-tasks"><span class="inline-comment-marker" data-ref="69ea27f3-270b-4b75-98f6-d8b3d4f3f5b1">Check message queues: There should be no large buildup in any of our queues during the load test</span></span></li>
</ul></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/48710582282/Sprint-165+Load+test+One+API">Sprint-165 Load test One API</a></p></td>
</tr>
<tr>
<td><p>2</p></td>
<td></td>
<td></td>
<td><p>166</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks">Is OneAPI able to process 600 messages per minute (i.e. 10 messages per second)</span></li>
<li><span class="placeholder-inline-tasks">Did the scaling of all OneAPI modules work correctly?</span></li>
<li><span class="placeholder-inline-tasks"><span class="inline-comment-marker" data-ref="74f7c3b5-fd37-4c12-98ef-42d3bd58ddd1">Is the rate of delivering roughly the same as the rate of receiving</span></span></li>
<li><span class="placeholder-inline-tasks"><span class="inline-comment-marker" data-ref="69ea27f3-270b-4b75-98f6-d8b3d4f3f5b1">Check message queues: There should be no large buildup in any of our queues during the load test</span></span></li>
</ul></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/x/AoBTVws" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/x/AoBTVws</a></p></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[EPC API - Load Test]]
- [[One API end to end testing]]
- [[Create Document API – Performance Testing Report]]
- [[Invoice Run V2UATExecute - Prevent error when luz-store is multiple pods]]
- [[Research The concept to update the status of delivery instantly after all documents are processed]]

%% ai-graph-end %%