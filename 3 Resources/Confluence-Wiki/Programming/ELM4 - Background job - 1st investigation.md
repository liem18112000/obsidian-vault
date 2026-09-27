---
ai_hash: bf0c486b3755f04f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 10
depth: 2.41
entities: []
relevance: 0.703
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519732885/ELM4+-+Background+job+-+1st+investigation
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: ELM4 - Background job - 1st investigation
topic: programming
type: source
updated: 2025-03-20
---

# ELM4 - Background job - 1st investigation

> [!info] Imported from Confluence
> Space **LUZ** · updated 2025-03-20 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519732885/ELM4+-+Background+job+-+1st+investigation)
> Relevance 0.703 · topic `programming`

<span class="legacy-color-text-blue1"></span>

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="ebfd304c-c889-447f-a239-a5ec06267ef0" macro-name="toc">

</div>

# <span class="legacy-color-text-blue1">Problem statement</span>

<span class="legacy-color-text-blue1">On the <a href="http://test.klara.tech/" class="external-link" rel="nofollow">http://test.klara.tech/</a> enviroment, we received some error logs from LUZ_ELM. Those are listed in below tickets:</span>

- <span class="legacy-color-text-blue1"> <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_20519732885_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-46179" macro-id="67149101-7111-45c5-92c4-5fba8015d4e6" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-46179" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-46179</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span> </span>
- <span class="legacy-color-text-blue1"> <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_20519732885_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-46181" macro-id="6e3e52d9-2bd3-4b8f-b21b-41d79cb2a843" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-46181" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-46181</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span> </span>

<span class="legacy-color-text-blue1"> The background job function can not be finished because the "java.lang.OutOfMemoryError: Java heap space" exception happened.</span>

# <span class="legacy-color-text-blue1">Testing environment</span>

- TEST enviroment for preproduce the problem.
- PROD enviroment for checking this problem existed.

# <span class="legacy-color-text-blue1">Investigating</span>

## <span class="legacy-color-text-blue1">TEST enviroment</span>

<span class="legacy-color-text-blue1">In the TEST, the time configuration to excute the background job is 10 mins.</span>

<span class="legacy-color-text-blue1">We just have 4 companies registed the ELM background job.</span>

- <span class="legacy-color-text-blue1">1 AHV_AVS AHV1 2018-01-01 29c17569-0e65-48a3-bdd4-55e18d862897 MONTHLY_AHV 2018-02-07 16:37:55  
  </span>
- <span class="legacy-color-text-blue1">1 AHV_AVS AHV1 2017-12-01 1414c78f-7353-4f83-9e05-7937fc4c51b4 MONTHLY_AHV 2018-03-01 16:54:08  
  </span>
- <span class="legacy-color-text-blue1">1 UVG_LAA UVG1 2017-01-01 **a729a76e-7aa9-4f9c-b175-64ba9415cfae** YEARLY 2018-02-07 13:11:45  
  </span>
- <span class="legacy-color-text-blue1">1 AHV_AVS AHV1 2020-12-01 2f61024d-d749-4619-9f01-5b1178ac9f29 YEARLY 2021-01-12 16:35:47</span>

<span class="legacy-color-text-blue1">The "java.lang.OutOfMemoryError: Java heap space" exception happened when call to get status for yearly salary declaration of tenanl **a729a76e-7aa9-4f9c-b175-64ba9415cfae.**</span>

<span class="legacy-color-text-blue1">When we check the database of this tenanl, we see some strange</span>

<span class="legacy-color-text-blue1">

![[20519732885-image2021-1-14_11-2-47.png]]

</span>

<span class="legacy-color-text-blue1">The **salary_declaration** table takes <span class="legacy-color-text-red2">**7.1 MB disk space**.</span></span>

<span class="legacy-color-text-blue1">In this table we have a salary declaration record with id 73</span>


![[20519732885-image2021-1-14_11-7-10.png]]



This <span class="legacy-color-text-blue1">salary declaration:</span>

- <span class="legacy-color-text-blue1">Created at **2018-01-12 13:01:43**</span>
- <span class="legacy-color-text-blue1">The lastest update at **<span class="legacy-color-text-red2">2020-12-10 14:38:28 </span>**→  This is the first time "java.lang.OutOfMemoryError: Java heap space" exception happening and lasting until now.</span>
- <span class="legacy-color-text-blue1">The transmission date is 2018-02-07 13:11:45</span>
- <span class="legacy-color-text-blue1">Has a text column (**notifications column) length is ****71.395361 MB** → **<span class="legacy-color-text-red2">This is the root cause for the problem.</span>**</span>

<span class="legacy-color-text-blue1">In the current implementation, the notifications column is a column with byte is TEXT. This column stores all the notifications included in the response received from Swissdec.  </span>

<span class="legacy-color-text-blue1">We received new a notification from Swissdec → We convert the string from notifications column to a list java instance. → We put the new notification to this list → We convert the list to string and store it to database again.</span>

<span class="legacy-color-text-blue1">Date by date, we background job still runs and get new notification from Swissdec. It will not stop until we reviced a stop event from Swissdec.</span>

<span class="legacy-color-text-blue1">In this case, when we checking the notification from the database, it shows the message "Auf diesem Testsystem sind keine Übermittlungen möglich. Ihre Daten wurden verworfen.\nSur ce système de test pas de transmissions sont possibles. Vos données ont été rejetées.\nSu questo sistema di test effettuati trasferimenti sono possibili. I loro dati sono stati scartati.". It mean that this company name was not registed to the Reffapp → So this one is not the case in PROD.</span>

## PROD enviroment

We send to supporter a script to check the current state of elm background job on PROD [Scripts to measure the time in filemanager and get elm background job information](https://axonivy.atlassian.net/wiki/spaces/NEXT/pages/23207134152/Scripts+to+measure+the+time+in+filemanager+and+get+elm+background+job+information)

This is the result <span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="b5e66dba-d614-480d-8139-da1fa62e2459" macro-name="view-file"><a href="../_attachments/20519732885-elm background job.xlsx" class="confluence-embedded-file" data-nice-type="Microsoft Excel Spreadsheet" data-file-src="/wiki/download/attachments/20519732885/elm%20background%20job.xlsx?version=1&amp;modificationDate=1610602359000&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet" data-has-thumbnail="true">

![[20519732885-elm background job.xlsx]]

</a></span>. In this file, it lists all the current registed background jobs for ELM.


![[20519732885-image2021-1-14_11-35-45.png]]



The last column shows the lenght of notifications string and the max lenght of all row is 0.32137 megabytes so the problem above just happened on TEST.

# More problems

Durring investigating, We find out some problems in ELM background function.

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th>No</th>
<th>Problem</th>
<th><span class="legacy-color-text-blue3">Description</span></th>
</tr>
&#10;<tr>
<td>1</td>
<td>We have some tenanls registed the ELM background job but not has salary declaration event</td>
<td><ul>
<li>s_a127902d_2b79_44a5_9cc4_8f91a231178a</li>
<li>s_ccec0afa_3bd5_413e_8896_81617f0030d3</li>
<li>s_3d95fe65_d22e_447a_a590_73716e29e6d1</li>
<li>s_0f93d2e1_4043_45c2_9402_9ef19311e3fe</li>
<li>s_c7912aec_a392_45b2_b418_876e1f270191</li>
<li>s_4021b2a9_69ca_4be9_9fac_04900c33a649</li>
<li>s_ff8a86ca_0fef_4d0b_a883_e04f831de91c</li>
<li>s_c7912aec_a392_45b2_b418_876e1f270191</li>
</ul></td>
</tr>
<tr>
<td>2</td>
<td><span class="inline-comment-marker" data-ref="a4c35ccb-80b9-41c5-8543-5ae4b080a14f">We have lots of salary declarations still running from the past</span></td>
<td><div>
<table style="letter-spacing: 0.0px;">
<tbody>
<tr>
<th>Year (period)</th>
<th>Count</th>
</tr>
&#10;<tr>
<td>2017</td>
<td>119</td>
</tr>
<tr>
<td>2018</td>
<td>106</td>
</tr>
<tr>
<td>2019</td>
<td>225</td>
</tr>
<tr>
<td>2020</td>
<td>446</td>
</tr>
</tbody>
</table>
</div></td>
</tr>
<tr>
<td>3</td>
<td>We have some companies have same (same domain, institution type,...) valid salary declaration.</td>
<td><div class="content-wrapper">
<p>example with the tenant d36d5f72-05c0-4d71-98c8-53c424b48908</p>

![[20519732885-image2021-1-14_13-48-52.png]]


<p>If there user do salary declaration with the option TEST CASE, we will have this problem.</p>
</div></td>
</tr>
<tr>
<td>4</td>
<td>The background job trigger time doesn't work as expected.</td>
<td><div class="content-wrapper">
<p>In the picture below, the configuration time for the PROD is 6 hours. But the background job runs again after ~ 40 mins.</p>

![[20519732885-image2021-1-14_13-31-42.png]]


</div></td>
</tr>
<tr>
<td>5</td>
<td>The background job for the yearly salary declaration</td>
<td><div class="content-wrapper">
<p>If a company has multi (n) yearly salary declarations with the same period, it will call the Swissdect n x n time</p>

![[20519732885-image2021-1-14_13-39-26.png]]


</div></td>
</tr>
<tr>
<td>6</td>
<td><div class="content-wrapper">
<p>The function to get status when the user login to the company or click the update button doesn't work as expected.</p>
<p><span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_20519732885_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-47178" data-macro-id="2a6126bd-3c97-428e-a1c8-a2fb101e763e" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-47178" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-47178</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span></p>
</div></td>
<td><p>The service use to update the status just support for case yearly.</p>
<p>In the yearly salary declaration, we don't care for the domain and institution type when calling the update status/result service. That mean all the valid salary declarations will be triggered to get the data.</p>
<p>If the user registed for the monthly salary declaration, all the monthly salary declarations in this company be triggered to get the data. We will get the problem like the case number 5 above. (n x n)</p></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[LUZ-92314 - AI Data Feed Migration issue - Investigate the cache mechanism from Postgresql]]
- [[Analyze performance for REST API calculate payslips for overview salary processing]]
- [[eArchive – Reproduce performance issue and understand the issue on DEV]]
- [[Helios myKLARA app(luz-mobile) - API Response Performance Analysis]]
- [[Error Log - Failed to store]]

%% ai-graph-end %%