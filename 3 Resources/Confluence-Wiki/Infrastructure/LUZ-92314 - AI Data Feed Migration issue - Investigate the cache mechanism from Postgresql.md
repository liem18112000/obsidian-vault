---
title: "LUZ-92314 - [AI Data Feed] [Migration issue] - Investigate the cache mechanism from Postgresql"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47317287027/LUZ-92314+-+AI+Data+Feed+Migration+issue+-+Investigate+the+cache+mechanism+from+Postgresql
space: "TS"
topic: infra
relevance: 0.874
depth: 3
updated: 2023-03-15
attachments: 10
tags:
  - confluence
  - infra
  - space/ts
---

# LUZ-92314 - [AI Data Feed] [Migration issue] - Investigate the cache mechanism from Postgresql

> [!info] Imported from Confluence
> Space **TS** · updated 2023-03-15 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47317287027/LUZ-92314+-+AI+Data+Feed+Migration+issue+-+Investigate+the+cache+mechanism+from+Postgresql)
> Relevance 0.874 · topic `infra`

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47317287027_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-92314" macro-id="25cc9184-f62f-412c-926f-6067810b887a" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-92314" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-92314</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

## **1. TEST REPORT**

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
<th><p><strong>Case</strong></p></th>
<th><p><strong>Test steps</strong></p></th>
<th><p><strong>Expected result</strong></p></th>
<th><p><strong>Crosstest</strong></p></th>
<th><p><strong>Staging</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p>2 jobs at a time</p></td>
<td><ul>
<li><p>trigger 2 BookingDetail jobs</p></li>
</ul></td>
<td rowspan="2"><ul>
<li><p>still access Klara normally</p></li>
</ul></td>
<td><p>

![[47317287027-check.png]]

</p></td>
<td><p>

![[47317287027-warning.png]]

</p></td>
</tr>
<tr>
<td>2</td>
<td><p>10 jobs at a time</p></td>
<td><ul>
<li><p>trigger 10 jobs</p></li>
</ul></td>
<td><p>

![[47317287027-error.png]]

</p>

![[47317287027-image-20230306-105705.png]]


<p>container was restarted due to high memory usage<br />
<a href="https://console.cloud.google.com/logs/query;cursorTimestamp=2023-03-06T09:01:11.085755279Z;query=resource.type%3D%22k8s_container%22%0Aresource.labels.project_id%3D%22klara-nonprod%22%0Aresource.labels.location%3D%22europe-west6-a%22%0Aresource.labels.cluster_name%3D%22klara-nonprod%22%0Aresource.labels.namespace_name%3D%22dev%22%0Alabels.k8s-pod%2Fapp%3D%22luz-accounting%22%20severity%3E%3DDEFAULT%0Atimestamp%3D%222023-03-06T09:01:09.920153781Z%22%0AinsertId%3D%223bdazyh6lzyrwqlp%22;timeRange=2023-03-06T09:01:00.000Z%2F2023-03-06T09:01:00.000Z--PT1M?project=klara-nonprod" class="external-link" rel="nofollow">https://console.cloud.google.com/logs/query;cursorTimestamp=2023-03-06T09:01:11.085755279Z;query=resource.type%3D"k8s_container" resource.labels.project_id%3D"klara-nonprod" resource.labels.location%3D"europe-west6-a" resource.labels.cluster_name%3D"klara-nonprod" resource.labels.namespace_name%3D"dev" labels.k8s-pod%2Fapp%3D"luz-accounting" severity&gt;%3DDEFAULT timestamp%3D"2023-03-06T09:01:09.920153781Z" insertId%3D"3bdazyh6lzyrwqlp";timeRange=2023-03-06T09:01:00.000Z%2F2023-03-06T09:01:00.000Z--PT1M?project=klara-nonprod</a></p></td>
<td><p>

![[47317287027-warning.png]]

 Memory was not released after bulk job finished</p></td>
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
<th><h3 id="LUZ-92314-[AIDataFeed][Migrationissue]-InvestigatethecachemechanismfromPostgresql-No."><strong>No.</strong></h3></th>
<th><h3 id="LUZ-92314-[AIDataFeed][Migrationissue]-InvestigatethecachemechanismfromPostgresql-REVIEWLOGIC"><strong>REVIEW LOGIC</strong></h3></th>
<th><h3 id="LUZ-92314-[AIDataFeed][Migrationissue]-InvestigatethecachemechanismfromPostgresql-Passed?"><strong>Passed?</strong></h3></th>
<th><h3 id="LUZ-92314-[AIDataFeed][Migrationissue]-InvestigatethecachemechanismfromPostgresql-Explanation(textorcapturedimage)"><strong>Explanation (</strong><em>text or captured image</em><strong>)</strong></h3></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><h3 id="LUZ-92314-[AIDataFeed][Migrationissue]-InvestigatethecachemechanismfromPostgresql-HavecoveredJUnittests?"><strong>Have covered JUnit tests?</strong></h3>
<p>(<em>check possible cases are coverage by JUnit test</em>)</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td></td>
</tr>
<tr>
<td><p>2</p></td>
<td><h3 id="LUZ-92314-[AIDataFeed][Migrationissue]-InvestigatethecachemechanismfromPostgresql-Havenoside-effectfromthechanges?"><strong>Have no side-effect from the changes?</strong></h3>
<h3 id="LUZ-92314-[AIDataFeed][Migrationissue]-InvestigatethecachemechanismfromPostgresql-(checkotherplacesthatcalltothis)">(<em>check other places that call to this</em>)</h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td></td>
</tr>
<tr>
<td><p>3</p></td>
<td><h3 id="LUZ-92314-[AIDataFeed][Migrationissue]-InvestigatethecachemechanismfromPostgresql-Handlingerrorsiscorrect?"><strong>Handling errors is correct?</strong></h3>
<p>(<em>check NPE, try/catch, validate...</em>)</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td></td>
</tr>
<tr>
<td><p>4</p></td>
<td><h3 id="LUZ-92314-[AIDataFeed][Migrationissue]-InvestigatethecachemechanismfromPostgresql-Noduplicatedcode?"><strong>No duplicated code?</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td></td>
</tr>
<tr>
<td><p>5</p></td>
<td><h3 id="LUZ-92314-[AIDataFeed][Migrationissue]-InvestigatethecachemechanismfromPostgresql-AttachjenkinbuildresultinPR"><strong>Attach jenkin build result in PR</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p> </p></td>
</tr>
<tr>
<td><p>6</p></td>
<td><h3 id="LUZ-92314-[AIDataFeed][Migrationissue]-InvestigatethecachemechanismfromPostgresql-Checkingimpactwithintergrationtest"><strong>Checking impact with intergration test</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p> </p></td>
</tr>
<tr>
<td><p>7</p></td>
<td><h3 id="LUZ-92314-[AIDataFeed][Migrationissue]-InvestigatethecachemechanismfromPostgresql-Functioniscorrectpurpose(noneedtosplitfunction)"><strong>Function is correct purpose ( no need to split function)</strong></h3>
<h3 id="LUZ-92314-[AIDataFeed][Migrationissue]-InvestigatethecachemechanismfromPostgresql-Datatypeiscorrect"><strong>Datatype is correct</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p> </p></td>
</tr>
<tr>
<td></td>
<td colspan="3"><h3 id="LUZ-92314-[AIDataFeed][Migrationissue]-InvestigatethecachemechanismfromPostgresql-REVIEWPERFORMANCEISSUES"><strong>REVIEW PERFORMANCE ISSUES</strong></h3></td>
</tr>
<tr>
<td><p>8</p></td>
<td><h3 id="LUZ-92314-[AIDataFeed][Migrationissue]-InvestigatethecachemechanismfromPostgresql-NoN+1issue?"><strong>No N + 1 issue?</strong></h3>
<p>(<em>Check DB &amp; API calls</em>)</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td></td>
</tr>
<tr>
<td><p>9</p></td>
<td><h3 id="LUZ-92314-[AIDataFeed][Migrationissue]-InvestigatethecachemechanismfromPostgresql-Noduplicatedcalls"><strong>No duplicated calls</strong></h3>
<p>(<em>Check DB &amp; API, method calls</em>)</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td></td>
</tr>
<tr>
<td><p>10</p></td>
<td><h3 id="LUZ-92314-[AIDataFeed][Migrationissue]-InvestigatethecachemechanismfromPostgresql-Canusecaching?"><strong>Can use caching?</strong></h3>
<p>(<em>Check the data, resource can be cached to improve performance</em>)</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td></td>
</tr>
<tr>
<td><p>11</p></td>
<td><h3 id="LUZ-92314-[AIDataFeed][Migrationissue]-InvestigatethecachemechanismfromPostgresql-Checkcorrectionofusingbeanscopes"><strong>Check correction of using  bean scopes</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td></td>
</tr>
<tr>
<td></td>
<td colspan="3"><h3 id="LUZ-92314-[AIDataFeed][Migrationissue]-InvestigatethecachemechanismfromPostgresql-REVIEWCODINGCONVENTION"><strong>REVIEW CODING CONVENTION</strong></h3></td>
</tr>
<tr>
<td><p>12</p></td>
<td><h3 id="LUZ-92314-[AIDataFeed][Migrationissue]-InvestigatethecachemechanismfromPostgresql-Followednamingconversion"><strong>Followed naming conversion</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
</ul>
<ul>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td></td>
</tr>
<tr>
<td><p>13</p></td>
<td><h3 id="LUZ-92314-[AIDataFeed][Migrationissue]-InvestigatethecachemechanismfromPostgresql-Classes/methodsarewellorganized?"><strong>Classes/methods are well organized?</strong> </h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td></td>
</tr>
<tr>
<td><p>14</p></td>
<td><h3 id="LUZ-92314-[AIDataFeed][Migrationissue]-InvestigatethecachemechanismfromPostgresql-Class/methodcouldberefactored?"><strong>Class/method could be refactored?</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td></td>
</tr>
<tr>
<td><p>15</p></td>
<td><h3 id="LUZ-92314-[AIDataFeed][Migrationissue]-InvestigatethecachemechanismfromPostgresql-Havejava-docforcomplexclass/method/parameter/api?"><strong>Have java-doc for complex class/method/parameter/api?</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p> </p></td>
</tr>
</tbody>
</table>

</div>
