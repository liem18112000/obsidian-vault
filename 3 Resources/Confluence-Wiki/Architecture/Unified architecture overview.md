---
title: "Unified architecture overview"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47751823411/Unified+architecture+overview
space: "FUT"
topic: architecture
relevance: 0.716
depth: 2.11
updated: 2024-12-13
attachments: 2
tags:
  - confluence
  - architecture
  - space/fut
---

# Unified architecture overview

> [!info] Imported from Confluence
> Space **FUT** · updated 2024-12-13 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47751823411/Unified+architecture+overview)
> Relevance 0.716 · topic `architecture`

![[47751823411-unified epost communication overview.png]]



**Epost Hub Services:** [ePost Communication Platform Services](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47409431288/ePost+Communication+Platform+Services)

### Q&A:

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th></th>
<th><p><strong>Questions</strong></p></th>
<th><p><strong>Answers</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p><strong>Orchestrator Design</strong></p>
<p>How many orchestrators should we implement? Would a single orchestrator effectively manage all services, or would utilizing multiple orchestrators offer better control and efficiency?</p></td>
<td></td>
</tr>
<tr>
<td>2</td>
<td><p><strong>Document Metadata Temporarily Storage step?</strong></p>
<p>Currently, we are using luz-storage exclusively for temporary storage of binary files. Since we also need to store document metadata, should we expand luz-storage to accommodate this metadata, or would it be more effective to utilize a separate solution?</p></td>
<td></td>
</tr>
<tr>
<td>3</td>
<td><p><strong>Unified Monitoring Approach</strong></p>
<p>Currently, ePost Hub utilizes MongoDB for data storage and monitoring, while luz-eletter employs PostgreSQL. What would be the best strategy to unify our monitoring system across both platforms to achieve more cohesive insights?</p></td>
<td></td>
</tr>
<tr>
<td>4</td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

</div>
