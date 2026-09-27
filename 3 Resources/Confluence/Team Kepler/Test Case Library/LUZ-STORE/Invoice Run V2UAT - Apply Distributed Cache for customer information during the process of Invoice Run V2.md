---
title: "[Invoice Run V2][UAT] - Apply Distributed Cache for customer information during the process of Invoice Run V2"
created: 2026-02-23
updated: 2026-02-24
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49168220163/Invoice+Run+V2+UAT+-+Apply+Distributed+Cache+for+customer+information+during+the+process+of+Invoice+Run+V2
confluence_id: "49168220163"
confluence_path: "Team Kepler > Test Case Library > LUZ-STORE"
tags: [confluence, invoice-run, luz-store, testing]
---

# [Invoice Run V2][UAT] - Apply Distributed Cache for customer information during the process of Invoice Run V2

*Confluence source · Team Kepler › Test Case Library › LUZ-STORE · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49168220163/Invoice+Run+V2+UAT+-+Apply+Distributed+Cache+for+customer+information+during+the+process+of+Invoice+Run+V2) · updated 2026-02-24*

## Test case Template

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>**Test Case#:** 1</p></td>
<td><p>**Test Case Name:** Apply Distributed Cache for customer information during the process of Invoice Run V2 - Customer has an **Invoice Email** configured</p></td>
</tr>
<tr>
<td><p>**System:** LUZ-STORE</p></td>
<td><p>**Subsystem: LUZ-STORE**</p></td>
</tr>
<tr>
<td><p>**Test case designed by:**<br />
[[Kepler] - Liem Doan](https://axonivy.atlassian.net/wiki/people/712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4?ref=confluence)</p></td>
<td><p>**Design Date:** 23 Feb 2026</p></td>
</tr>
<tr>
<td><p>**Short description: **</p></td>
<td><h2 id="id-[InvoiceRunV2][UAT]-ApplyDistributedCacheforcustomerinformationduringtheprocessofInvoiceRunV2-Shortdescription:" data-local-id="b7358e29-104c-46b6-981b-da07aa96a3c6">Short description:</h2>
<p>The customer receives invoices at the wrong email address. This occurs despite having set an invoice email in their profile.</p>
<p>The issue arises due to caching of the email address. When processing with multiple pods, the incorrect cache may be accessed, leading to the wrong email being used.</p></td>
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
<li><p>`luz-store`run in multi-pod mode (number of pod is greater than 1)</p></li>
<li><p>Invoice Run test data is available.</p></li>
<li><p>Customer has an **Invoice Email** configured</p></li>
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
<li><p>The email is sent to the expected mailbox</p></li>
</ul></td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

### Test Steps

<table>
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
<td><p>Run an Invoice Run V2:</p></td>
<td>![[3 Resources/Confluence/Team Kepler/Test Case Library/LUZ-STORE/attachments/invoice-run-v2uat-apply-distributed-cache-for-customer-infor/image-20260223-103302.png]]![[3 Resources/Confluence/Team Kepler/Test Case Library/LUZ-STORE/attachments/invoice-run-v2uat-apply-distributed-cache-for-customer-infor/image-20260223-104051.png]]</td>
<td></td>
<td></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/LUZ-STORE/attachments/invoice-run-v2uat-apply-distributed-cache-for-customer-infor/check.png]]</p></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/LUZ-STORE/attachments/invoice-run-v2uat-apply-distributed-cache-for-customer-infor/1f6ab.png]]</p></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>Choose your newly create invoice run</p></td>
<td>![[3 Resources/Confluence/Team Kepler/Test Case Library/LUZ-STORE/attachments/invoice-run-v2uat-apply-distributed-cache-for-customer-infor/image-20260223-104215.png]]</td>
<td></td>
<td><p> </p></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/LUZ-STORE/attachments/invoice-run-v2uat-apply-distributed-cache-for-customer-infor/check.png]]</p></td>
<td></td>
</tr>
<tr>
<td><p>3</p></td>
<td><p>The receiver email must be the configured invoice email:</p></td>
<td>![[3 Resources/Confluence/Team Kepler/Test Case Library/LUZ-STORE/attachments/invoice-run-v2uat-apply-distributed-cache-for-customer-infor/image-20260223-104824.png]]![[3 Resources/Confluence/Team Kepler/Test Case Library/LUZ-STORE/attachments/invoice-run-v2uat-apply-distributed-cache-for-customer-infor/image-20260223-111132.png]]</td>
<td></td>
<td></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/LUZ-STORE/attachments/invoice-run-v2uat-apply-distributed-cache-for-customer-infor/check.png]]</p></td>
<td></td>
</tr>
<tr>
<td><p>4</p></td>
<td><p>When run the invoice run v2 last step “Sending invoice“. The invoice must be sent to the invoice mail box.</p></td>
<td>![[3 Resources/Confluence/Team Kepler/Test Case Library/LUZ-STORE/attachments/invoice-run-v2uat-apply-distributed-cache-for-customer-infor/image-20260223-112912.png]]</td>
<td></td>
<td></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/LUZ-STORE/attachments/invoice-run-v2uat-apply-distributed-cache-for-customer-infor/check.png]]</p></td>
<td></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>**Test Case#:** 2</p></td>
<td><p>**Test Case Name:** Apply Distributed Cache for customer information during the process of Invoice Run V2 - Customer has **NO Invoice Email** configured</p></td>
</tr>
<tr>
<td><p>**System:** LUZ-STORE</p></td>
<td><p>**Subsystem: LUZ-STORE**</p></td>
</tr>
<tr>
<td><p>**Test case designed by:**<br />
[[Kepler] - Liem Doan](https://axonivy.atlassian.net/wiki/people/712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4?ref=confluence)</p></td>
<td><p>**Design Date:** 23 Feb 2026</p></td>
</tr>
<tr>
<td><p>**Short description: **</p></td>
<td><h2 id="id-[InvoiceRunV2][UAT]-ApplyDistributedCacheforcustomerinformationduringtheprocessofInvoiceRunV2-Shortdescription:.1" data-local-id="b44eaea79175">Short description:</h2>
<p>The customer receives invoices at the wrong email address. This occurs despite having set an invoice email in their profile.</p>
<p>The issue arises due to caching of the email address. When processing with multiple pods, the incorrect cache may be accessed, leading to the wrong email being used.</p></td>
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
<li><p>`luz-store`run in multi-pod mode (number of pod is greater than 1)</p></li>
<li><p>Invoice Run test data is available.</p></li>
<li><p>Customer has an NO **Invoice Email** configured, the system falls back to the **Contact Email**</p></li>
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
<li><p>The email is sent to the expected mailbox</p></li>
</ul></td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

### Test Steps

<table>
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
<td><p>Run an Invoice Run V2:</p></td>
<td>![[3 Resources/Confluence/Team Kepler/Test Case Library/LUZ-STORE/attachments/invoice-run-v2uat-apply-distributed-cache-for-customer-infor/image-20260223-103302.png]]![[3 Resources/Confluence/Team Kepler/Test Case Library/LUZ-STORE/attachments/invoice-run-v2uat-apply-distributed-cache-for-customer-infor/image-20260223-104051.png]]</td>
<td></td>
<td></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/LUZ-STORE/attachments/invoice-run-v2uat-apply-distributed-cache-for-customer-infor/check.png]]</p></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/LUZ-STORE/attachments/invoice-run-v2uat-apply-distributed-cache-for-customer-infor/1f6ab.png]]</p></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>Choose your newly create invoice run</p></td>
<td>![[3 Resources/Confluence/Team Kepler/Test Case Library/LUZ-STORE/attachments/invoice-run-v2uat-apply-distributed-cache-for-customer-infor/image-20260223-104215.png]]</td>
<td></td>
<td><p> </p></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/LUZ-STORE/attachments/invoice-run-v2uat-apply-distributed-cache-for-customer-infor/check.png]]</p></td>
<td></td>
</tr>
<tr>
<td><p>3</p></td>
<td><p>The receiver email must be the contact email</p></td>
<td>![[3 Resources/Confluence/Team Kepler/Test Case Library/LUZ-STORE/attachments/invoice-run-v2uat-apply-distributed-cache-for-customer-infor/image-20260224-003119.png]]![[3 Resources/Confluence/Team Kepler/Test Case Library/LUZ-STORE/attachments/invoice-run-v2uat-apply-distributed-cache-for-customer-infor/image-20260224-004043.png]]</td>
<td></td>
<td></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/LUZ-STORE/attachments/invoice-run-v2uat-apply-distributed-cache-for-customer-infor/check.png]]</p></td>
<td></td>
</tr>
<tr>
<td><p>4</p></td>
<td><p>When run the invoice run v2 last step “Sending invoice“. The invoice must be sent to the contact mail box.</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>
