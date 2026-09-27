---
title: "[CROSS-TEST] [LUZ-142507] Implement Analyze API Integration (Phase 1) | Part 2"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/48804429825/CROSS-TEST+LUZ-142507+Implement+Analyze+API+Integration+Phase+1+Part+2
space: "TS"
topic: programming
relevance: 0.928
depth: 3
updated: 2025-10-29
attachments: 125
tags:
  - confluence
  - programming
  - space/ts
---

# [CROSS-TEST] [LUZ-142507] Implement Analyze API Integration (Phase 1) | Part 2

> [!info] Imported from Confluence
> Space **TS** · updated 2025-10-29 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/48804429825/CROSS-TEST+LUZ-142507+Implement+Analyze+API+Integration+Phase+1+Part+2)
> Relevance 0.928 · topic `programming`

Related US: <a href="https://axonivy.atlassian.net/browse/LUZ-140089" class="external-link" rel="nofollow">Implement Analyze API Integration (Phase 1) | Part 2</a>

## **1. TEST REPORT**

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
<th></th>
<th><p><strong>Case</strong></p></th>
<th><p><strong>Test steps</strong></p></th>
<th><p><strong>Test data</strong></p></th>
<th><p><strong>Expected result</strong></p></th>
<th><p><strong>Attachment</strong></p></th>
<th><p><strong>Status</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p>Upload a zip file (1 pdf and 1 xml)</p></td>
<td><ol>
<li><p>Go to Upload scanning zip</p></li>
<li><p>Select the zip file you want to upload and wait until the file is submitted successfully</p></li>
<li><p>Wait until the cron job is triggered (30 mins).</p></li>
<li><p>Go to the Scan Deliveries Overview page</p></li>
</ol></td>
<td><ul>
<li><p>Company: <strong>1.Miracle Company</strong></p></li>
<li><p>File: <span class="confluence-embedded-file-wrapper conf-macro output-inline" data-hasbody="false" data-macro-id="ee1a0925-224b-46c4-998e-ccb89115c903" data-macro-name="view-file"><a href="../_attachments/48804429825-thien_individual.zip" class="confluence-embedded-file" data-nice-type="Zip Archive" data-file-src="/wiki/download/attachments/48804429825/thien_individual.zip?version=1&amp;modificationDate=1761726577212&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/zip" data-has-thumbnail="true">

![[48804429825-thien_individual.zip]]

</a></span></p></li>
</ul></td>
<td><ul>
<li><p>The zip file should show in the Scan Deliveries Overview page with status is DELIVERED and document delivered is 1</p></li>
<li><p>The tenant has tenantId in xml file will be received</p></li>
</ul></td>
<td>

![[48804429825-image-20251029-083320.png]]

![[48804429825-image-20251029-083404.png]]

</td>
<td><p>

![[48804429825-check.png]]

</p></td>
</tr>
<tr>
<td>2</td>
<td><p>Upload a zip file with 1 incorrect and 2 correct (3 pdf and 3 xml)</p></td>
<td><ol>
<li><p>Go to Upload scanning zip</p></li>
<li><p>Select the zip file you want to upload and wait until the file is submitted successfully</p></li>
<li><p>Wait until the cron job is triggered (30 mins).</p></li>
<li><p>Go to the Scan Deliveries Overview page</p></li>
</ol></td>
<td><ul>
<li><p>Company: <strong>1.Miracle Company</strong></p></li>
<li><p>File: <span class="confluence-embedded-file-wrapper conf-macro output-inline" data-hasbody="false" data-macro-id="8d47cb9e-c752-439e-9b26-69783a29148a" data-macro-name="view-file"><a href="../_attachments/48804429825-scans.zip" class="confluence-embedded-file" data-nice-type="Zip Archive" data-file-src="/wiki/download/attachments/48804429825/scans.zip?version=1&amp;modificationDate=1761726773861&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/zip" data-has-thumbnail="true">

![[48804429825-scans.zip]]

</a></span></p></li>
</ul></td>
<td><ul>
<li><p>The zip file should show in the Scan Deliveries Overview page with status is DELIVERED and document delivered is 2.</p></li>
<li><p>The tenants have tenantId in xml file will be received</p></li>
<li><p>1 record is not matched and it will show in the Not matches page</p></li>
</ul></td>
<td>

![[48804429825-image-20251029-093225.png]]



![[48804429825-image-20251029-083718.png]]

![[48804429825-image-20251029-083435.png]]

![[48804429825-image-20251029-083542.png]]

</td>
<td><p>

![[48804429825-check.png]]

</p></td>
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
<th><h3 id="id-[CROSS-TEST][LUZ-142507]ImplementAnalyzeAPIIntegration(Phase1)|Part2-No."><strong>No.</strong></h3></th>
<th><h3 id="id-[CROSS-TEST][LUZ-142507]ImplementAnalyzeAPIIntegration(Phase1)|Part2-REVIEWLOGIC"><strong>REVIEW LOGIC</strong></h3></th>
<th><h3 id="id-[CROSS-TEST][LUZ-142507]ImplementAnalyzeAPIIntegration(Phase1)|Part2-Passed?"><strong>Passed?</strong></h3></th>
<th><h3 id="id-[CROSS-TEST][LUZ-142507]ImplementAnalyzeAPIIntegration(Phase1)|Part2-Explanation(textorcapturedimage)"><strong>Explanation (</strong><em>text or captured image</em><strong>)</strong></h3></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><h3 id="id-[CROSS-TEST][LUZ-142507]ImplementAnalyzeAPIIntegration(Phase1)|Part2-HavecoveredJUnittests?"><strong>Have covered JUnit tests?</strong></h3>
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
<td><h3 id="id-[CROSS-TEST][LUZ-142507]ImplementAnalyzeAPIIntegration(Phase1)|Part2-Havenoside-effectfromthechanges?"><strong>Have no side-effect from the changes?</strong></h3>
<h3 id="id-[CROSS-TEST][LUZ-142507]ImplementAnalyzeAPIIntegration(Phase1)|Part2-(checkotherplacesthatcalltothis)">(<em>check other places that call to this</em>)</h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>3</p></td>
<td><h3 id="id-[CROSS-TEST][LUZ-142507]ImplementAnalyzeAPIIntegration(Phase1)|Part2-Handlingerrorsiscorrect?"><strong>Handling errors is correct?</strong></h3>
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
<td><h3 id="id-[CROSS-TEST][LUZ-142507]ImplementAnalyzeAPIIntegration(Phase1)|Part2-Noduplicatedcode?"><strong>No duplicated code?</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>5</p></td>
<td><h3 id="id-[CROSS-TEST][LUZ-142507]ImplementAnalyzeAPIIntegration(Phase1)|Part2-AttachjenkinbuildresultinPR"><strong>Attach jenkin build result in PR</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td></td>
</tr>
<tr>
<td><p>6</p></td>
<td><h3 id="id-[CROSS-TEST][LUZ-142507]ImplementAnalyzeAPIIntegration(Phase1)|Part2-Checkingimpactwithintegrationtest"><strong>Checking impact with integration test</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td></td>
</tr>
<tr>
<td><p>7</p></td>
<td><h3 id="id-[CROSS-TEST][LUZ-142507]ImplementAnalyzeAPIIntegration(Phase1)|Part2-Functioniscorrectpurpose(noneedtosplitfunction)"><strong>Function is correct purpose ( no need to split function)</strong></h3>
<h3 id="id-[CROSS-TEST][LUZ-142507]ImplementAnalyzeAPIIntegration(Phase1)|Part2-Datatypeiscorrect"><strong>Datatype is correct</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td></td>
</tr>
<tr>
<td><p><br />
</p></td>
<td colspan="3"><h3 id="id-[CROSS-TEST][LUZ-142507]ImplementAnalyzeAPIIntegration(Phase1)|Part2-REVIEWPERFORMANCEISSUES"><strong>REVIEW PERFORMANCE ISSUES</strong></h3></td>
</tr>
<tr>
<td><p>8</p></td>
<td><h3 id="id-[CROSS-TEST][LUZ-142507]ImplementAnalyzeAPIIntegration(Phase1)|Part2-NoN+1issue?"><strong>No N + 1 issue?</strong></h3>
<p>(<em>Check DB &amp; API calls</em>)</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>9</p></td>
<td><h3 id="id-[CROSS-TEST][LUZ-142507]ImplementAnalyzeAPIIntegration(Phase1)|Part2-Noduplicatedcalls"><strong>No duplicated calls</strong></h3>
<p>(<em>Check DB &amp; API, method calls</em>)</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>10</p></td>
<td><h3 id="id-[CROSS-TEST][LUZ-142507]ImplementAnalyzeAPIIntegration(Phase1)|Part2-Canusecaching?"><strong>Can use caching?</strong></h3>
<p>(<em>Check the data, resource can be cached to improve performance</em>)</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>11</p></td>
<td><h3 id="id-[CROSS-TEST][LUZ-142507]ImplementAnalyzeAPIIntegration(Phase1)|Part2-Checkcorrectionofusingbeanscopes"><strong>Check correction of using  bean scopes</strong></h3>
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
<td colspan="3"><h3 id="id-[CROSS-TEST][LUZ-142507]ImplementAnalyzeAPIIntegration(Phase1)|Part2-REVIEWCODINGCONVENTION"><strong>REVIEW CODING CONVENTION</strong></h3></td>
</tr>
<tr>
<td><p>12</p></td>
<td><h3 id="id-[CROSS-TEST][LUZ-142507]ImplementAnalyzeAPIIntegration(Phase1)|Part2-Followednamingconversion"><strong>Followed naming conversion</strong></h3>
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
<td><p>13</p></td>
<td><h3 id="id-[CROSS-TEST][LUZ-142507]ImplementAnalyzeAPIIntegration(Phase1)|Part2-Classes/methodsarewellorganized?"><strong>Classes/methods are well organized?</strong> </h3>
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
<td><p>14</p></td>
<td><h3 id="id-[CROSS-TEST][LUZ-142507]ImplementAnalyzeAPIIntegration(Phase1)|Part2-Class/methodcouldberefactored?"><strong>Class/method could be refactored?</strong></h3>
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
<td><p>15</p></td>
<td><h3 id="id-[CROSS-TEST][LUZ-142507]ImplementAnalyzeAPIIntegration(Phase1)|Part2-Havejava-docforcomplexclass/method/parameter/api?"><strong>Have java-doc for complex class/method/parameter/api?</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td></td>
</tr>
</tbody>
</table>

</div>
