---
ai_hash: 1f94dacfd6943c57
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.65
entities: []
relevance: 0.738
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/48553623640/Kotlin+migration+plan
space: Helios
status: reference
tags:
- confluence
- programming
- space/helios
title: Kotlin migration plan
topic: programming
type: source
updated: 2026-01-27
---

# Kotlin migration plan

> [!info] Imported from Confluence
> Space **Helios** · updated 2026-01-27 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/48553623640/Kotlin+migration+plan)
> Relevance 0.738 · topic `programming`

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
<th></th>
<th><p><strong>Package</strong></p></th>
<th><p><strong>Classes</strong></p></th>
<th><p><strong>Line of code</strong></p></th>
<th><p><strong>Raw EST (pts)</strong></p></th>
<th><p><strong>Stories (pts)</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p>App</p></td>
<td><p>860</p></td>
<td><p>107'238</p></td>
<td><p>21 → 34</p></td>
<td><ol>
<li><p><strong>Base Fragment</strong> <strong>(10)</strong> - <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="6fa0df65-30a2-45bc-a0ac-14eb8b1f7bf6" data-macro-name="status">DONE</span><br />
1.1 (3p) <span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_48553623640_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-136935" data-macro-id="fa100e8e-ccc7-4410-8199-5960e603db09" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-136935" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-136935</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span><br />
1.2 (3p) <span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_48553623640_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-137528" data-macro-id="dc6a4301-b218-4c73-961a-1d29ed24ff86" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-137528" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-137528</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span><br />
</p></li>
<li><p><strong>Base Presenter (5)</strong> - <span class="status-macro aui-lozenge aui-lozenge-visual-refresh conf-macro output-inline" data-hasbody="false" data-macro-id="4f03f739-2d95-40b1-88e6-61e9ad2d5d7c" data-macro-name="status">TODO</span><br />
2.1 (3p) <span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_48553623640_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-137529" data-macro-id="5b3a577a-928d-4c46-83cd-04f719270555" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-137529" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-137529</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span><br />
2.2 (3p) <span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_48553623640_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-144913" data-macro-id="21ffb0d2-9842-4992-aa79-9608e4250d8d" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-144913" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-144913</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span><br />
</p></li>
<li><p><strong>Base Activity (2)</strong> - <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="879f34f9-0e7d-4496-9c7b-4e5fedeb3fec" data-macro-name="status">DONE</span><br />
3.1 (2p) <span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_48553623640_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-136950" data-macro-id="b4136acc-c17f-4146-aa7e-20fe60d09ca6" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-136950" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-136950</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span><br />
</p></li>
<li><p><strong>Views (3) -</strong> <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="8ce7e7a6-bff8-4884-a784-70323774ce3e" data-macro-name="status">DONE</span><br />
(3p) <span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_48553623640_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-144918" data-macro-id="58ebfe50-935a-4331-bd5e-3e9a9909a252" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-144918" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-144918</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span><br />
</p></li>
<li><p><strong>Utils/Common (3)</strong> <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="76fb5781-6753-4a5b-a900-3227c6902a61" data-macro-name="status">DONE</span><br />
(3p) <a href="https://axonivy.atlassian.net/jira/software/c/projects/LUZ/boards/127/backlog?selectedIssue=LUZ-144919" class="external-link" data-card-appearance="inline" data-local-id="1094b9d4-b8a5-4717-938b-f7bddfc48f7a" rel="nofollow">https://axonivy.atlassian.net/jira/software/c/projects/LUZ/boards/127/backlog?selectedIssue=LUZ-144919</a><br />
</p></li>
<li><p><strong>Dialogs[~50 class] (3)</strong> <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-error conf-macro output-inline" data-hasbody="false" data-macro-id="e491c447-b6c0-4158-90c0-cdb1aa22c6b7" data-macro-name="status">NOT PLANED YET</span></p></li>
</ol>
<p><span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_48553623640_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-145633" data-macro-id="1b148b13-30ef-4853-b261-c41a1215ca73" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-145633" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-145633</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span></p>
<ol start="7">
<li><p><strong>Others (5)</strong> <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-error conf-macro output-inline" data-hasbody="false" data-macro-id="00a48f1d-dd76-4c01-aef8-f3b6b4b67db0" data-macro-name="status">NOT PLANED YET</span></p></li>
</ol></td>
</tr>
<tr>
<td>2</td>
<td><p>Devices</p></td>
<td><p>71</p></td>
<td><p>6'982</p></td>
<td><p>2</p></td>
<td></td>
</tr>
<tr>
<td>3</td>
<td><p>Domain</p></td>
<td><p>488</p></td>
<td><p>26'173</p></td>
<td><p>8</p></td>
<td></td>
</tr>
<tr>
<td>4</td>
<td><p>Data</p></td>
<td><p>461</p></td>
<td><p>41'352</p></td>
<td><p>13</p></td>
<td><ol>
<li><p>Mappers</p></li>
<li><p>Data classes</p></li>
</ol></td>
</tr>
<tr>
<td>5</td>
<td><p>Common</p></td>
<td><p>33</p></td>
<td><p>3'264</p></td>
<td><p>1</p></td>
<td></td>
</tr>
<tr>
<td>6</td>
<td><p>ExternalScreen</p></td>
<td><p>0</p></td>
<td><p>0</p></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[List selection Error handling on multi-selection actions]]
- [[Test and code review report template.2.93]]
- [[Merging process]]
- [[00. Test and code review report template]]
- [[Test and code review report template]]

%% ai-graph-end %%