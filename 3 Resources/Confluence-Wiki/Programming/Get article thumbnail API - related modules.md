---
title: "Get article thumbnail API - related modules"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47842656696/Get+article+thumbnail+API+-+related+modules
space: "FUT"
topic: programming
relevance: 0.755
depth: 2.81
updated: 2024-06-28
attachments: 0
tags:
  - confluence
  - programming
  - space/fut
---

# Get article thumbnail API - related modules

> [!info] Imported from Confluence
> Space **FUT** · updated 2024-06-28 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47842656696/Get+article+thumbnail+API+-+related+modules)
> Relevance 0.755 · topic `programming`

This page lists out all modules calling get article thumbnail APIs of luz-article.

- *GET /companies/{company-id}/documents/{document-id}/thumbnail*

- *GET /companies/{company-id}/documents/{document-id}/thumbnail/{size}*

To fix the performance issue <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47842656696_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-109145" macro-id="cf25e219-5263-46b5-bc30-638e80a2ee2b" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-109145" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-109145</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span> , the return type of these APIs is changed from byte\[\] to stream. Therefore, every module that calls those APIs has to adapt accordingly.

PR in luz_article: <a href="https://bitbucket.org/axonivy-prod/%7Bf822ba34-92a7-4f90-a80c-59d3726ce127%7D/pull-requests/934" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/%7Bf822ba34-92a7-4f90-a80c-59d3726ce127%7D/pull-requests/934</a>

<div>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Module</strong></p></th>
<th><p><strong>Java Class</strong></p></th>
<th><p><strong>Team</strong></p></th>
<th><p><strong>Status</strong></p></th>
<th><p><strong>Note</strong></p></th>
</tr>
&#10;<tr>
<td><p>luz_online</p></td>
<td><p>ArticleRestClient</p></td>
<td><p>Miracle</p></td>
<td><p><span>DONE</span></p></td>
<td><ul>
<li><p>Online shop: Helios</p></li>
<li><p>Online booking: Miracle</p></li>
</ul></td>
</tr>
<tr>
<td><p>luz_pos/luz_pos_adapter</p></td>
<td><p>LuzArticleRestClient</p></td>
<td><p>Helios</p></td>
<td><p><span>DONE</span></p></td>
<td></td>
</tr>
</tbody>
</table>

</div>
