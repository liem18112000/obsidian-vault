---
title: "[Invoice Run V2][UAT] - Update latest Stimulsoft template"
created: 2025-12-17
updated: 2025-12-18
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48983965720/Invoice+Run+V2+UAT+-+Update+latest+Stimulsoft+template
confluence_id: "48983965720"
confluence_path: "Team Kepler > Test Case Library > LUZ-STORE"
tags: [confluence, invoice-run, luz-store, testing]
---

# [Invoice Run V2][UAT] - Update latest Stimulsoft template

*Confluence source · Team Kepler › Test Case Library › LUZ-STORE · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48983965720/Invoice+Run+V2+UAT+-+Update+latest+Stimulsoft+template) · updated 2025-12-18*

## Test case Template

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>**Test Case#:** 1</p></td>
<td><p>**Test Case Name:** Test Case Template</p></td>
</tr>
<tr>
<td><p>**System:** LUZ-DOCS</p></td>
<td><p>**Subsystem: LUZ-STORE**</p></td>
</tr>
<tr>
<td><p>**Test case designed by:**<br />
[Alvin Villanueva](https://axonivy.atlassian.net/wiki/people/712020:a8f84630-b701-4aff-b3f4-82349928cfc7?ref=confluence)</p></td>
<td><p>**Design Date:** 24 Sep 2025</p></td>
</tr>
<tr>
<td><p>**Short description: **</p></td>
<td><h2 id="id-[InvoiceRunV2][UAT]-UpdatelatestStimulsofttemplate-Shortdescription:" data-local-id="b7358e29-104c-46b6-981b-da07aa96a3c6">Short description:</h2>
<p>We need to apply this update new billing details template so that the next Invoice Run uses the correct template.<br />
[Stimulsoft mrt files](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20474834652/Stimulsoft+mrt+files)</p></td>
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
![[3 Resources/Confluence/Team Kepler/Test Case Library/LUZ-STORE/attachments/invoice-run-v2uat-update-latest-stimulsoft-template/image-20251218-021239.png]]</td>
<td><p>It should be like this:</p>
![[3 Resources/Confluence/Team Kepler/Test Case Library/LUZ-STORE/attachments/invoice-run-v2uat-update-latest-stimulsoft-template/image-20251218-021415.png]]</td>
<td></td>
<td></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/LUZ-STORE/attachments/invoice-run-v2uat-update-latest-stimulsoft-template/check.png]]</p></td>
<td><p>🚫</p></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>Download PDF and view billing details:</p>
![[3 Resources/Confluence/Team Kepler/Test Case Library/LUZ-STORE/attachments/invoice-run-v2uat-update-latest-stimulsoft-template/image-20251218-022029.png]]</td>
<td><p>It should be like this:</p>
![[3 Resources/Confluence/Team Kepler/Test Case Library/LUZ-STORE/attachments/invoice-run-v2uat-update-latest-stimulsoft-template/image-20251218-021819.png]]</td>
<td></td>
<td><p> </p></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>
