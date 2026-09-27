---
title: "List out places calling booking function"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47220687910/List+out+places+calling+booking+function
space: "LUZ"
topic: programming
relevance: 0.792
depth: 3
updated: 2022-12-14
attachments: 4
tags:
  - confluence
  - programming
  - space/luz
---

# List out places calling booking function

> [!info] Imported from Confluence
> Space **LUZ** · updated 2022-12-14 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47220687910/List+out+places+calling+booking+function)
> Relevance 0.792 · topic `programming`

<div>

<table>
<colgroup>
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
</colgroup>
<tbody>
<tr>
<th></th>
<th><p><strong>API (luz_accounting)</strong></p></th>
<th><p><strong>Middle Module</strong></p></th>
<th><p><strong>Module → Class calling the API</strong></p></th>
<th><p><strong>Business functionality</strong></p></th>
<th><p><strong>Questions</strong></p></th>
<th><p><strong>Tickets</strong></p></th>
<th><p><strong>Assigned Team</strong></p></th>
<th><p><strong>Prio 2: Check in GUI</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p>/accounting-configurations</p></td>
<td></td>
<td><p>luz_finance → AccountingConfigurationController.maintainAccountingConfiguration</p>
<p>luz_Finance → <span class="inline-comment-marker" data-ref="befd4eb7-2b81-4dd4-86be-e4dd1824691d">BusinessYearBean.save</span></p></td>
<td><p>Opening balance sheet</p></td>
<td><p>Is this only affecting the configuration or bookings itself?</p>
<p>Kathrin:<br />
<span class="inline-comment-marker" data-ref="9edc2090-f444-4fa0-bd03-c874713c6b3e">Is that opening balance?</span></p></td>
<td><p><span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47220687910_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-90016" data-macro-id="d1c59769-8587-43cb-b4f2-25ad6fc48648" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-90016" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-90016</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span></p></td>
<td><p>NEXT</p></td>
<td></td>
</tr>
<tr>
<td>2</td>
<td><p>/accounting-configurations/{id}/status</p></td>
<td></td>
<td><p><strong>luz_finance</strong><br />
BusinessYearEndClosingPage.xhtml<br />
-&gt; BusinessYearBean.<span class="inline-comment-marker" data-ref="b951956c-360d-44e3-be8f-a281607d3902">sealBusinessYear</span>(BusinessYear, String)<br />
-&gt; BusinessYearBean --&gt; AccountingConfigurationContainer.<span class="inline-comment-marker" data-ref="ed459760-5b10-4f5f-860c-2e4cc8167e5e">updateStatusAccountingConfiguration</span>(long, FiscalYearStatus, String, Map&lt;String, Object&gt;)</p></td>
<td><p>Year End Closing</p></td>
<td><p>Kathrin: IMO, we have to allow it as long as the last day of the BY is &lt;= last subscription day</p></td>
<td><p><span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47220687910_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-90016" data-macro-id="d545a1ee-521f-4fa2-921c-08379489cb40" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-90016" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-90016</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span></p></td>
<td><p>NEXT</p></td>
<td></td>
</tr>
<tr>
<td>3</td>
<td><p>/vat-clearing-reports/{vatClearingReportId}/state</p></td>
<td></td>
<td><p>luz_finance → VatClearingBean.sealVatClearingReport</p></td>
<td><p>Vat Clearing</p></td>
<td><p>Only if it affects the year 2022 or there is a running/valid subscription</p>
<p>Kathrin: IMO, we have to allow it as long as the booking date is &lt;= last subscription day</p>
<p>It is not just the change between 2022 / 2023 effected</p></td>
<td><p><span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47220687910_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-90016" data-macro-id="9f6e3b0c-31c2-4568-93d1-b6c01a215b08" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-90016" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-90016</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span></p></td>
<td><p>NEXT</p></td>
<td></td>
</tr>
<tr>
<td>4</td>
<td><p>/bookings</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>5</td>
<td><p>/manual-bookings</p></td>
<td></td>
<td><p>luz_finance → ManualBookingProcessing.book</p></td>
<td><p>Manual booking</p></td>
<td><p>Should we prevent user create the booking when the booking date in 2023 immediately? Don’t need to way until receiving the error from the service?</p>
<p>→ For now just block the booking at the end. Probably we will introduce a warning later.</p></td>
<td><p><span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47220687910_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-90016" data-macro-id="b6365818-391a-493e-9786-eeeb71c6e1fa" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-90016" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-90016</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span></p></td>
<td><p>NEXT</p></td>
<td><p>Booking Date</p></td>
</tr>
<tr>
<td>6</td>
<td><p>/auto-bookings/reconciliations/case-12</p></td>
<td></td>
<td><p>luz_finance → BookingTemplatesBean.bookAndReconcileNotSave</p>
<p>luz_finance → BookingTemplatesBean.bookNow</p>
<p>luz_finance → BookingTemplatesBean.bookReconcileAndSave</p></td>
<td><p>Booking Template</p></td>
<td><p>Kathrin: IMO, we have to allow the usage of booking templates as long as the booking date is &lt;= last subscription day</p></td>
<td><p><span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47220687910_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-90016" data-macro-id="5e522e64-87b9-4c4f-9445-6f81437129a1" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-90016" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-90016</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span></p></td>
<td><p>NEXT</p></td>
<td></td>
</tr>
<tr>
<td>7</td>
<td><p>/auto-bookings/salary-run</p></td>
<td><p>luz_compensation → SalaryRunService.triggerAutoBookSalaryRuns</p></td>
<td></td>
<td><p>Salary run</p></td>
<td><p>Kathrin: For Salary bookings we will use the <strong>“Ignore_unsubscripted_dates”:true</strong> and core accounting will ignore all bookings for the unsubscripted time period</p></td>
<td><p><span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47220687910_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-89915" data-macro-id="86f64902-c7bc-45b4-9d17-d185d6f3cae0" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-89915" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-89915</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span></p></td>
<td><p>WOW</p></td>
<td><p>Addaptions in salary run 2.0</p></td>
</tr>
<tr>
<td>8</td>
<td><p>/auto-bookings/cash-diff-transactions</p></td>
<td></td>
<td><p><span class="inline-comment-marker" data-ref="955fe2b5-418d-4e8b-8212-a266ce7114de">luz_pos</span> → AutoBookingRestClientService.bookForCashDiffTransactions</p></td>
<td><p>POS booking for cash check</p></td>
<td></td>
<td></td>
<td><p>NEXT/Helios</p></td>
<td><p>Introduce new status in transaction log</p></td>
</tr>
<tr>
<td>9</td>
<td><p>/auto-bookings/cash-flow-transactions</p></td>
<td></td>
<td><p>luz_pos → AutoBookingRestClientService.bookForCashFlowTransactions</p></td>
<td><p>POS booking for cashflow transactions</p></td>
<td></td>
<td></td>
<td><p>NEXT/Helios</p></td>
<td><p>Introduce new status in transaction log</p></td>
</tr>
<tr>
<td>10</td>
<td><p>/auto-bookings/sale-transactions</p></td>
<td></td>
<td><p>luz_pos → AutoBookingRestClientService.bookForSaleTransactions</p></td>
<td><p>POS booking for sales transactions</p></td>
<td></td>
<td></td>
<td><p>NEXT/Helios</p></td>
<td><p>Introduce new status in transaction log</p></td>
</tr>
<tr>
<td>11</td>
<td><p>/auto-bookings/inventory-transactions</p></td>
<td></td>
<td><p>l<span class="inline-comment-marker" data-ref="4816b459-b3e8-4411-9e5f-adfc9910976e">uz_article_web → InventoryTransactionPostingHandler.executePostingTransaction</span></p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>12</td>
<td><p>/business-cases</p></td>
<td></td>
<td><p>luz_docs_process → BusinessCaseController.approveBusinessCase</p>
<p>luz_docs_process → BusinessCaseController.bookBusinessCase</p>
<p>luz_docs_process → AmountItemViewHandler.book</p></td>
<td><p>Booking workflow</p></td>
<td><p>Should we prevent user create the booking when the booking date in 2023 immediately? Don’t need to way until receiving the error from the service.</p></td>
<td><p><span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47220687910_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-90016" data-macro-id="9f8a7c01-959a-4f75-bdf6-96450df7fa0e" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-90016" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-90016</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span></p></td>
<td><p>NEXT</p></td>
<td><p>Booking Date</p></td>
</tr>
<tr>
<td>13</td>
<td><p>/partner-reconciliation</p></td>
<td></td>
<td><p>luz_finance → PartnerReconciliationDetailBean.reconciliePartner</p>
<p>luz_finance → PartnerReconciliationDetailBean.reconciliateForCashAccount</p></td>
<td><p>Partner reconciliation</p></td>
<td><p>Kathrin: IMO, we have to allow it as long as the booking date is &lt;= last subscription day</p></td>
<td><p><span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47220687910_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-90016" data-macro-id="8107efb8-d0fe-47eb-81b1-32a7b40b608f" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-90016" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-90016</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span></p></td>
<td><p>NEXT</p></td>
<td></td>
</tr>
<tr>
<td>14</td>
<td><p>/bank-reconciliation</p></td>
<td></td>
<td><p>luz_finance → BankReconciliationController.confirmReconciliation</p>
<p>luz_finance → BankReconciliationController.submitAllAutoReconciliation</p></td>
<td><p>Bank reconciliation</p></td>
<td><p>Kathrin: IMO, we have to allow it as long as the booking date is &lt;= last subscription day</p></td>
<td><p><span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47220687910_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-90016" data-macro-id="3613bf1f-696f-4b86-85cc-025d497cf452" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-90016" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-90016</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span></p></td>
<td><p>NEXT</p></td>
<td><p>maybe in the selection of the transactions</p>

![[47220687910-image-20221208-123513.png]]

</td>
</tr>
<tr>
<td>15</td>
<td><p>/auto-bookings/credit-card-transaction</p></td>
<td></td>
<td><p>luz_finance → SwissBankersTransferMoneyBean.<span class="inline-comment-marker" data-ref="9568ef0c-0774-4f00-ba2c-ad95a6cb9e2a">doBookingForMoneyTransfer</span></p></td>
<td><p>CreditCardManualReconciliationMatching</p></td>
<td><p>Kathrin: IMO, we have to allow it as long as the booking date is &lt;= last subscription day</p></td>
<td><p><span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47220687910_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-90016" data-macro-id="8362a44b-4d31-4c65-8266-91405b0dd3ea" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-90016" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-90016</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span></p></td>
<td><p>NEXT</p></td>
<td></td>
</tr>
<tr>
<td>16</td>
<td><p>/auto-bookings/online-order</p></td>
<td></td>
<td><p><span class="inline-comment-marker" data-ref="c1144495-e428-4798-9940-8d2b0bf11a91">luz_online →TransactionService.handleDebitTransaction</span></p></td>
<td></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/people/60d99658a3de4a006b64737f?ref=confluence" class="confluence-userlink user-mention" data-account-id="60d99658a3de4a006b64737f" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Chris Sutter</a> , <a href="https://axonivy.atlassian.net/wiki/people/557058:28a52c28-55d6-4bd9-b219-266037376661?ref=confluence" class="confluence-userlink user-mention" data-account-id="557058:28a52c28-55d6-4bd9-b219-266037376661" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Nam Ha [Next]</a> , <a href="https://axonivy.atlassian.net/wiki/people/557058:03c63c48-df0c-431f-ade0-75431c014051?ref=confluence" class="confluence-userlink user-mention" data-account-id="557058:03c63c48-df0c-431f-ade0-75431c014051" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">thong.nguyen</a>, <a href="https://axonivy.atlassian.net/wiki/people/605414fe66c87900683d008b?ref=confluence" class="confluence-userlink user-mention" data-account-id="605414fe66c87900683d008b" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Gianfranco Gaio</a> fyi</p>
<p>Alex: This service is used if an invoice for credit card payments is created out of online shop. Because first the credit card payment has to be veryfied, it is another service than used for a “normal” OM invoice. So this service has also to validate the subscription of accounting widget.</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>17</td>
<td><p>/auto-bookings/invoice</p></td>
<td rowspan="2"><p>luzfin_finance → RecurringInvoiceRunService.saveAndSendInvoiceFromBasicInfo</p>
<p>luzfin_finance → RecurringInvoiceRunService.smartDeliveryInvoices</p>
<p>luzfin_finance → InvoiceService.saveInvoice/InvoiceService.updateInvoice</p></td>
<td><p>luz_pos → LuzfinFinanceRestClient.createOmInvoice</p>
<p>luz_finance → DeliverableInvoiceHandler.smartDeliveryInvoice</p></td>
<td></td>
<td></td>
<td><p>Prio 3</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td>18</td>
<td><p>/auto-bookings/online-op-payment</p></td>
<td><p>luz_finance → InvoiceController.createInvoice</p></td>
<td><p>Order mgmt</p></td>
<td><p>clarify</p></td>
<td><p><span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47220687910_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-89911" data-macro-id="74840f0e-196a-464f-a478-4bbcd0f0d1ad" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-89911" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-89911</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span></p></td>
<td></td>
<td><p>Invoice Date</p>
<p>How long do we desicde to book?</p></td>
</tr>
<tr>
<td>19</td>
<td><p>/auto-bookings/transfer-money-to-card</p></td>
<td></td>
<td><p>luz_finance →CreditCardAddMoneyViewHandler.handleAddMoney</p></td>
<td></td>
<td><p>Kathrin: This case has prio 3. At the moment we can ignore it because we do not need to solve this urgently as currently no construct is possible to use swissbankers cards without accounting</p></td>
<td><p><span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47220687910_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-90016" data-macro-id="694bf336-ff20-4eba-92cd-96e9e8c0eafe" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-90016" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-90016</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span></p></td>
<td><p>NEXT</p></td>
<td></td>
</tr>
<tr>
<td>20</td>
<td><p>/auto-bookings/depreciation-event</p></td>
<td><p>luz_asset → AccountingRestClientService.createBookings</p></td>
<td><p>luz_asset_web → DepreciationViewHandler.processAction</p></td>
<td><ul>
<li><p>Depreciation booking</p></li>
<li><p>Sell Off Asset</p></li>
</ul></td>
<td><p>Kathrin: IMO, we have to allow it as long as the booking date is &lt;= last subscription day</p>
<p><a href="https://axonivy.atlassian.net/wiki/people/60d99658a3de4a006b64737f?ref=confluence" class="confluence-userlink user-mention" data-account-id="60d99658a3de4a006b64737f" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Chris Sutter</a> Batch job!</p></td>
<td><p><span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47220687910_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-90016" data-macro-id="7a6273f7-59b7-4c5b-876a-4fd2158c1fb7" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-90016" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-90016</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span></p></td>
<td><p>NEXT</p></td>
<td></td>
</tr>
<tr>
<td>21</td>
<td><p>/business-cases/business-cases-with-scripted-data</p></td>
<td></td>
<td><p>luzfin_script</p>
<p>luz_deploy_dev</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>22</td>
<td><p>/bookings/{id}/documents</p></td>
<td></td>
<td><p>luz_finance → MainJournalBookingFileUploadViewHandler.saveReceiptToBooking</p></td>
<td><p>Update documentId and documentDate for booking</p></td>
<td><p>Kathrin: <a href="https://axonivy.atlassian.net/wiki/people/60d99658a3de4a006b64737f?ref=confluence" class="confluence-userlink user-mention" data-account-id="60d99658a3de4a006b64737f" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Chris Sutter</a> Do we book with this function?</p>
<p>Team Next: <a href="https://axonivy.atlassian.net/wiki/people/5a3b1db06bfb7e348845e2d6?ref=confluence" class="confluence-userlink user-mention" data-account-id="5a3b1db06bfb7e348845e2d6" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Katharina Hunkeler-Merz</a> <span class="inline-comment-marker" data-ref="30a351a2-6739-4053-89d9-c38c58a09e28">No, the booking is existing. It just update the document Id and document date in the booking header.</span></p></td>
<td></td>
<td><p>Next</p></td>
<td></td>
</tr>
<tr>
<td>23</td>
<td><p>/bookings/{id}/booking-template</p></td>
<td></td>
<td><p>luz_finance → BookingWithTemplateCreation.book</p>
<p>luz_finance → BookingTemplateManagementBase.create</p></td>
<td><p>Booking template</p></td>
<td><p>Kathrin: IMO, we have to allow it as long as the booking date is &lt;= last subscription day</p></td>
<td><p><span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47220687910_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-90016" data-macro-id="c5701484-c8ce-4501-8fe0-e856ecfb775d" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-90016" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-90016</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span></p></td>
<td><p>NEXT</p></td>
<td></td>
</tr>
<tr>
<td>24</td>
<td></td>
<td></td>
<td></td>
<td><p>Delimitation</p></td>
<td><p>For booking in 2022 with period in 2022 and 2023, should only delimited bookings in 2022 be created?</p>
<p>Kathrin:<br />
<span class="inline-comment-marker" data-ref="3a5c594c-3d16-4d2c-bea9-7eee09695583">Scenario 1: we do not allow any booking if the main booking or one or more of the delimitation bookings is in an unsubscribed period</span></p>
<p>Scenario 2: We book de delimitation-part for the unsubscribed period in the last allowed delimitation booking (the opposite of sealed business year)</p></td>
<td></td>
<td><p>NEXT</p></td>
<td></td>
</tr>
</tbody>
</table>

</div>
