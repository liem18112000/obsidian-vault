---
title: "[WIP] Analyze subfolder search API (542ms)"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47150137855/WIP+Analyze+subfolder+search+API+542ms
space: "TP2020"
topic: programming
relevance: 0.863
depth: 3
updated: 2022-07-25
attachments: 4
tags:
  - confluence
  - programming
  - space/tp2020
---

# [WIP] Analyze subfolder search API (542ms)

> [!info] Imported from Confluence
> Space **TP2020** · updated 2022-07-25 · [open original](https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47150137855/WIP+Analyze+subfolder+search+API+542ms)
> Relevance 0.863 · topic `programming`

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
<td>1</td>
<td><p>POST</p>
<p>/luz_docs_view_controller</p></td>
<td></td>
<td><p>/archives/directories/search?languageCode=en&amp;language=en&amp;value=&amp;sortField=_updatedDate&amp;sortMode=desc&amp;isWithTrash=true&amp;parent-directory-id=undefined bytes-sent=838 </p></td>
<td><p><strong>547</strong></p></td>
</tr>
<tr>
<td>2</td>
<td></td>
<td><p>luz_docs</p></td>
<td><p>api/53520800-57de-4f10-8c7d-d3c98741caa8/<strong>documents/search?</strong>skip-security-classes=false HTTP/1.1 status-code=200 bytes-sent=35 </p></td>
<td><p>90</p></td>
</tr>
<tr>
<td>3</td>
<td></td>
<td><p>luz_docs</p></td>
<td><p>api/53520800-57de-4f10-8c7d-d3c98741caa8/<strong>folders/search</strong> HTTP/1.1 status-code=200 bytes-sent=35 </p></td>
<td><p>94</p></td>
</tr>
<tr>
<td>4</td>
<td></td>
<td><p>luz_docs</p></td>
<td><p>api/53520800-57de-4f10-8c7d-d3c98741caa8/<strong>documents/search</strong>?skip-security-classes=false HTTP/1.1 status-code=200 bytes-sent=255 </p></td>
<td><p>57</p></td>
</tr>
<tr>
<td>5</td>
<td></td>
<td><p>luz_docs</p></td>
<td><p>api/53520800-57de-4f10-8c7d-d3c98741caa8/<strong>folders/search</strong> HTTP/1.1 status-code=200 bytes-sent=344 </p></td>
<td><p>96</p></td>
</tr>
<tr>
<td>6</td>
<td></td>
<td><p>luz_docs</p></td>
<td><p>api/53520800-57de-4f10-8c7d-d3c98741caa8/<strong>folders/search</strong> HTTP/1.1 status-code=200 bytes-sent=597 </p></td>
<td><p>127</p></td>
</tr>
</tbody>
</table>

</div>


![[47150137855-image-20220722-105319.png]]

![[47150137855-image-20220722-110208.png]]

![[47150137855-image-20220722-110623.png]]



## Product detail can be skipped (no need to join the data table)


![[47150137855-image-20220725-013126.png]]
