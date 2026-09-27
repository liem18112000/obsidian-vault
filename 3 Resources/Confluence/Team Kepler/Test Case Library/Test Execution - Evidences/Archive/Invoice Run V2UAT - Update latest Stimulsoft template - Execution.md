---
ai_hash: d075430b12393d1e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '48988913665'
confluence_path: Team Kepler > Test Case Library > Test Execution / Evidences > Archive
  > Sprint 146 - Test Report
created: 2025-12-18
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- invoice-run
- sprint
- testing
title: '[Invoice Run V2][UAT] - Update latest Stimulsoft template - Execution'
type: source
updated: 2025-12-19
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48988913665/Invoice+Run+V2+UAT+-+Update+latest+Stimulsoft+template+-+Execution
---

# [Invoice Run V2][UAT] - Update latest Stimulsoft template - Execution

*Confluence source · Team Kepler › Test Case Library › Test Execution / Evidences › Archive › Sprint 146 - Test Report · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48988913665/Invoice+Run+V2+UAT+-+Update+latest+Stimulsoft+template+-+Execution) · updated 2025-12-19*

## Test case

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>**Test Case#:** 1</p></td>
<td><p>**Test Case Name:** Update latest Stimulsoft template</p></td>
</tr>
<tr>
<td><p>**System:** LUZ-DOCS</p></td>
<td><p>**Subsystem: LUZ-STORE**</p></td>
</tr>
<tr>
<td><p>**Test case designed by:**<br />
[[Kepler] - Liem Doan](https://axonivy.atlassian.net/wiki/people/712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4?ref=confluence)</p></td>
<td><p>**Design Date:** 18 Dec 2025</p></td>
</tr>
<tr>
<td><p>**Short description: **</p></td>
<td><h2 id="id-[InvoiceRunV2][UAT]-UpdatelatestStimulsofttemplate-Execution-Shortdescription:" data-local-id="b7358e29-104c-46b6-981b-da07aa96a3c6">Short description:</h2>
<p>We need to apply this update new billing details template so that the next Invoice Run uses the correct template.<br />
[Stimulsoft mrt files](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20474834652/Stimulsoft+mrt+files)</p>
![[template change.png]]</td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td><p>**Pre – conditions:**</p></td>
</tr>
<tr>
<td><ul>
<li><p>`luz-store` and all consuming services are deployed, healthy, and connected.</p></li>
<li><p>Invoice Run test data is available.</p></li>
<li><p>Choosing a invoice run v2 with status “INVOICES_CALCUALTED“ with item quantity is greater than zero</p></li>
</ul></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td><p>**Post – conditions:**</p></td>
</tr>
<tr>
<td><ul>
<li><p>Invoice Run completes without errors.</p></li>
<li><p>No duplicates or missing records; system is stable for the next run</p></li>
<li><p>Generate a PDF invoice in which billing details page show no KLARA Icon or Title with “KLARA“.</p></li>
</ul></td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

### Test Steps

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
<td colspan="3"><p>**Test Case**</p></td>
<td colspan="4"><p>**Test Execution**</p></td>
</tr>
<tr>
<td rowspan="2"><p>**Step**</p></td>
<td rowspan="2"><p>**Action**</p></td>
<td rowspan="2"><p>**Expected Behavior**</p></td>
<td rowspan="2"><p>**Actual Behavior**</p></td>
<td rowspan="2"><p>**Comment**</p></td>
<td colspan="2"><p>**Results**</p></td>
</tr>
<tr>
<td><p>As expected</p></td>
<td><p>not as expected</p></td>
</tr>
<tr>
<td><p>1</p></td>
<td><p>Choose a invoice v2 with status “INVOICES_CALCUALTED“ and view details</p>
![[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Archive/attachments/invoice-run-v2uat-update-latest-stimulsoft-template-executio/image-20251218-021239.png]]</td>
<td><p>It should be like this:</p>
![[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Archive/attachments/invoice-run-v2uat-update-latest-stimulsoft-template-executio/image-20251218-021415.png]]</td>
<td></td>
<td></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Archive/attachments/invoice-run-v2uat-update-latest-stimulsoft-template-executio/check.png]]</p></td>
<td></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>Download PDF and view billing details:</p>
![[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Archive/attachments/invoice-run-v2uat-update-latest-stimulsoft-template-executio/image-20251218-022029.png]]</td>
<td><p>It should be like this:</p>
![[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Archive/attachments/invoice-run-v2uat-update-latest-stimulsoft-template-executio/image-20251218-021819.png]]</td>
<td>![[8f479866-1867-4491-8aa2-0a19132746ad.jpg]]
![[3b90f570-46a7-487d-8834-141a1ec047ed.jpg]]
![[9d196391-2708-43ac-96ec-fc25754374a5.jpg]]</td>
<td><p> </p></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Archive/attachments/invoice-run-v2uat-update-latest-stimulsoft-template-executio/check.png]]</p></td>
<td></td>
</tr>
</tbody>
</table>

%% ai-graph-start %%

**Related notes:**
- [[Invoice Run V2UAT - Update latest Stimulsoft template]]
- [[Invoice Run V2UAT - No error when luz-store is running with multiple pods]]
- [[Invoice Run V2 - Retry Uploaded customer document step - Should update correct status after retry]]
- [[Invoice Run V2UAT - Execution - Change filestore cache implement to filestore_utils]]
- [[Invoice Run V2UATExecute - Prevent error when luz-store is multiple pods]]

%% ai-graph-end %%