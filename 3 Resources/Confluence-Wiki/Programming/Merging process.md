---
ai_hash: a920581aa3e1fa0f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.81
entities: []
relevance: 0.724
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47337640302/Merging+process
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: Merging process
topic: programming
type: source
updated: 2023-07-03
---

# Merging process

> [!info] Imported from Confluence
> Space **LUZ** · updated 2023-07-03 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47337640302/Merging+process)
> Relevance 0.724 · topic `programming`

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
<th><p><strong>Function</strong></p></th>
<th><p><strong>Sub-function</strong></p></th>
<th><p><strong>Project</strong></p></th>
<th><p><strong>Class</strong></p></th>
<th><p><strong>Note</strong></p></th>
</tr>
&#10;<tr>
<td rowspan="2"><p>Add column documentId</p></td>
<td rowspan="2"></td>
<td><p>luz_docs_process</p></td>
<td><p>DocumentResource.java<br />
DocumentResourceTest.java</p></td>
<td><p>Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>luz_components</p></td>
<td><p>Constants.java<br />
DocumentCategoryDataServiceFactory.java<br />
DocumentDisplayableServiceFactory.java<br />
DocumentDisplayServiceFactory.java<br />
DocumentServiceFactory.java<br />
ServiceFactory.java</p></td>
<td><p>Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
New</p></td>
</tr>
<tr>
<td><p>Upload Invoice</p></td>
<td></td>
<td><p>luz_components</p></td>
<td><p>CommonThumbnailBean.java<br />
DocumentBean.java<br />
DocumentCategoryDataServiceFactory.java<br />
DocumentDisplay.java<br />
DocumentDisplayableServiceFactory.java<br />
DocumentDisplayServiceFactory.java<br />
DocumentMetadataConstant.java<br />
DocumentServiceDelegate.java<br />
DocumentServiceFactory.java<br />
FileManagerDocumentCategoryDataService.java<br />
FileManagerDocumentDisplayService.java<br />
ICommonDocumentService.java<br />
LuzDocsDocumentCategoryDataService.java<br />
LuzDocsDocumentDisplayableService.java<br />
LuzDocsDocumentDisplayService.java<br />
LuzDocsDocumentService.java</p></td>
<td><p>New<br />
Edit<br />
New<br />
Edit<br />
New<br />
New<br />
Edit<br />
New<br />
New<br />
New<br />
New<br />
New<br />
New<br />
New<br />
New<br />
New</p></td>
</tr>
<tr>
<td><p>Show Invoice in Booking page</p></td>
<td></td>
<td><p>luz_docs_process</p></td>
<td><p>BookingWorkflowPage.xhtml<br />
DocumentManagementController.java</p></td>
<td><p>Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>Delete Invoice<br />
in Booking page</p></td>
<td></td>
<td></td>
<td><p>BookingDocumentDisplayablesService.java<br />
CommonDocumentController.java<br />
DocumentCategoryUtils.java<br />
LuzDocsAIPredictionService.java</p></td>
<td><p>New<br />
New<br />
New<br />
New</p></td>
</tr>
<tr>
<td rowspan="4"><p>Book now</p></td>
<td rowspan="2"><p>Book now</p></td>
<td><p>luz_docs_process</p></td>
<td><p>AIPredictionController.java<br />
AIPredictionDataProvider.java<br />
AmountItemViewHandler.java<br />
BookingInformationViewHandler.java<br />
BusinessCase.java<br />
BusinessCaseController.java<br />
BusinessCaseSelectionViewHandler.java<br />
FinishBookingViewHandler.java<br />
OverviewViewHandler.java<br />
PaidOptionViewHandler.java<br />
PaymentSelectionViewHandler.java</p></td>
<td><p>Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>luz_components</p></td>
<td><p>BookingWorkflowController.java<br />
BookingWorkflowDocumentApprovedException.java<br />
BookingWorkflowDocumentDeleteTagException.java<br />
BookingWorkflowDocumentException.java<br />
BookingWorkflowDocumentMoveToLiabilityException.java<br />
BookingWorkflowDocumentRemoveImageException.java</p></td>
<td><p>New<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>Prediction data</p></td>
<td><p>luz_docs_process</p></td>
<td><p>AnalysisController.java<br />
AnalyzedPrediction.java<br />
CommonAIPredictionServiceFactory.java<br />
FileManagerAIPredictionService.java<br />
ICommonAIPredictionService.java<br />
LuzDocsAIPredictionSerice.java<br />
PredictionType.java</p></td>
<td><p>New<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>Show document of partner</p></td>
<td><p>luz_components</p></td>
<td><p>InvoicePaymentItem.xhtml</p></td>
<td><p>Edit</p></td>
</tr>
<tr>
<td rowspan="2"><p>Book manually</p></td>
<td></td>
<td><p>luz_components</p></td>
<td><p>BookingHeader.java</p></td>
<td><p>Edit</p></td>
</tr>
<tr>
<td></td>
<td><p>luz_finance</p></td>
<td><p>BookingDocument.java<br />
ManualBooking.mod<br />
ManualBookingData.ivyClass<br />
ManualBookingDetailPage.xhtml<br />
ManualBookingHeader.java<br />
ManualBookingHeaderValidator.java<br />
ManualBookingPageData.ivyClass<br />
ManualBookingPageProcess.mod<br />
ManualBookingUploadFile.xhtml<br />
ManualController.java</p></td>
<td><p>Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>Counter on Book</p></td>
<td></td>
<td><p>luz_finance</p></td>
<td><p>InvoiceController.java</p></td>
<td><p>Edit</p></td>
</tr>
<tr>
<td rowspan="2"><p>Sync</p></td>
<td rowspan="2"><p>Add bank transaction manually &amp; Reconcile</p></td>
<td><p>luz_components</p></td>
<td><p>BankTransaction.java<br />
BankTransactionConverter.java<br />
RestClient.java</p></td>
<td><p>Edit<br />
New<br />
Edit</p></td>
</tr>
<tr>
<td><p>luz_finance</p></td>
<td><p>InvoiceControllerTest.java<br />
NewReceiptController.java<br />
NewReceiptControllerTest.java<br />
NewReceiptData.ivyClass<br />
NewReceiptData.java</p></td>
<td><p>Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td rowspan="2"><p>Show Open positions</p></td>
<td rowspan="2"></td>
<td><p>luz_finance</p></td>
<td><p>OpenPositionDisplay.java<br />
OpenPositionHandler.java<br />
OpenPostion.xhtml</p></td>
<td><p>Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>luz_components</p></td>
<td><p>OpenedPosition.java<br />
OpenedPositionDisplay.java</p></td>
<td><p>Edit<br />
Edit</p></td>
</tr>
<tr>
<td rowspan="2"><p>Upload invoices</p></td>
<td rowspan="2"></td>
<td><p>luz-docs-process</p></td>
<td><p>CallAIUploadedFileCommand.java<br />
CompanyFileUploadCommand.java<br />
DocumentResource.java</p></td>
<td><p>Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>luz-component</p></td>
<td><p>ServiceFactory.java</p></td>
<td><p>New</p></td>
</tr>
<tr>
<td rowspan="2"><p>Journal</p></td>
<td rowspan="2"></td>
<td><p>luz-components</p></td>
<td><p>BookingHeader.java<br />
DocumentViewerData.ivyClass<br />
DocumentViewerProcess.mod<br />
getDocumentManagementData.ivyClass</p></td>
<td><p>Edit<br />
Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>luz-finance</p></td>
<td><p>AccountCompanyFileProvideController.java<br />
AccountingConfigurationController.java<br />
AccountingDataUtil.java<br />
AccountingOverviewData.ivyClass<br />
AccountingOverviewProcess.mod<br />
BookingContainerTest.java<br />
BookingControllerTest.java<br />
BookingDocument.java<br />
BookingHeaderValidatorExistingFileTest.java<br />
BookingItem.xhtml<br />
BusinessYearBean.java<br />
BusinessYearBeanTest.java<br />
BusinessYearReportDocument.java<br />
CommonAccountingDocumentServiceFactory.java<br />
FileManageController.java<br />
ICommonAccoutingDocumentService.java<br />
LuzDocsController.java<br />
MainJournalBookingFileUploadViewHandler.java<br />
MainJournalBookingFileUploadViewHandlerTest.java<br />
MainJournalController.java<br />
MainJournalControllerTest.java<br />
ManualBooking.mod<br />
ManualBookingData.ivyClass<br />
ManualBookingDetailPage.xhtml<br />
ManualBookingFactoryTest.java<br />
ManualBookingHeader.java<br />
ManualBookingHeaderValidator.java<br />
ManualBookingPageData.ivyClass<br />
ManualBookingPageProcess.mod<br />
ManualBookingUploadFile.xhtml<br />
ManualController.java<br />
YearEndClosingDataUtil.java</p></td>
<td><p>Edit<br />
Edit<br />
New<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
New<br />
Edit<br />
New<br />
New<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
New</p></td>
</tr>
<tr>
<td rowspan="2"><p>Pay</p></td>
<td rowspan="2"></td>
<td><p>luz-components</p></td>
<td><p>DocumentBookingPaymentService.java<br />
Payment.java<br />
PaymentsInitiation.java</p></td>
<td><p>New<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>luz-finance</p></td>
<td><p>AccountingPaymentController.java<br />
AccountingPaymentGenerator.java<br />
BankAccountConverter.java<br />
PaymentItem.xhtml</p></td>
<td><p>Edit<br />
Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>Show number invoices<br />
in dashboard</p></td>
<td></td>
<td><p>luz-components</p></td>
<td><p>AccountingIndicatorBean.java</p></td>
<td><p>Edit</p></td>
</tr>
<tr>
<td rowspan="2"><p>Dunning Page</p></td>
<td rowspan="2"></td>
<td><p>luz-components</p></td>
<td><p>CommonDocumentPreviewController.java<br />
DocumentSendingComponent.xhtml<br />
DunningDataUtils.java<br />
FileUtils.java<br />
ICommonDocumentDisplayService.java<br />
IDocumentCategoryDataService.java<br />
LocalDateTimeUtils.java</p></td>
<td><p>Edit<br />
Edit<br />
New<br />
Edit<br />
New<br />
New<br />
Edit</p></td>
</tr>
<tr>
<td><p>luz-finance</p></td>
<td><p>DunningInvoice.xhtml<br />
DunningMethodSelection.xhtml<br />
DunningOverview.xhtml<br />
DunningOverviewNew.xhtml<br />
DunningPartnerInvoices.xhtml<br />
DunningPage.xhtml<br />
CommonDunningBean.java<br />
DunningSendingGroup.java<br />
DunningOpenPositionDisplay.java<br />
DunningMetaData.java<br />
DunningUtils.java<br />
DunningConverter.java<br />
DunningFormGenerator.java<br />
DunningSendingGroupConverter.java<br />
DunningPageData.ivyClass<br />
DunningPageProcess.mod</p></td>
<td><p>Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>Do VAT reporting</p></td>
<td></td>
<td><p>luz_finance</p></td>
<td><p>VatClearingReportBookingDetail.java<br />
VatClearingReportTable.xhtml</p></td>
<td><p>Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>Year and Closing</p></td>
<td></td>
<td><p>luz_finance</p></td>
<td><p>BusinessYearBean.java</p>
<p>BusinessYearReportDocument.java</p>
<p>YearEndClosingDataUtil.java</p></td>
<td><p>Edit</p>
<p>Edit</p>
<p>Add</p></td>
</tr>
<tr>
<td rowspan="4"><p>Credit Card</p></td>
<td rowspan="2"><p>Add transaction manually</p></td>
<td><p>luz_components</p></td>
<td><p>BookingCreditCardTransaction.java</p></td>
<td><p>Edit</p></td>
</tr>
<tr>
<td><p>luz_finance</p></td>
<td><p>BookingWithTemplateCreation.java<br />
CreditCardBankTransactionConverter.java<br />
CreditCardTransaction.java<br />
CreditCardTransactionReconciliationBean.java<br />
IBankTransactionReconcilable.java<br />
ManualAddingTransactionsBean.java</p></td>
<td><p>Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>Reconciliation</p></td>
<td><p>luz_finance</p></td>
<td><p>ManualReconciliationBookingInfo.xhtml<br />
ManualReconciliationTransactionInfo.xhtml</p></td>
<td><p>Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>Top up</p></td>
<td><p>luz_finance</p></td>
<td><p>CreditCardAddMoneyViewHandler.java<br />
CreditCardAddMoneyBooking.java</p></td>
<td><p>Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>Show number of invoices<br />
in dashboard</p></td>
<td></td>
<td><p>luz_components</p></td>
<td><p>AccountingIndicatorBean.java<br />
AccountingIndicatorBeanTest.java</p></td>
<td><p>Edit<br />
Edit</p></td>
</tr>
<tr>
<td rowspan="23"><p>Accounting</p></td>
<td rowspan="2"><p>Accounting</p></td>
<td><p>luz_components</p></td>
<td><p>AccountingConfiguration.java<br />
AccountingConfigurationContainer.java<br />
CommonBookingBean.java<br />
CommonDocumentUploadController.java<br />
DocumentItem.xhtml<br />
DocumentListViewData.ivyClass<br />
DocumentListViewProcess.mod<br />
DocumentUploadData.ivyClass<br />
DocumentUploadProcess.mod<br />
DocumentUtils.java<br />
DocumentViewData.ivyClass<br />
DocumentViewProcess.mod<br />
FileManagerDocumentService.java<br />
YearEndClosingFinish.xhtml</p></td>
<td><p>Edit<br />
Edit<br />
New<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>luz_finance</p></td>
<td><p>AccountCompanyFileProvideController.java<br />
AccountingConfigurationController.java<br />
AccountingDataUtil.java<br />
AccountingOverviewData.ivyClass<br />
AccountingOverviewProcess.mod<br />
BusinessYearBean.java<br />
BusinessYearBeanTest.java<br />
BusinessYearReportDocument.java<br />
CommonAccountingDocumentServiceFactory.java<br />
FileManageController.java<br />
ICommonAccoutingDocumentService.java<br />
LuzDocsController.java<br />
YearEndClosingDataUtil.java</p></td>
<td><p>Edit<br />
Edit<br />
New<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
New<br />
Edit<br />
New<br />
New<br />
New</p></td>
</tr>
<tr>
<td rowspan="2"><p>Credit Card</p></td>
<td><p>luz_components</p></td>
<td><p>BookingCreditCardTransaction.java</p></td>
<td><p>Edit</p></td>
</tr>
<tr>
<td><p>luz_finance</p></td>
<td><p>BookingWithTemplateCreation.java<br />
CreditCardAddMoneyBooking.java<br />
CreditCardAddMoneyViewHandler.java<br />
CreditCardAddMoneyViewHandlerTest.java<br />
CreditCardBankTransactionConverter.java<br />
CreditCardBankTransactionConverterTest.java<br />
CreditCardTransaction.java<br />
CreditCardTransactionConverterTest.java<br />
CreditCardTransactionReconciliationBean.java<br />
IBankTransactionReconcilable.java<br />
ManualAddingTransactionsBean.java<br />
ManualReconciliationBookingInfo.xhtml<br />
ManualReconciliationTransactionInfo.xhtml<br />
VatClearingReportBookingDetail.java<br />
VatClearingReportTable.xhtml</p></td>
<td><p>Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td rowspan="2"><p>VAT reporting</p></td>
<td><p>luz_accounting</p></td>
<td><p>BookingDetailEntity.java</p></td>
<td><p>Edit</p></td>
</tr>
<tr>
<td><p>luz_finance</p></td>
<td><p>BookingWithTemplateCreation.java<br />
CreditCardAddMoneyBooking.java<br />
CreditCardAddMoneyViewHandler.java<br />
CreditCardAddMoneyViewHandlerTest.java<br />
CreditCardBankTransactionConverter.java<br />
CreditCardBankTransactionConverterTest.java<br />
CreditCardTransaction.java<br />
CreditCardTransactionConverterTest.java<br />
CreditCardTransactionReconciliationBean.java<br />
IBankTransactionReconcilable.java<br />
ManualAddingTransactionsBean.java<br />
ManualReconciliationBookingInfo.xhtml<br />
ManualReconciliationTransactionInfo.xhtml<br />
VatClearingReportBookingDetail.java<br />
VatClearingReportTable.xhtml</p></td>
<td><p>Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>Partner Reconciliation</p></td>
<td><p>luz_components</p></td>
<td><p>InvoicePaymentItem.xhtml</p></td>
<td><p>Edit</p></td>
</tr>
<tr>
<td rowspan="3"><p>Dunning</p></td>
<td><p>luz_accounting</p></td>
<td><p>DunningHistory.java<br />
DunningHistoryEntity.java<br />
DunningServiceTest.java</p></td>
<td><p>Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>luz_components</p></td>
<td><p>DocumentSending.java<br />
DocumentSendingComponent.xhtml</p></td>
<td><p>Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>luz_finance</p></td>
<td><p>CommonDunningBean.java<br />
CommonDunningBeanTest.java<br />
DunningConverter.java<br />
DunningFormGenerator.java<br />
DunningInvoice.xhtml<br />
DunningMetaData.java<br />
DunningMethodSelection.xhtml<br />
DunningOpenPositionDisplay.java<br />
DunningOverview.xhtml<br />
DunningOverviewNew.xhtml<br />
DunningPage.xhtml<br />
DunningPageData.ivyClass<br />
DunningPageProcess.mod<br />
DunningPartnerInvoices.xhtml<br />
DunningSendingGroup.java<br />
DunningSendingGroupConverter.java<br />
DunningUtils.java</p></td>
<td><p>New<br />
New<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>Analyze Invoices</p></td>
<td><p>luz_docs</p></td>
<td><p>AnalyzeResponseController.java<br />
PredictedStringValueVisitor.java</p></td>
<td><p>Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>Own Documents</p></td>
<td><p>luz_docs</p></td>
<td><p>KlaraBusinessAGFolderIdBuilder.java<br />
KlaraBusinessAGFolderIdBuilderTest.java<br />
KlaraBusinessAGFolderUtil.java<br />
KlaraBusinessAGFolderUtilTest.java<br />
KlaraBusinessAGServiceTest.java</p></td>
<td><p>Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td rowspan="2"><p>Pay an Invoice</p></td>
<td><p>luz_components</p></td>
<td><p>DocumentBookingPaymentService.java<br />
DocumentBookingPaymentServiceTest.java<br />
DocumentController.java<br />
FileManagerDocumentDisplayService.java<br />
FileManagerDocumentDisplayServiceTest.java<br />
ICommonDocumentDisplayService.java<br />
LuzDocsDocumentDisplayService.java<br />
LuzDocsDocumentDisplayServiceTest.java<br />
Payment.java<br />
PaymentsInitiation.java</p></td>
<td><p>New<br />
New<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>luz_finance</p></td>
<td><p>AccountingPaymentController.java<br />
AccountingPaymentControllerTest.java<br />
AccountingPaymentGenerator.java<br />
BankAccountConverter.java<br />
BankAccountConverterTest.java<br />
PaymentItem.xhtml</p></td>
<td><p>Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>Multi Select</p></td>
<td><p>luz_epost_business_web</p></td>
<td><p>KlaraBusinessAGFolderLetterBoxContentHandler.java<br />
KlaraBusinessAGMultiselectionActionBarHandler.java</p></td>
<td><p>Edit<br />
Edit</p></td>
</tr>
<tr>
<td rowspan="2"><p>Add counter for folders</p></td>
<td><p>luz_epost_business_web</p></td>
<td><p>folder.xhtml</p></td>
<td><p>Edit</p></td>
</tr>
<tr>
<td><p>luz_docs_view_controller</p></td>
<td><p>FolderSearchQueryBuilder.java<br />
FolderSearchQueryBuilderTest.java<br />
KlaraBusinessAGConstant.java<br />
KlaraBusinessAGFolderIdBuilder.java<br />
KlaraBusinessAGFolderIdBuilderTest.java<br />
KlaraBusinessAGFolderUtil.java<br />
KlaraBusinessAGFolderUtilTest.java<br />
KlaraBusinessAGService.java<br />
KlaraBusinessAGServiceTest.java</p></td>
<td><p>Edit<br />
New<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>Folder Name</p></td>
<td><p>luz_epost_business_web</p></td>
<td><p>StorageFolderWebModel.java</p></td>
<td><p>Edit</p></td>
</tr>
<tr>
<td><p>Folder Structure</p></td>
<td><p>luz_epost_business_web</p></td>
<td><p>FolderActionMenuBean.java</p></td>
<td><p>Edit</p></td>
</tr>
<tr>
<td><p>Folder Category</p></td>
<td><p>luz_epost_business_web</p></td>
<td><p>EArchiveBasicUnbrandedFolderListSectionHandler.java<br />
EArchiveBasicUnbrandedFolderListSectionHandlerTest.java<br />
UnbrandedFolderListSectionHandler.java</p></td>
<td><p>Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td rowspan="3"><p>Upload Documents<br />
call from Mobile</p></td>
<td><p>luz_docs_process</p></td>
<td><p>CallAIUploadedFileCommand.java<br />
CompanyFileUploadCommand.java<br />
DocumentResource.java<br />
DocumentResourceTest.java<br />
PaidOptionViewHandler.java<br />
PreselectedInvoiceInfo.java<br />
PreselectedInvoiceInfoContainer.java</p></td>
<td><p>Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>luz_accounting</p></td>
<td><p>PreselectedInvoiceInfo.java<br />
PreselectedInvoiceInfoDao.java<br />
PreselectedInvoiceInfoEntity.java<br />
PreselectedInvoiceInfoResource.java<br />
PreselectedInvoiceInfoService.java<br />
PreselectedInvoiceInfoServiceIT.java</p></td>
<td><p>Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>luz_components</p></td>
<td><p>CacheManager.java<br />
ClearCacheHelper.java<br />
Constants.java<br />
DocumentServiceFactory.java<br />
DocumentUploadBeanParam.java<br />
FileManagerDocumentService.java<br />
ICommonDocumentService.java<br />
ServiceFactory.java</p></td>
<td><p>New<br />
New<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td rowspan="12"><p>Order Management</p></td>
<td rowspan="2"><p><a href="https://axonivy.atlassian.net/browse/LUZ-100848?atlOrigin=eyJpIjoiYTk1ZTNjM2I2M2IwNDJkZDhhNjc5YTExNzAxM2YyN2MiLCJwIjoiaiJ9" class="external-link" rel="nofollow">Credit Rate Check</a></p></td>
<td><p>luz_components</p></td>
<td><p>CreditRatingChecker.xhtml<br />
CreditreformController.java<br />
CreditreformControllerTest.java<br />
CustomerCreditworthiness.java<br />
DocumentCategory.java<br />
DocumentMetadataConstant.java<br />
DocumentUploadBeanParam.java<br />
SubCategory.java<br />
UploadedFileConverter.java<br />
UploadedFileConverterTest.java</p></td>
<td><p>Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>luzfin_finance</p></td>
<td><p>CreditworthinessService.java<br />
CreditworthinessServiceIT.java<br />
CreditworthinessServiceTest.java<br />
CustomerCreditworthiness.java<br />
CustomerCreditworthinessEntity.java<br />
OrderUploadedDocumentEntity.java<br />
OrderUploadedDocumentService.java<br />
OrderUploadedDocumentServiceIT.java<br />
PartnerDocumentEntity.java<br />
PartnerDocumentService.java<br />
PartnerDocumentServiceIT.java<br />
UploadedDocument.java</p></td>
<td><p>Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p><a href="https://axonivy.atlassian.net/browse/LUZ-100646?jql=issueKey%20in%20(LUZ-99658%2C%20LUZ-99661%2C%20LUZ-99666%2C%20LUZ-100646)" class="external-link" rel="nofollow">Confirmations</a></p></td>
<td><p>luz_finance</p></td>
<td><p>ConfirmationController.java</p></td>
<td><p>Edit</p></td>
</tr>
<tr>
<td><p><a href="https://axonivy.atlassian.net/browse/LUZ-100647?jql=issueKey%20in%20(LUZ-99667%2C%20LUZ-99668%2C%20LUZ-99673%2C%20LUZ-100647)" class="external-link" rel="nofollow">Delivery Notes</a></p></td>
<td><p>luz_finance</p></td>
<td><p>DeliveryNoteController.java</p></td>
<td><p>Edit</p></td>
</tr>
<tr>
<td rowspan="2"><p><a href="https://axonivy.atlassian.net/browse/LUZ-90768?jql=issueKey%20in%20(LUZ-90754%2C%20LUZ-90757%2C%20LUZ-90758%2C%20LUZ-90762%2C%20LUZ-90763%2C%20LUZ-90766%2C%20LUZ-90767%2C%20LUZ-90768)" class="external-link" rel="nofollow">Invoices / Credit Notes</a></p></td>
<td><p>luz_components</p></td>
<td><p>AdditionalDataBeanParam.java<br />
Constant.java<br />
DocumentServiceDelegate.java<br />
DocumentUploadBeanParam.java<br />
DocumentUploadMetadata.java<br />
FileManagerDocumentCategoryDataService.java<br />
FileManagerDocumentService.java<br />
ICommonDocumentService.java<br />
IDocumentCategoryDataService.java<br />
LuzDocsDocumentCategoryDataService.java<br />
LuzDocsDocumentService.java</p></td>
<td><p>New<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>luz_finance</p></td>
<td><p>CommonOrderAttachmentUtil.java<br />
InvoiceController.java<br />
InvoiceDao.java<br />
MoveOrderController.java<br />
OrderPrintUtil.java</p></td>
<td><p>Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td rowspan="3"><p><a href="https://axonivy.atlassian.net/browse/LUZ-90775?atlOrigin=eyJpIjoiYzU0YzVmMDQ1NjFhNDZjNThjNmU5ZDVhZTA1ODU0NzUiLCJwIjoiaiJ9" class="external-link" rel="nofollow">Recurring Invoices</a></p></td>
<td><p>luzfin_finance</p></td>
<td><p>IvyWebClient.java</p></td>
<td><p>Edit</p></td>
</tr>
<tr>
<td><p>luz_finance</p></td>
<td><p>CommonOrderAttachmentUtil.java<br />
CommonOrderAttachmentUtilTest.java<br />
DocumentController.java<br />
InvoicePageProcess.mod<br />
OrderAttachmentService.java<br />
OrderAttachmentServiceTest.java<br />
OrderDataSupplierUtil.java<br />
PrintInvoiceResult.java<br />
PrintInvoiceService.java<br />
PrintOrderTemplateResource.java<br />
RecurringInvoiceTemplateHandler.java<br />
RecurringInvoiceTemplateInfo.java<br />
RecurringInvoiceTemplatePageProcess.mod</p></td>
<td><p>Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
New<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>luz_components</p></td>
<td><p>AttachmentProcess.mod<br />
DocumentResource.java<br />
FileAttachment.java<br />
FileAttachmentConverter.java<br />
KlaraEArchiveDetector.java</p></td>
<td><p>Edit<br />
New<br />
Edit<br />
Edit<br />
New</p></td>
</tr>
<tr>
<td rowspan="2"><p><a href="https://axonivy.atlassian.net/browse/LUZ-99614?jql=issueKey%20in%20(LUZ-99608%2C%20LUZ-99614%2C%20LUZ-99605)" class="external-link" rel="nofollow">Offers</a></p></td>
<td><p>luz_components</p></td>
<td><p>FileManagerDocumentService.java<br />
ICommonDocumentService.java<br />
LuzDocsDocumentService.java</p></td>
<td><p>Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p>luz_finance</p></td>
<td><p>CommonOrderAttachmentUtil.java<br />
ConfirmationEditPageProcess.mod<br />
ConfirmationInfo.java<br />
DeliveryInfo.java<br />
DeliveryNotePageProcess.mod<br />
DocumentInfo.java<br />
InvoiceInfo.java<br />
OfferInfo.java<br />
OfferPageProcess.mod<br />
RecurringInvoiceTemplateInfo.java</p></td>
<td><p>Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td><p><a href="https://axonivy.atlassian.net/browse/LUZ-100980?atlOrigin=eyJpIjoiMjEwNTBmZmMxYjQ4NDdkNWE2NzVmZDRkYTE2ZTI1YWEiLCJwIjoiaiJ9" class="external-link" rel="nofollow">Upload Files (Orders)</a></p></td>
<td><p>luz_docs_process</p></td>
<td><p>DocumentItemController.java<br />
DocumentItemControllerTest.java<br />
DocumentResourceHelper.java<br />
FileManagerDocumentService.java<br />
FileManagerDocumentServiceTest.java<br />
ICommonDocumentService.java<br />
IvyWebClient.java<br />
IvyWebClientService.java<br />
KlaraEArchiveDetector.java<br />
LuzDocsDocumentService.java<br />
OrderContainer.java<br />
OrderUploadedDocumentService.java<br />
OrderUploadedFileResource.java<br />
PartnerDocumentResource.java<br />
UploadedDocument.java<br />
UploadedFiles.xhtml<br />
UploadFileViewHandler.java</p></td>
<td><p>Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit<br />
Edit</p></td>
</tr>
<tr>
<td rowspan="2"><p>Customers / Partners</p></td>
<td rowspan="2"><p><a href="https://axonivy.atlassian.net/browse/LUZ-99586?atlOrigin=eyJpIjoiZTVlYmIzYTVkOTE5NGU1ZDhlZTM0MzQ1OTRlNWM2NzEiLCJwIjoiaiJ9" class="external-link" rel="nofollow">Thumbnail of Invoices</a></p></td>
<td><p>luz_components</p></td>
<td><p>CustomerViewDetail.java<br />
DocumentIdUtils.java</p></td>
<td><p>Edit<br />
New</p></td>
</tr>
<tr>
<td><p>luz_finance</p></td>
<td><p>BusinessPartnerActivityStatement.xhtml<br />
DefaultPartnerActivityStatement.java<br />
PartnerActivityStatement.java</p></td>
<td><p>Edit<br />
Edit<br />
Edit</p></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Impact of code changes on common components]]
- [[Collect all calls FileManager APIs by Klara Modules]]
- [[Investigate Analyze the API's which call to FileManager]]
- [[List out places calling booking function]]
- [[Programming]]

%% ai-graph-end %%