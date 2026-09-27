---
ai_hash: ae51d739118d36b8
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 9
depth: 2.73
entities: []
relevance: 0.731
source: https://axonivy.atlassian.net/wiki/spaces/X4/pages/47530443805/Avoid+warning+logs+related+to+Java+Problem+on+Ivy+Designer
space: X4
status: reference
tags:
- confluence
- programming
- space/x4
title: Avoid warning logs related to "Java Problem" on Ivy Designer
topic: programming
type: source
updated: 2023-10-25
---

# Avoid warning logs related to "Java Problem" on Ivy Designer

> [!info] Imported from Confluence
> Space **X4** · updated 2023-10-25 · [open original](https://axonivy.atlassian.net/wiki/spaces/X4/pages/47530443805/Avoid+warning+logs+related+to+Java+Problem+on+Ivy+Designer)
> Relevance 0.731 · topic `programming`

US: <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47530443805_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="AD-1851" macro-id="10aa8f53-313f-4d71-840c-cb3847a71e95" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/AD-1851" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>AD-1851</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>No</strong></p></th>
<th><p><strong>Warning log</strong></p></th>
<th><p><strong>How to fix</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td>

![[47530443805-image-20231025-090301.png]]

</td>
<td>

![[47530443805-image-20231025-090309.png]]


<p>Noted: Input exactly parameter needed - Ex: Object like above</p></td>
</tr>
<tr>
<td><p>2</p></td>
<td>

![[47530443805-image-20231025-090402.png]]

</td>
<td>

![[47530443805-image-20231025-090408.png]]


<p>Noted: Use DateUtil</p></td>
</tr>
<tr>
<td><p>3</p></td>
<td>

![[47530443805-image-20231025-090454.png]]

</td>
<td>

![[47530443805-image-20231025-090546.png]]


<p>Noted: Use Calendar</p></td>
</tr>
<tr>
<td><p>4</p></td>
<td>

![[47530443805-image-20231025-090608.png]]

</td>
<td>

![[47530443805-image-20231025-090659.png]]


<p>Noted: Use Mockito.anyListOf(*.class) instead of Mockito.anyList()</p></td>
</tr>
<tr>
<td><p>5</p></td>
<td><p>Another warning logs</p></td>
<td><p>Noted: Based on kind of warning logs - Adding <code>@SuppressWarnings</code>Ex: “rawtypes“, “unchecked“,….</p></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Upgrade Ivy - Known issues]]
- [[WIP Recipe How to adapt Unit Test to be able to run with Junit 5]]
- [[Problem of class cast exception]]
- [[How to debug java code in Ivy Designer]]
- [[Test and code review report template]]

%% ai-graph-end %%