---
title: "Test and code review report template"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47212822914/Test+and+code+review+report+template
space: "TS"
topic: programming
relevance: 0.818
depth: 3
updated: 2022-11-14
attachments: 2
tags:
  - confluence
  - programming
  - space/ts
---

# Test and code review report template

> [!info] Imported from Confluence
> Space **TS** · updated 2022-11-14 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47212822914/Test+and+code+review+report+template)
> Relevance 0.818 · topic `programming`

## **1. TEST REPORT**

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
<th><p>No.</p></th>
<th><p>Case</p></th>
<th><p>Expected result</p></th>
<th><p><strong>DEV TEST</strong></p></th>
<th><p>Latest Status</p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p>Turn on feature switch<br />
</p></td>
<td><p>Can see the chart in the overview and detail pages.</p>
<p>Can use filter in the overview page</p>
<p>Can use other functionality normally</p></td>
<td><p>

![[47212822914-check.png]]

</p>
<p>

![[47212822914-error.png]]

 category, custom field (because remove address field)</p>
<p>

![[47212822914-check.png]]

</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>Turn off feature switch</p></td>
<td><p>Don’t call to process show charts</p>
<p>Don’t show the charts</p>
<p>Can use filter in the overview page</p>
<p>Can use other functionality normally</p></td>
<td><p>

![[47212822914-check.png]]

</p>
<p>

![[47212822914-check.png]]

</p>
<p>

![[47212822914-error.png]]

 category, custom field (because remove address field)</p>
<p>

![[47212822914-check.png]]

</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
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
<th><h3 id="Testandcodereviewreporttemplate-No."><strong>No.</strong></h3></th>
<th><h3 id="Testandcodereviewreporttemplate-REVIEWLOGIC"><strong>REVIEW LOGIC</strong></h3></th>
<th><h3 id="Testandcodereviewreporttemplate-Passed?"><strong>Passed?</strong></h3></th>
<th><h3 id="Testandcodereviewreporttemplate-Explanation(textorcapturedimage)"><strong>Explanation (</strong><em>text or captured image</em><strong>)</strong></h3></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><h3 id="Testandcodereviewreporttemplate-HavecoveredJUnittests?"><strong>Have covered JUnit tests?</strong></h3>
<p>(<em>check possible cases are coverage by JUnit test</em>)</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>2</p></td>
<td><h3 id="Testandcodereviewreporttemplate-Havenoside-effectfromthechanges?"><strong>Have no side-effect from the changes?</strong></h3>
<h3 id="Testandcodereviewreporttemplate-(checkotherplacesthatcalltothis)">(<em>check other places that call to this</em>)</h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>3</p></td>
<td><h3 id="Testandcodereviewreporttemplate-Handlingerrorsiscorrect?"><strong>Handling errors is correct?</strong></h3>
<p>(<em>check NPE, try/catch, validate...</em>)</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>4</p></td>
<td><h3 id="Testandcodereviewreporttemplate-Noduplicatedcode?"><strong>No duplicated code?</strong></h3>
<p><br />
</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p><br />
</p></td>
<td colspan="3"><h3 id="Testandcodereviewreporttemplate-REVIEWPERFORMANCEISSUES"><strong>REVIEW PERFORMANCE ISSUES</strong></h3></td>
</tr>
<tr>
<td><p>5</p></td>
<td><h3 id="Testandcodereviewreporttemplate-NoN+1issue?"><strong>No N + 1 issue?</strong></h3>
<p>(<em>Check DB &amp; API calls</em>)</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>6</p></td>
<td><h3 id="Testandcodereviewreporttemplate-Noduplicatedcalls"><strong>No duplicated calls</strong></h3>
<p>(<em>Check DB &amp; API, method calls</em>)</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>7</p></td>
<td><h3 id="Testandcodereviewreporttemplate-Canusecaching?"><strong>Can use caching?</strong></h3>
<p>(<em>Check the data, resource can be cached to improve performance</em>)</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>8</p></td>
<td><h3 id="Testandcodereviewreporttemplate-Checkcorrectionofusingbeanscopes"><strong>Check correction of using  bean scopes</strong></h3>
<p><br />
</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p><br />
</p></td>
<td colspan="3"><h3 id="Testandcodereviewreporttemplate-REVIEWCODINGCONVENTION"><strong>REVIEW CODING CONVENTION</strong></h3></td>
</tr>
<tr>
<td><p>9</p></td>
<td><h3 id="Testandcodereviewreporttemplate-Followednamingconversion"><strong>Followed naming conversion</strong></h3>
<p><br />
</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
</ul>
<ul>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>10</p></td>
<td><h3 id="Testandcodereviewreporttemplate-Classes/methodsarewellorganized?"><strong>Classes/methods are well organized?</strong> </h3>
<p><br />
</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>11</p></td>
<td><h3 id="Testandcodereviewreporttemplate-Class/methodcouldberefactored?"><strong>Class/method could be refactored?</strong></h3>
<p><br />
</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>12</p></td>
<td><h3 id="Testandcodereviewreporttemplate-Havejava-docforcomplexclass/method/parameter/api?"><strong>Have java-doc for complex class/method/parameter/api?</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
</tbody>
</table>

</div>
