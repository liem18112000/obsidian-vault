---
title: "[Invoice Run V2][UAT] - No error when luz-store is running with multiple pods"
created: 2025-12-09
updated: 2026-02-03
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48950280193/Invoice+Run+V2+UAT+-+No+error+when+luz-store+is+running+with+multiple+pods
confluence_id: "48950280193"
confluence_path: "Team Kepler > Test Case Library > LUZ-STORE"
tags: [confluence, invoice-run, luz-store, testing]
---

# [Invoice Run V2][UAT] - No error when luz-store is running with multiple pods

*Confluence source · Team Kepler › Test Case Library › LUZ-STORE · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48950280193/Invoice+Run+V2+UAT+-+No+error+when+luz-store+is+running+with+multiple+pods) · updated 2026-02-03*

## Test case Template

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>**Test Case#:** 1</p></td>
<td><p>**Test Case Name:** luz-store running with multiple pods</p></td>
</tr>
<tr>
<td><p>**System:** LUZ-DOCS</p></td>
<td><p>**Subsystem: LUZ-STORE**</p></td>
</tr>
<tr>
<td><p>**Test case designed by:**<br />
[[Kepler] - Liem Doan](https://axonivy.atlassian.net/wiki/people/712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4?ref=confluence)</p></td>
<td><p>**Design Date:** 24 Sep 2025</p></td>
</tr>
<tr>
<td><p>**Short description: **</p></td>
<td><h2 id="id-[InvoiceRunV2][UAT]-Noerrorwhenluz-storeisrunningwithmultiplepods-Shortdescription:" data-local-id="62e1b4ae-0a58-49cc-8ff6-7634f49aa85b">Short description:</h2>
<p>Verify that when `luz-store` runs with multiple pods, all consuming services (`luz-reporting`, `luz-reporting-dotnet`, `luz-docs-creator-client`, `luzfin-finance`, `luz-online-payment`) can scale and handle the increased request load so that the Invoice Run (including PDF generation) completes successfully and without errors in a multi‑pod environment.</p></td>
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
<li><p>`luz-store` is running in multiple pods</p></li>
<li><p>Billilng table has many records ready for Invoice Run to start</p></li>
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
</ul></td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

### Test Steps

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
<td colspan="3"><p>**Test Case**</p></td>
<td colspan="3"><p>**Test Execution**</p></td>
</tr>
<tr>
<td><p>**Step**</p></td>
<td><p>**Action**</p></td>
<td><p>**Expected Behavior**</p></td>
<td><p>**Actual Behavior**</p></td>
<td><p>**Comment**</p></td>
<td><p>**Status**</p></td>
</tr>
<tr>
<td><p>1</p></td>
<td><p>Make sure luz-store is running in multiple pods</p></td>
<td><p>luz-store is running in 3 pods</p>
![[image-20260129-040333.png]]</td>
<td></td>
<td></td>
<td rowspan="5"><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/LUZ-STORE/attachments/invoice-run-v2uat-no-error-when-luz-store-is-running-with-mu/check.png]]</p>
<p>**Passed**</p></td>
</tr>
<tr>
<td><p>2</p></td>
<td><ul>
<li><p>Start invoice run with many invoice item</p></li>
</ul></td>
<td><p>Starting</p>
![[image-20260129-041722.png]]
<p><br />
</p></td>
<td></td>
<td><p> </p></td>
</tr>
<tr>
<td><p>3</p></td>
<td><ul>
<li><p>Start and finish Calculating step without error</p></li>
</ul></td>
<td>![[image-20260129-070131.png]]
<p><br />
Invoice Run Detail</p>
![[image-20260129-070154.png]]</td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>4</p></td>
<td><ul>
<li><p>Start and finish Calculating and Creating PDF without error</p></li>
</ul></td>
<td>![[image-20260129-071941.png]]
<p>Invoice Run Detail</p>
![[image-20260129-072010.png]]</td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>5</p></td>
<td><ul>
<li><p>Start and finish Sending and Upload Customer document without error</p></li>
</ul></td>
<td>![[image-20260129-073128.png]]
<p><br />
Invoice Run Detail<br />
</p>
![[image-20260129-073112.png]]</td>
<td></td>
<td></td>
</tr>
</tbody>
</table>
