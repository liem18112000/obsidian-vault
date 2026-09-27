---
title: "LUZ-59727 - CTI Interface | implementation"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/31171779824/LUZ-59727+-+CTI+Interface+implementation
space: "TS"
topic: programming
relevance: 0.746
depth: 2.57
updated: 2021-08-17
attachments: 1
tags:
  - confluence
  - programming
  - space/ts
---

# LUZ-59727 - CTI Interface | implementation

> [!info] Imported from Confluence
> Space **TS** · updated 2021-08-17 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/31171779824/LUZ-59727+-+CTI+Interface+implementation)
> Relevance 0.746 · topic `programming`

<div>

<table style="width:100%;">
<colgroup>
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
</colgroup>
<tbody>
<tr>
<td colspan="7" style="text-align: left; width: 1758.0px;"><div class="content-wrapper">
<p><span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_31171779824_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-59619" data-macro-id="03c93c5a-5dd8-418f-a641-73ca8e480f20" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-59619" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-59619</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span></p>
</div></td>
</tr>
<tr>
<td style="text-align: left; width: 48.0px;">No.</td>
<td style="text-align: left; width: 313.0px;">Case</td>
<td style="text-align: left; width: 351.0px;">Test steps</td>
<td style="text-align: left; width: 377.0px;">Test data</td>
<td style="text-align: left; width: 324.0px;">Expected result</td>
<td style="text-align: left; width: 238.0px;">Attachment (URL only)</td>
<td style="text-align: left; width: 107.0px;">Latest status</td>
</tr>
<tr>
<td style="text-align: left; width: 48.0px;">1</td>
<td style="text-align: left; width: 313.0px;">Show LeanSync part in Company Detail Page </td>
<td style="text-align: left; width: 351.0px;"><ul>
<li>Access to Klara Dashboard</li>
<li>Click on Company Title on Menu Item</li>
</ul>
<p><br />
</p></td>
<td style="text-align: left; width: 377.0px;"><br />
</td>
<td style="text-align: left; width: 324.0px;">The LeanSync will a part of Master Data.</td>
<td style="text-align: left; width: 238.0px;"><div class="content-wrapper">

![[31171779824-image2021-8-17_15-18-27.png]]


<p><br />
</p>
</div></td>
<td style="text-align: left; width: 107.0px;"><ul>
<li>OK</li>
</ul></td>
</tr>
<tr>
<td style="text-align: left;">2</td>
<td style="text-align: left;">Add valid LeanSync key</td>
<td style="text-align: left;"><ul>
<li>Access to Klara Dashboard</li>
<li>Click on Company Title on Menu Item</li>
<li>Click on LeanSync title</li>
<li>Click on "Add another key" button</li>
<li>Input title and key</li>
<li>Click on "Check key" button</li>
</ul></td>
<td style="text-align: left;"><p><span class="legacy-color-text-blue3"><strong>Title: </strong>Bern</span></p>
<p><span class="legacy-color-text-blue3"><strong>LeanSyncKey</strong>: deecad3f-3b86-4ec1-8bea-a31ea7cfb583</span></p></td>
<td style="text-align: left;">Show the green icon on the left</td>
<td style="text-align: left;"><br />
</td>
<td style="text-align: left;"><ul>
<li>OK</li>
</ul></td>
</tr>
<tr>
<td style="text-align: left;">3</td>
<td style="text-align: left;">Add invalid LeanSync key</td>
<td style="text-align: left;"><ul>
<li>Access to Klara Dashboard</li>
<li>Click on Company Title on Menu Item</li>
<li>Click on LeanSync title</li>
<li>Click on "Add another key" button</li>
<li>Input title and key</li>
<li>Click on "Check key" button</li>
</ul></td>
<td style="text-align: left;"><p><span class="legacy-color-text-blue3"><strong>Title: </strong>Bern</span></p>
<p><span class="legacy-color-text-blue3"><strong>LeanSyncKey</strong>: deecad3f-3b86-5fb5-8bea-a31ea7cfb583</span></p></td>
<td style="text-align: left;"><p>Show validation message</p>
<p><span>Your LeanSync Key is not valid</span></p></td>
<td style="text-align: left;"><br />
</td>
<td style="text-align: left;"><ul>
<li>OK</li>
</ul></td>
</tr>
<tr>
<td style="text-align: left;">4</td>
<td style="text-align: left;">Add valid LeanSync key → Change to invalid key</td>
<td style="text-align: left;"><ul>
<li>Access to Klara Dashboard</li>
<li>Click on Company Title on Menu Item</li>
<li>Click on LeanSync title</li>
<li>Click on "Add another key" button</li>
<li>Input title and key</li>
<li>Click on "Check key" button</li>
<li>Change to invalid key</li>
<li>Click on "Check key" button</li>
</ul></td>
<td style="text-align: left;"><p><span class="legacy-color-text-blue3"><strong>Title: </strong>Bern</span></p>
<p><span class="legacy-color-text-blue3"><strong>LeanSyncKey</strong>: deecad3f-3b86-4ec1-8bea-a31ea7cfb583</span></p>
<p><strong><span class="legacy-color-text-blue3">Change to: </span></strong><span class="legacy-color-text-blue3">deecad3f-3b86-5fb5-8bea-a31ea7cfb583</span></p></td>
<td style="text-align: left;"><p>Hide the green icon on the left</p>
<p>Show validation message</p>
<p><span>Your LeanSync Key is not valid</span></p></td>
<td style="text-align: left;"><br />
</td>
<td style="text-align: left;"><ul>
<li>OK</li>
</ul></td>
</tr>
<tr>
<td style="text-align: left;">5</td>
<td style="text-align: left;">Add valid LeanSync key → Add more LeanSync key</td>
<td style="text-align: left;"><ul>
<li>Access to Klara Dashboard</li>
<li>Click on Company Title on Menu Item</li>
<li>Click on LeanSync title</li>
<li>Click on "Add another key" button</li>
<li>Input title and key</li>
<li>Click on "Check key" button</li>
<li>Click on "Add another key" button</li>
</ul></td>
<td style="text-align: left;"><p><span class="legacy-color-text-blue3"><strong>Title: </strong>Bern</span></p>
<p><span class="legacy-color-text-blue3"><strong>LeanSyncKey</strong>: deecad3f-3b86-4ec1-8bea-a31ea7cfb583</span></p>
<p><span class="legacy-color-text-blue3"><strong>Title:</strong> Lucern</span></p>
<p><span class="legacy-color-text-blue3"><strong>LeanSyncKey</strong>: deecad3f-3b86-4ec1-8bea-a31ea7cfb696</span></p></td>
<td style="text-align: left;"><p>First key: Show the green icon on the left</p>
<p>Second key: Show validation message</p>
<p><span>Your LeanSync Key is not valid</span></p></td>
<td style="text-align: left;"><br />
</td>
<td style="text-align: left;"><ul>
<li>OK</li>
</ul></td>
</tr>
<tr>
<td style="text-align: left;">6</td>
<td style="text-align: left;">Remove valid LeanSync key</td>
<td style="text-align: left;"><ul>
<li>Access to Klara Dashboard</li>
<li>Click on Company Title on Menu Item</li>
<li>Click on LeanSync title</li>
<li>Click on "Add another key" button</li>
<li>Input title and key</li>
<li>Click on "Check key" button</li>
<li>Click on remove key</li>
</ul></td>
<td style="text-align: left;"><p><span class="legacy-color-text-blue3"><strong>Title: </strong>Bern</span></p>
<p><span class="legacy-color-text-blue3"><strong>LeanSyncKey</strong>: deecad3f-3b86-4ec1-8bea-a31ea7cfb583</span></p></td>
<td style="text-align: left;"><p>Show the green icon on the left</p>
<p>→ The key is removed</p></td>
<td style="text-align: left;"><br />
</td>
<td style="text-align: left;"><ul>
<li>OK</li>
</ul></td>
</tr>
<tr>
<td style="text-align: left;">7</td>
<td style="text-align: left;">Remove invalid LeanSync key</td>
<td style="text-align: left;"><ul>
<li>Access to Klara Dashboard</li>
<li>Click on Company Title on Menu Item</li>
<li>Click on LeanSync title</li>
<li>Click on "Add another key" button</li>
<li>Input title and key</li>
<li>Click on "Check key" button</li>
<li>Click on remove key</li>
</ul></td>
<td style="text-align: left;"><p><span class="legacy-color-text-blue3"><strong>Title:</strong> Lucern</span></p>
<p><span class="legacy-color-text-blue3"><strong>LeanSyncKey</strong>: deecad3f-3b86-4ec1-8bea-a31ea7cfb696</span></p></td>
<td style="text-align: left;"><p> Show validation message</p>
<p><span>Your LeanSync Key is not valid</span></p>
<p>→ The key is removed. Validation message is hiden.</p></td>
<td style="text-align: left;"><br />
</td>
<td style="text-align: left;"><ul>
<li>OK</li>
</ul></td>
</tr>
</tbody>
</table>

</div>
