---
ai_hash: 9c5c1fa0eb5e2a4c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 1
depth: 3
entities: []
relevance: 0.877
source: https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47150006413/Analytics+Analyze+API+call+when+accessing+eArchive
space: TP2020
status: reference
tags:
- confluence
- programming
- space/tp2020
title: '[Analytics] Analyze API call when accessing eArchive'
topic: programming
type: source
updated: 2022-08-04
---

# [Analytics] Analyze API call when accessing eArchive

> [!info] Imported from Confluence
> Space **TP2020** · updated 2022-08-04 · [open original](https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47150006413/Analytics+Analyze+API+call+when+accessing+eArchive)
> Relevance 0.877 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="34a06ed62edeb5a9b0dc285ed171336e" macro-name="toc">

</div>

# Which data show on page eArchive

## 1. Check subscription: subscription list

- DONE in <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47150006413_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-80493" macro-id="d100d89f-8d7b-4bb6-9406-cbaf64fe04b2" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-80493" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-80493</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

## 2. When not subscribe eArchive

- eArchive banner:

- Branded folder list:

- Custom folder list:

- Document section:

## 3. When eArchive subscribed

<div id="expander-125154228" class="expand-container conf-macro output-block" hasbody="true" macro-id="2c00c9ac-9e25-4a2b-bc17-dffccaef97eb" macro-name="expand">

<div id="expander-control-125154228" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Filter bar: defer loading</span>

</div>

<div id="expander-content-125154228" class="expand-content expand-hidden">

<div>

<table>
<tbody>
<tr>
<th></th>
<th></th>
<th><p><strong>API</strong></p></th>
<th><p><strong>Query</strong></p></th>
<th><p><strong>Byte-sent</strong></p></th>
<th><p><strong>Time consuming</strong></p></th>
<th><p><strong>Proposal</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p><strong>Tag filter</strong></p></td>
<td><p>luz_docs_view_controller/api/documents/search</p></td>
<td><p>SearchRequest</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>2</td>
<td rowspan="3"><p><strong>Security class filter</strong></p></td>
<td><p>luz_docs_view_controller/api/letters/security-class-codes</p></td>
<td><p>IsStored = true</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>3</td>
<td><p>luz_docs_view_controller/api/letters/security-class-codes</p></td>
<td><p>IsStored = false, origin = user upload</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>4</td>
<td><p>luz_docs_view_controller/api/securityclass/search/codes</p></td>
<td><p>Security class code list</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>5</td>
<td><p><strong>Sorting</strong></p></td>
<td><p>No need to call API</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="expander-223665290" class="expand-container conf-macro output-block" hasbody="true" macro-id="bb7b03ec-082a-4e0b-a331-0a6f9209363d" macro-name="expand">

<div id="expander-control-223665290" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Page content</span>

</div>

<div id="expander-content-223665290" class="expand-content expand-hidden">

<div>

<table>
<colgroup>
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
</colgroup>
<tbody>
<tr>
<th></th>
<th></th>
<th><p><strong>API</strong></p></th>
<th></th>
<th><p><strong>Query</strong></p></th>
<th><p><strong>Bytes-sent</strong></p></th>
<th><p><strong>Time consuming (ms)</strong></p></th>
<th><p><strong>Proposal</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td colspan="7"><p><strong>Branded folder section</strong></p></td>
</tr>
<tr>
<td>2</td>
<td rowspan="3"><p>Get branded folder</p></td>
<td colspan="2"><p>luz_docs_view_controller/api/v2/{tenant-id}/archives/directories/branded</p></td>
<td><p>languageCode=en</p>
<p>language=en</p>
<p>sortField=updatedAt</p>
<p>sortMode=desc</p></td>
<td><p>2</p></td>
<td><p>160</p></td>
<td></td>
</tr>
<tr>
<td>3</td>
<td></td>
<td><p>luz_docs/api/{tenantId}/documents/search?skip-security-classes=false</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>4</td>
<td></td>
<td><p>luztenant_service/sender-configurations/search/{tenantId}</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>5</td>
<td colspan="7"><p><strong>Custom folder section</strong></p></td>
</tr>
<tr>
<td>6</td>
<td rowspan="3"><p>Get custom folder</p></td>
<td colspan="2"><p>luz_docs_view_controller/api/{tenantId}/archives/directories/search</p></td>
<td><p>languageCode=en</p>
<p>language=en</p>
<p>value=</p>
<p>sortField=_updatedDate</p>
<p>sortMode=desc</p>
<p>isWithTrash=true</p>
<p>parent-directory-id=undefined</p></td>
<td><p>838</p></td>
<td><p>542</p></td>
<td></td>
</tr>
<tr>
<td>7</td>
<td></td>
<td><p>luz_docs/api/folders/search</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>8</td>
<td></td>
<td><p>luz_docs/api/documents/search</p></td>
<td><p>SearchDeletedLettersQuery</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>9</td>
<td colspan="7"><p><strong>Documnets section</strong></p></td>
</tr>
<tr>
<td>10</td>
<td rowspan="2"><p>Check if tenant has letter for <strong>Archived outside folders</strong> tab</p></td>
<td colspan="2"><p>luz_docs_view_controller/api/v2/{tenant-id}/letters/search</p></td>
<td><p>languageCode=en</p>
<p>language=en</p>
<p>excludes=readHistoryEntries.atTime</p>
<p>excludes=readHistoryEntries.byUser</p>
<p>excludes=_files.referenceTsq&amp;excludes=_files.thumbnail512</p>
<p>excludes=_files.thumbnail256</p>
<p>excludes=uploadedHistoryEntry</p>
<p>excludes=storageHistoryEntries</p>
<p>excludes=printHistoryEntries</p>
<p>excludes=downloadHistoryEntries&amp;soft-excludes=readHistoryEntries</p>
<p>value=</p>
<p><strong>isStored=true</strong></p>
<p><strong>hasRootStorage=true</strong></p>
<p><strong>hasDirectoryIds=true</strong></p>
<p>onlyList=false</p>
<p><strong>from=0</strong></p>
<p><strong>size=1</strong></p>
<p>sortField=_createdDate</p>
<p>sortMode=DESC</p></td>
<td><p>2350</p></td>
<td><p>165</p></td>
<td></td>
</tr>
<tr>
<td>11</td>
<td></td>
<td><p>luz_docs/api/documents/search</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>12</td>
<td rowspan="2"><p>Check if tenant has letter for <strong>Archived inside folders</strong> tab</p></td>
<td colspan="2"><p>luz_docs_view_controller/api/v2/{tenant-id}/letters/search</p></td>
<td><p>languageCode=en</p>
<p>language=en</p>
<p>excludes=readHistoryEntries.atTime</p>
<p>excludes=readHistoryEntries.byUser</p>
<p>excludes=_files.referenceTsq</p>
<p>excludes=_files.thumbnail512</p>
<p>excludes=_files.thumbnail256</p>
<p>excludes=uploadedHistoryEntry</p>
<p>excludes=storageHistoryEntries</p>
<p>excludes=printHistoryEntries</p>
<p>excludes=downloadHistoryEntries&amp;soft-excludes=readHistoryEntries</p>
<p>value=</p>
<p><strong>isStored=true</strong></p>
<p><strong>hasRootStorage=true</strong></p>
<p><strong>hasDirectoryIds=false</strong></p>
<p>onlyList=false</p>
<p><strong>from=0</strong></p>
<p><strong>size=1</strong></p>
<p>sortField=_createdDatesortMode=DESC</p></td>
<td><p>2325</p></td>
<td><p>206</p></td>
<td></td>
</tr>
<tr>
<td>13</td>
<td></td>
<td><p>luz_docs/api/documents/search</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>14</td>
<td rowspan="2"><p>Documents stored outside folder list:</p></td>
<td colspan="2"><p>luz_docs_view_controller/api/v2/{tenantId}/letters/search</p></td>
<td><p>languageCode=en</p>
<p>language=en</p>
<p>excludes=readHistoryEntries.atTime</p>
<p>excludes=readHistoryEntries.byUser</p>
<p>excludes=_files.referenceTsq</p>
<p>excludes=_files.thumbnail512</p>
<p>excludes=_files.thumbnail256</p>
<p>excludes=uploadedHistoryEntry</p>
<p>excludes=storageHistoryEntries</p>
<p>excludes=printHistoryEntries</p>
<p>excludes=downloadHistoryEntries</p>
<p>soft-excludes=readHistoryEntries</p>
<p>value=</p>
<p>isStored=true</p>
<p>hasRootStorage=true</p>
<p>hasDirectoryIds=false</p>
<p>onlyList=false&amp;from=0</p>
<p>size=48</p>
<p>sortField=_createdDate</p>
<p>sortMode=DESC</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>15</td>
<td></td>
<td><p>luz_docs/api/documents/search</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

## 4. When eArchive unsubscribed

### 4.1. Inside subscription until

- The same with subscribed eArchive

### 4.2. Inside grace period

#### a. eArchive basic

#### a. eArchive plus

## 5. Reference

LOG:

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="d1a7a114-d68e-4736-bdda-0b73d199467a" macro-name="view-file"><a href="../_attachments/47150006413-performance_log_prod_21_07_2022.csv" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47150006413/performance_log_prod_21_07_2022.csv?version=1&amp;modificationDate=1658721596687&amp;cacheVersion=1&amp;api=v2" data-mime-type="text/csv" data-has-thumbnail="true">

![[47150006413-performance_log_prod_21_07_2022.csv]]

</a></span>

%% ai-graph-start %%

**Related notes:**
- [[EArchive - Search doc process]]
- [[WIP Analyze subfolder search API (542ms)]]
- [[Performance Issue Slow Document Listing Query in MongoDB - eArchive page]]
- [[eArchive – Reproduce performance issue and understand the issue on DEV]]
- [[Public API client performance analysis]]

%% ai-graph-end %%