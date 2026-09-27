---
ai_hash: cbe9308782f92a07
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 6
depth: 2.44
entities: []
relevance: 0.711
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47242215811/New+architecture+for+documentStatistic
space: LUZ
status: reference
tags:
- confluence
- architecture
- space/luz
title: New architecture for documentStatistic
topic: architecture
type: source
updated: 2023-01-04
---

# New architecture for documentStatistic

> [!info] Imported from Confluence
> Space **LUZ** · updated 2023-01-04 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47242215811/New+architecture+for+documentStatistic)
> Relevance 0.711 · topic `architecture`

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Problem</strong></p></th>
<th><p><strong>Solution</strong></p></th>
<th><p><strong>Benefit</strong></p></th>
</tr>
&#10;<tr>
<td><p>Utilize pods</p></td>
<td><p>Split statistic to separate module (luz_docs_statistic) and use Java Timer to schedule</p></td>
<td><p>Resources for updating statistic can freely scale due to number of request</p></td>
</tr>
<tr>
<td><p>JsonStore resources aren’t stable because of cronjob scheduled each 5 min</p>

![[47242215811-image-20221226-070947.png]]

</td>
<td><p>Java Timer reduce time from 5 min to 30s (100 → 10 msg for each time)</p></td>
<td><p>JsonStore resources more stable</p></td>
</tr>
<tr>
<td><p>API to bring out of luz_docs</p></td>
<td><p>getLatestDocumentStatistic</p>
<p>getArchivedDocumentsPerTenant</p></td>
<td></td>
</tr>
</tbody>
</table>

</div>


![[47242215811-luz_docs_statistic.png]]



\*keep interact with luz_cache to reduce spam

## Modules need to be adapted

<div>

|  |  |
|----|----|
| **Module** | **Changes** |
| luz_docs | Remove DocumentStatisticResource |
| luz_store | Adapt endpoint to get document statistic for consumption billing |

</div>

## Apply fault tolerance

<div>

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Module</strong></p></th>
<th><p><strong>Type</strong></p></th>
<th><p><strong>Service</strong></p></th>
<th><p><strong>Fault Tolerance</strong></p></th>
</tr>
&#10;<tr>
<td><p>luz_docs_statistic</p></td>
<td><p>API</p></td>
<td><p>getLatestDocumentStatistic</p></td>
<td><p>Retry: 3 times</p></td>
</tr>
<tr>
<td><p>luz_jsonstore</p></td>
<td><p>REST client</p></td>
<td><p>getFacets<br />
getCollectionsByFilter<br />
updateCollectionByFilter<br />
createCollection</p></td>
<td><p>ConnectTimeout: 3000ms<br />
ReadTimeout: 15000ms</p>
<p>@CircuitBreaker<br />
Threads hold: 10<br />
ratio: 0.5</p></td>
</tr>
<tr>
<td><p>luz_cache</p></td>
<td><p>REST client</p></td>
<td><p>getCache<br />
deleteCache</p></td>
<td><p>ConnectTimeout: 3000ms<br />
ReadTimeout: 2000ms</p>
<p>@CircuitBreaker<br />
Threads hold: 10<br />
ratio: 0.5</p></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[luz_docs_statistic updates stats via 1-minute EJB timer over PubSub and $facet aggregation]]
- [[Background Process Optimization for luz-docs API Architecture and Recommendations]]
- [[LUZ Audit Refactor- 2025-2026]]
- [[Measure API luz-docs]]
- [[luz_docs Improvement - Document Reliable Delivery Proof Of Concept]]

%% ai-graph-end %%