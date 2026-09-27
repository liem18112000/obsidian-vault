---
ai_hash: c5289c425c64c56b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 3
depth: 2.65
entities: []
relevance: 0.738
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47127396682/LUZ-79226+PayPal+in+Online+Shop+-+Adaption+KLARA+Pay+-+Gateway+API
space: TS
status: reference
tags:
- confluence
- programming
- space/ts
title: 'LUZ-79226: PayPal in Online Shop - Adaption KLARA Pay - Gateway API'
topic: programming
type: source
updated: 2022-06-23
---

# LUZ-79226: PayPal in Online Shop - Adaption KLARA Pay - Gateway API

> [!info] Imported from Confluence
> Space **TS** · updated 2022-06-23 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47127396682/LUZ-79226+PayPal+in+Online+Shop+-+Adaption+KLARA+Pay+-+Gateway+API)
> Relevance 0.738 · topic `programming`

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47127396682_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-79226" macro-id="4f74f545-d464-40dd-af02-703b7a2044b5" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-79226" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-79226</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>


![[47127396682-image-20220614-090208.png]]



## **1. TEST REPORT (14/06/2022 16:05, DEV)**

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
<th><p><strong>No.</strong></p></th>
<th><p><strong>Check list</strong></p></th>
<th><p><strong>Attachment (URL only)</strong></p></th>
<th><p><strong>Dev</strong></p></th>
<th><p><strong>Package tester</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p>Remove on UI</p></td>
<td>

![[47127396682-image-20220614-090405.png]]

</td>
<td><p>

![[47127396682-check.png]]

</p></td>
<td><p>

![[47127396682-check.png]]

</p></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>Remove object field (email) in relevant modules</p></td>
<td><ul>
<li><p>luz_compensation</p></li>
<li><p>luz_online</p></li>
<li><p>luz_online_web</p></li>
</ul></td>
<td><p>

![[47127396682-check.png]]

</p></td>
<td></td>
</tr>
<tr>
<td><p>3</p></td>
<td><p>Do PayPal transaction normally</p></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

</div>

## **2. CODE REVIEW REPORT  **

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
<th><h3 id="LUZ-79226:PayPalinOnlineShop-AdaptionKLARAPay-GatewayAPI-No."><strong>No.</strong></h3></th>
<th><h3 id="LUZ-79226:PayPalinOnlineShop-AdaptionKLARAPay-GatewayAPI-REVIEWLOGIC"><strong>REVIEW LOGIC</strong></h3></th>
<th><h3 id="LUZ-79226:PayPalinOnlineShop-AdaptionKLARAPay-GatewayAPI-Passed?"><strong>Passed?</strong></h3></th>
<th><h3 id="LUZ-79226:PayPalinOnlineShop-AdaptionKLARAPay-GatewayAPI-Explanation(textorcapturedimage)"><strong>Explanation (</strong><em>text or captured image</em><strong>)</strong></h3></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><h3 id="LUZ-79226:PayPalinOnlineShop-AdaptionKLARAPay-GatewayAPI-HavecoveredJunittests?"><strong>Have covered Junit tests?</strong></h3>
<p>(<em>check possible cases are covered by Junit test</em>)</p></td>
<td><p>

![[47127396682-check.png]]

</p></td>
<td></td>
</tr>
<tr>
<td><p>2</p></td>
<td><h3 id="LUZ-79226:PayPalinOnlineShop-AdaptionKLARAPay-GatewayAPI-Havenoside-effectfromthechanges?"><strong>Have no side-effect from the changes?</strong></h3>
<p>(<em>check other places that call to this</em>)</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>3</p></td>
<td><h3 id="LUZ-79226:PayPalinOnlineShop-AdaptionKLARAPay-GatewayAPI-Handlingerrorsiscorrect?"><strong>Handling errors is correct?</strong></h3>
<p>(<em>check NPE, try/catch, validation...</em>)</p></td>
<td><p>

![[47127396682-check.png]]

</p></td>
<td></td>
</tr>
<tr>
<td><p>4</p></td>
<td><h3 id="LUZ-79226:PayPalinOnlineShop-AdaptionKLARAPay-GatewayAPI-Noduplicatedcode?"><strong>No duplicated code?</strong></h3></td>
<td><p>

![[47127396682-check.png]]

</p></td>
<td></td>
</tr>
<tr>
<td></td>
<td colspan="3"><h3 id="LUZ-79226:PayPalinOnlineShop-AdaptionKLARAPay-GatewayAPI-REVIEWPERFORMANCEISSUES"><strong>REVIEW PERFORMANCE ISSUES</strong></h3></td>
</tr>
<tr>
<td><p>5</p></td>
<td><h3 id="LUZ-79226:PayPalinOnlineShop-AdaptionKLARAPay-GatewayAPI-NoN+1issue?"><strong>No N + 1 issue?</strong></h3>
<p>(<em>Check DB &amp; API calls</em>)</p></td>
<td><p>

![[47127396682-check.png]]

</p></td>
<td></td>
</tr>
<tr>
<td><p>6</p></td>
<td><h3 id="LUZ-79226:PayPalinOnlineShop-AdaptionKLARAPay-GatewayAPI-Noduplicatedcalls"><strong>No duplicated calls</strong></h3>
<p>(<em>Check DB &amp; API, method calls</em>)</p></td>
<td><p>

![[47127396682-check.png]]

</p></td>
<td></td>
</tr>
<tr>
<td><p>7</p></td>
<td><h3 id="LUZ-79226:PayPalinOnlineShop-AdaptionKLARAPay-GatewayAPI-Canusecaching?"><strong>Can use caching?</strong></h3>
<p>(<em>Check the data, resource can be cached to improve performance</em>)</p></td>
<td><p>

![[47127396682-check.png]]

</p></td>
<td></td>
</tr>
<tr>
<td><p>8</p></td>
<td><h3 id="LUZ-79226:PayPalinOnlineShop-AdaptionKLARAPay-GatewayAPI-Checkcorrectionofusingbeanscopes"><strong>Check correction of using bean scopes</strong></h3></td>
<td><p>

![[47127396682-check.png]]

</p></td>
<td></td>
</tr>
<tr>
<td></td>
<td colspan="3"><h3 id="LUZ-79226:PayPalinOnlineShop-AdaptionKLARAPay-GatewayAPI-REVIEWCODINGCONVENTION"><strong>REVIEW CODING CONVENTION</strong></h3></td>
</tr>
<tr>
<td><p>9</p></td>
<td><h3 id="LUZ-79226:PayPalinOnlineShop-AdaptionKLARAPay-GatewayAPI-Followednamingconversion"><strong>Followed naming conversion</strong></h3></td>
<td><p>

![[47127396682-check.png]]

</p></td>
<td></td>
</tr>
<tr>
<td><p>10</p></td>
<td><h3 id="LUZ-79226:PayPalinOnlineShop-AdaptionKLARAPay-GatewayAPI-Classes/methodsarewellorganized?"><strong>Classes/methods are well organized?</strong> </h3></td>
<td><p>

![[47127396682-check.png]]

</p></td>
<td></td>
</tr>
<tr>
<td><p>11</p></td>
<td><h3 id="LUZ-79226:PayPalinOnlineShop-AdaptionKLARAPay-GatewayAPI-Class/methodcouldberefactored?"><strong>Class/method could be refactored?</strong></h3></td>
<td><p>

![[47127396682-check.png]]

</p></td>
<td></td>
</tr>
<tr>
<td><p>12</p></td>
<td><h3 id="LUZ-79226:PayPalinOnlineShop-AdaptionKLARAPay-GatewayAPI-Havejava-docforcomplexclass/method/parameter/api?"><strong>Have java-doc for complex class/method/parameter/api?</strong></h3></td>
<td><p>

![[47127396682-check.png]]

</p></td>
<td></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[LUZ-75886 Part 1 Implement real-time API updates]]
- [[LUZ-106177 - Delete credit card expiry reminder]]
- [[Test and code review report template]]
- [[Test and code review report template.2.93]]
- [[LUZ-81908 - Implement test mode without send to the Rhine server]]

%% ai-graph-end %%