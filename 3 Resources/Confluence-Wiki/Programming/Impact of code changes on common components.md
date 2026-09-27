---
ai_hash: 36115ab7054adc04
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.73
entities: []
relevance: 0.731
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47303492220/Impact+of+code+changes+on+common+components
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: Impact of code changes on common components
topic: programming
type: source
updated: 2023-02-20
---

# Impact of code changes on common components

> [!info] Imported from Confluence
> Space **LUZ** · updated 2023-02-20 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47303492220/Impact+of+code+changes+on+common+components)
> Relevance 0.731 · topic `programming`

## 1. Modified common components

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th></th>
<th><p><strong>File</strong></p></th>
<th><p><strong>Relevant modules</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p>ThumbnailProcessBean.java</p>
<p>getDocumentThumbnail.mod</p>
<p>getDocumentManagementData.ivyClass</p></td>
<td><p>luz_store_web:</p>
<ul>
<li><p>VaudoiseLiabilityWidget: Confirmation.xhtml, Finish.xhtml</p></li>
<li><p>VaudoiseWidget: Confirmation.xhtml, Finish.xhtml</p></li>
</ul>
<p>luz_finance:</p>
<ul>
<li><p>OrderDetailPage: RecurringInvoiceItemView.xhtml</p></li>
<li><p>OrderDocumentViewItem: OrderDocumentViewItem.xhtml</p></li>
<li><p>InvoiceItem: InvoiceItem.xhtml</p></li>
</ul></td>
</tr>
<tr>
<td>2</td>
<td><p>com.axonivy.luz.components.documents.DocumentBean</p></td>
<td><p>luz_finance:</p>
<ul>
<li><p>OrderDetailPage: UploadedFiles.xhtml, UploadFileViewHandler.java</p></li>
</ul></td>
</tr>
<tr>
<td>3</td>
<td><p>BankTransaction.java<br />
PaymentsInitiation.java</p>
<p>Payment.java</p></td>
<td><p>sixcor, bank, card, payment,…</p></td>
</tr>
<tr>
<td>4</td>
<td><p>CommonInvoicePaymentItem.xhtml</p></td>
<td><p>luz_finance:</p>
<ul>
<li><p>sale/order: AvailablePrepayment.xhtml</p></li>
</ul></td>
</tr>
<tr>
<td>5</td>
<td><p>com/axonivy/luz/components/documents/DocumentListView<br />
com/axonivy/luz/components/documents/DocumentUpload<br />
com/axonivy/luz/components/documents/DocumentView<br />
com/axonivy/luz/components/documents/DocumentViewer</p>
<p>com/axonivy/luz/components/documents/DocumentItem</p></td>
<td><p>luz_finance:</p>
<ul>
<li><p>order/bank: OrderDetailPage: UploadedFiles.xhtml</p></li>
<li><p>fin: UploadedDocumentItem.xhtml</p></li>
</ul>
<p>luz_components:</p>
<ul>
<li><p>Company documents view</p></li>
<li><p>Employee documents view</p></li>
</ul>
<p>luz_xhrm_process:</p>
<ul>
<li><p>SalaryPaymentPage( and V2): Step_Employees.xhtml</p></li>
</ul>
<p>luz_web:</p>
<ul>
<li><p>Reports Generator: ReportsGenerator.xhtml</p></li>
<li><p>Household: Dashboard_ContractWorkingTime.xhtml</p></li>
</ul></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Merging process]]
- [[Collect all calls FileManager APIs by Klara Modules]]
- [[Change get API when paying invoices]]
- [[Investigate Analyze the API's which call to FileManager]]
- [[Get article thumbnail API - related modules]]

%% ai-graph-end %%