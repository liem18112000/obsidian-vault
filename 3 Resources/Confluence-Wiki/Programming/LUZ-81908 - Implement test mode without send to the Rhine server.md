---
ai_hash: 5157d3f3659769bc
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 4
depth: 2.92
entities: []
relevance: 0.798
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47164031007/LUZ-81908+-+Implement+test+mode+without+send+to+the+Rhine+server
space: TS
status: reference
tags:
- confluence
- programming
- space/ts
title: LUZ-81908 - Implement test mode without send to the Rhine server
topic: programming
type: source
updated: 2022-08-12
---

# LUZ-81908 - Implement test mode without send to the Rhine server

> [!info] Imported from Confluence
> Space **TS** · updated 2022-08-12 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47164031007/LUZ-81908+-+Implement+test+mode+without+send+to+the+Rhine+server)
> Relevance 0.798 · topic `programming`

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47164031007_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-81908" macro-id="3c5fccf7-b965-4c3a-adf0-e52e42fdfc29" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-81908" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-81908</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

## **1. TEST REPORT**

[Test-mode and tenant-blacklist](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47161606523/LEGACY+BookingDetail+with+Test-mode+and+tenant-blacklist)

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
<th></th>
<th><h3 id="LUZ-81908-ImplementtestmodewithoutsendtotheRhineserver-REVIEWLOGIC"><strong>REVIEW LOGIC</strong></h3></th>
<th><h3 id="LUZ-81908-ImplementtestmodewithoutsendtotheRhineserver-Passed?"><strong>Passed?</strong></h3></th>
<th><h3 id="LUZ-81908-ImplementtestmodewithoutsendtotheRhineserver-Explanation(textorcapturedimage)"><strong>Explanation (</strong><em>text or captured image</em><strong>)</strong></h3></th>
</tr>
&#10;<tr>
<td>1</td>
<td><h3 id="LUZ-81908-ImplementtestmodewithoutsendtotheRhineserver-HavecoveredJunittests?"><strong>Have covered Junit tests?</strong></h3>
<p>(<em>check possible cases are covered by Junit test</em>)</p></td>
<td><p>

![[47164031007-check.png]]

</p></td>
<td></td>
</tr>
<tr>
<td>2</td>
<td><h3 id="LUZ-81908-ImplementtestmodewithoutsendtotheRhineserver-Handlingerrorsiscorrect?"><strong>Handling errors is correct?</strong></h3>
<p>(<em>check NPE, try/catch, validation...</em>)</p></td>
<td><p>

![[47164031007-check.png]]

</p></td>
<td></td>
</tr>
<tr>
<td>3</td>
<td><h3 id="LUZ-81908-ImplementtestmodewithoutsendtotheRhineserver-Noduplicatedcode?"><strong>No duplicated code?</strong></h3></td>
<td><p>

![[47164031007-error.png]]

</p></td>
<td>

![[47164031007-image-20220812-102046.png]]

</td>
</tr>
<tr>
<td>4</td>
<td colspan="3"><h3 id="LUZ-81908-ImplementtestmodewithoutsendtotheRhineserver-REVIEWPERFORMANCEISSUES"><strong>REVIEW PERFORMANCE ISSUES</strong></h3></td>
</tr>
<tr>
<td>5</td>
<td><h3 id="LUZ-81908-ImplementtestmodewithoutsendtotheRhineserver-NoN+1issue?"><strong>No N + 1 issue?</strong></h3>
<p>(<em>Check DB &amp; API calls</em>)</p></td>
<td><p>

![[47164031007-check.png]]

</p></td>
<td></td>
</tr>
<tr>
<td>6</td>
<td><h3 id="LUZ-81908-ImplementtestmodewithoutsendtotheRhineserver-Noduplicatedcalls"><strong>No duplicated calls</strong></h3>
<p>(<em>Check DB &amp; API, method calls</em>)</p></td>
<td><p>

![[47164031007-check.png]]

</p></td>
<td></td>
</tr>
<tr>
<td>7</td>
<td><h3 id="LUZ-81908-ImplementtestmodewithoutsendtotheRhineserver-Canusecaching?"><strong>Can use caching?</strong></h3>
<p>(<em>Check the data, resource can be cached to improve performance</em>)</p></td>
<td><p>

![[47164031007-check.png]]

</p></td>
<td></td>
</tr>
<tr>
<td>8</td>
<td><h3 id="LUZ-81908-ImplementtestmodewithoutsendtotheRhineserver-Checkcorrectionofusingbeanscopes"><strong>Check correction of using bean scopes</strong></h3></td>
<td><p>

![[47164031007-check.png]]

</p></td>
<td></td>
</tr>
<tr>
<td>9</td>
<td colspan="3"><h3 id="LUZ-81908-ImplementtestmodewithoutsendtotheRhineserver-REVIEWCODINGCONVENTION"><strong>REVIEW CODING CONVENTION</strong></h3></td>
</tr>
<tr>
<td>10</td>
<td><h3 id="LUZ-81908-ImplementtestmodewithoutsendtotheRhineserver-Followednamingconversion"><strong>Followed naming conversion</strong></h3></td>
<td><p>

![[47164031007-check.png]]

</p></td>
<td></td>
</tr>
<tr>
<td>11</td>
<td><h3 id="LUZ-81908-ImplementtestmodewithoutsendtotheRhineserver-Classes/methodsarewellorganized?"><strong>Classes/methods are well organized?</strong> </h3></td>
<td><p>

![[47164031007-check.png]]

</p></td>
<td></td>
</tr>
<tr>
<td>12</td>
<td><h3 id="LUZ-81908-ImplementtestmodewithoutsendtotheRhineserver-Class/methodcouldberefactored?"><strong>Class/method could be refactored?</strong></h3></td>
<td></td>
<td>

![[47164031007-image-20220812-102209.png]]

</td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Test and code review report template]]
- [[LUZ-79226 PayPal in Online Shop - Adaption KLARA Pay - Gateway API]]
- [[LUZ-75886 Part 1 Implement real-time API updates]]
- [[Test and code review report template.2.93]]
- [[Test and code review report template.2]]

%% ai-graph-end %%