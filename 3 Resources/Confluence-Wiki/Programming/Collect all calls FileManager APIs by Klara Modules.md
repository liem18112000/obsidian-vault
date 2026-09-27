---
ai_hash: 0d5b17141552d576
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 1
depth: 2.65
entities: []
relevance: 0.738
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47124251845/Collect+all+calls+FileManager+APIs+by+Klara+Modules
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: Collect all calls FileManager APIs by Klara Modules
topic: programming
type: source
updated: 2023-11-15
---

# Collect all calls FileManager APIs by Klara Modules

> [!info] Imported from Confluence
> Space **LUZ** · updated 2023-11-15 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47124251845/Collect+all+calls+FileManager+APIs+by+Klara+Modules)
> Relevance 0.738 · topic `programming`

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
<th><p><strong>Team</strong></p></th>
<th><p><strong>Module Name</strong></p></th>
<th><p><strong>Direct/Not direct to File Manager</strong></p></th>
<th><p><strong>File/Class Involved (in case of directly call to File Manager)</strong></p>
<p><strong>and EndPoint/Resource (in case of call via a REST API)</strong></p></th>
<th><p><strong>Notes</strong></p></th>
<th><p><strong>Code Migration</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td></td>
<td><p>luz_docs_processes</p></td>
<td><p>Direct call to FileManager</p></td>
<td><p>BookingProcessProcess.mod</p></td>
<td><p>Fetch Data Booking Process</p></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>2</td>
<td></td>
<td><p>luz_finance</p></td>
<td><p>Direct call to FileManager</p></td>
<td><p>OrderDetailPage.xhtml</p></td>
<td><p>Fetch Document for Order: User click to Order Management → Orders → Order Detail</p></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>3</td>
<td></td>
<td><p>luz_finance</p></td>
<td><p>Send REST API to backend to upload the file to FileManager</p></td>
<td><p>Pront-end: UploadFileViewHandler.java</p>
<p>REST Endpoint: {company-tenant-id}/companies/{companies-id}/orders/{order-id}/order-uploaded-documents/uploaded-file (OrderUploadedDocumentResource.java)</p></td>
<td><p>Upload Order document</p></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>4</td>
<td></td>
<td><p>luz_components</p></td>
<td><p>Direct call to FileManager</p></td>
<td><p>DocumentHelper, DocumentService, DocumentRepo</p></td>
<td><p>Insert and get</p></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>5</td>
<td><p>Hacka</p></td>
<td><p>luz_news</p></td>
<td><p>Direct</p></td>
<td><p>UploadImage.xhtml</p></td>
<td><p>Upload image for posts</p></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>6</td>
<td><p>Hacka</p></td>
<td><p>luz_news</p></td>
<td><p>Direct</p></td>
<td><p>UploadFileSection.xhtml</p></td>
<td><p>Upload document for posts</p></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>7</td>
<td><p>Hacka</p></td>
<td><p>luz_marketing</p></td>
<td><p>Direct</p></td>
<td><p>/{company-tenant-id}/companies/{company-id}/documents/{file-id}</p>
<p>(IvyWebClient.java)</p></td>
<td><p>Get image/document for posts</p></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>8</td>
<td><p>Avatar</p></td>
<td><p>luz_docs_processes</p></td>
<td><p>Direct call to FileManager</p></td>
<td><p>ReporterPictureResource</p></td>
<td><p>Upload avatar for partner/customer</p>
<p>Delete avatar of partner/customer</p></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>9</td>
<td><p>Avatar</p></td>
<td><p>luz_docs_processes</p></td>
<td><p>Direct call to FileManager</p></td>
<td><p>PartnerDocumentResource</p></td>
<td><p>Upload document for partner/customer. Go to partner detail, upload document for that partner</p></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>10</td>
<td><p>Avatar</p></td>
<td><p>luz_docs_processes</p></td>
<td><p>Direct call to FileManager</p></td>
<td><p>OrderUploadedFileResource</p></td>
<td><p>Upload document for order detail</p></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>11</td>
<td><p>Avatar</p></td>
<td><p>luz_finance</p></td>
<td><p>Direct</p></td>
<td><p>PrintOrderTemplateResource</p></td>
<td><p>Clone attachment for copy documents case</p></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>12</td>
<td><p>Avatar</p></td>
<td><p>luz_finance</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>13</td>
<td><p>Helios</p></td>
<td><p>luz_mobile</p></td>
<td></td>
<td><p>LuzApiIvyRestClient.java all rest api call to IVY</p></td>
<td><p>Upload invoice document (done)</p></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>14</td>
<td><p>Helios</p></td>
<td><p>luz_mobile</p></td>
<td></td>
<td><p>LuzApiIvyRestClient.java all rest api call to IVY</p></td>
<td><p>Upload payment invoice</p></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>15</td>
<td><p>Helios</p></td>
<td><p>luz_mobile</p></td>
<td></td>
<td><p>ContractWorkingTimeResource.java =&gt; saveContractWorkingTime() OR updateContractWorkingTime()<br />
-&gt; luzIvyServiceKey/{company-tenant-id}/companies/{company-id}/employees/{employee-id}/documents</p></td>
<td><p>Upload employee’s expense document (done)</p></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>16</td>
<td><p>Helios</p></td>
<td><p>luz_mobile</p></td>
<td></td>
<td><p>LuzApiIvyRestClient.java all rest api call to IVY</p></td>
<td><p>Get employee’s expense document (done)</p></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>17</td>
<td><p>Helios</p></td>
<td><p>luz_mobile</p></td>
<td></td>
<td><p>ContractWorkingTimeResource.java =&gt; deleteContractWorkingTime()<br />
-&gt; {tenant-id}/companies/{company-id}/employees/{employee-id}/documents/{document-id}</p></td>
<td><p>Delete employee’s expense document</p></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>18</td>
<td><p>Helios</p></td>
<td><p>luz_mobile → luzfin_finance</p></td>
<td></td>
<td><p>luzfinFinanceServiceKey/{tenant-id}/companies/{company-id}/partners/{partner-id}/partner-documents</p></td>
<td><p>Upload partner’s document</p></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>19</td>
<td><p>Helios</p></td>
<td><p>luz_mobile</p></td>
<td></td>
<td><p>luzfinFinanceServiceKey/{tenant-id}/companies/{company-id}/partners/{partner-id}/partner-documents/{partner-document-id}/download</p></td>
<td><p>Get partner’s document (done)</p></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>20</td>
<td><p>Helios</p></td>
<td><p>luz_mobile</p></td>
<td></td>
<td><p>luzfinFinanceServiceKey/{tenant-id}/companies/{company-id}/partners/{partner-id}/partner-documents/{document-id}</p></td>
<td><p>Delete partner’s document</p></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>21</td>
<td><p>Helios</p></td>
<td><p>luz_pos → luz_compensation</p>
<p>luz_pos → IVY</p></td>
<td></td>
<td><p>luz_compensation/api/{company-tenant-id}/companies/{company-id}/logo → Return logo ID</p>
<p>/luz/api/{tenant-id}/companies/{company-id}/documents/{logo-id}</p></td>
<td><p>Get company logo</p></td>
<td></td>
</tr>
<tr>
<td>22</td>
<td><p>Wow</p></td>
<td><p>luz_fortuna_web</p></td>
<td><p>Direct call to FileManager</p></td>
<td><p>FortunaWidget.xhtml</p>
<p>FortunaDocumentHelper.saveFileToOwnerDocumentFolder</p></td>
<td><p>Subscribe Fortuna widget</p></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>23</td>
<td><p>Wow</p></td>
<td><p>luz_xhrm_processes</p></td>
<td><p>Direct call to FileManager</p></td>
<td><p>SAVE file:</p>
<p>SocialInsuranceRegistrationProcessingModHelper.storeSocialInsuranceRegistrationFile</p>
<p>HouseholdConfirmationPage.xhtml</p>
<p>GET file: SocialInsuranceRegistrationConfirmationModHelper.getSocialInsuranceStreamedContent → <em>FileManagerUtil.getIvyFileByFilePath(path)</em></p></td>
<td><p>Create first employee of Klara Home</p></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>24</td>
<td><p>Wow</p></td>
<td><p>luz_xhrm_processes, luz_components</p></td>
<td><p>Direct call to FileManager</p></td>
<td><p>luz_components/src/com/axonivy/luz/components/documents/service/DocumentHelper</p>
<p>Called from (luz_xhrm_processes):</p>
<ul>
<li><p>SalaryPaymentController.storeImportanceDocumentOfCompany() → FileManagerDocumentService → DocumentHelper</p></li>
<li><p>SalaryPaymentUtil.storePaySlipFileOfEmployeeToDatabase() → FileManagerDocumentService → DocumentHelper</p></li>
</ul></td>
<td><p>Salary Run</p></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>25</td>
<td><p>Wow</p></td>
<td><p>luz_finance, luz_components</p></td>
<td><p>Direct call to FileManager</p></td>
<td><p>luz_components/src/com/axonivy/luz/components/documents/DocumentManager</p>
<p>Called from (luz_finance):</p>
<ul>
<li><p>WidgetInvoicesBean.generateInvoiceDetails() → WidgetInvoiceDataExportBean → DocumentUploadController → DocumentManager</p></li>
<li><p>WidgetInvoicesBean.uploadInvoicesToCustomerFolders() → DocumentUploadController → DocumentManager</p></li>
<li><p>WidgetInvoicesBean.uploadDataFilesToCustomerFolders() → WidgetInvoiceDataExportBean → DocumentUploadController → DocumentManager</p></li>
</ul></td>
<td><p>Old Invoice Run (write)</p></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>26</td>
<td><p>Wow</p></td>
<td><p>luz_finance, luz_components</p></td>
<td><p>Direct call to FileManager</p></td>
<td><p>luz_components/src/com/axonivy/luz/components/documents/service/repo/FileManagerDocumentRepo</p>
<p>Call from (luz_finance):</p>
<ul>
<li><p>WidgetInvoicesBean.collectInvoiceFile() → DocumentController → DocumentService → FileManagerDocumentRepo</p></li>
</ul></td>
<td><p>Old Invoice Run (read)</p></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>27</td>
<td><p>Wow</p></td>
<td><p>luz_store, luz_docs_processes, luz_components</p></td>
<td><p>Backend call from luz_store to → luz_docs_processes → luz_components</p></td>
<td><p>luz_components/src/com/axonivy/luz/components/documents/DocumentManager</p>
<p>Called from:</p>
<p>→ (luz_store) IvyRestClientService → IvyRestClientService</p>
<p>→ (luz_docs_processes) OrderUploadedFileResource → DocumentResourceHelper → DocumentRestService</p>
<p>→ (luz_components) DocumentManager</p></td>
<td><p>New Invoice Run (not yet on production)</p></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>28</td>
<td><p>Arrow</p></td>
<td><p>luz-online</p></td>
<td></td>
<td><p><a href="https://bitbucket.org/axonivy-prod/luz_online/src/36b8b1872234bc4ef1b381ef432a0f6bf72e545a/src/main/java/ch/klara/luz/online/rest/client/IvyRestClient.java?at=master#IvyRestClient.java-25" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_online/src/36b8b1872234bc4ef1b381ef432a0f6bf72e545a/src/main/java/ch/klara/luz/online/rest/client/IvyRestClient.java?at=master#IvyRestClient.java-25</a></p></td>
<td><p>Create order</p></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>29</td>
<td rowspan="2"><p>Miracle</p></td>
<td rowspan="2"><p>luz_analytics_etl</p></td>
<td><p>Backend call API of luz_docs_processes</p></td>
<td><p><code>src/com/axonivy/luz/document/rest/DocumentResource.java</code></p>
<ul>
<li><p>Get all documents of company in tenant: <a href="http://luz-webclient:8081/luz/api/%7Btenant-id%7D/companies/%7Bcompany-id%7D/documents/metadata?get-all=true" class="external-link" rel="nofollow">http://luz-webclient:8081/luz/api/{tenant-id}/companies/{company-id}/documents/metadata?get-all=true</a></p></li>
<li><p>Get document: <a href="http://luz-webclient:8081/luz/api/%7Btenant-id%7D/companies/%7Bcompany-id%7D/documents/%7Bencoded-path%7D/metadata" class="external-link" rel="nofollow">http://luz-webclient:8081/luz/api/{tenant-id}/companies/{company-id}/documents/{encoded-path}/metadata</a></p></li>
<li><p>Get document content: <a href="http://luz-webclient:8081/luz/api/%7Btenant-id%7D/companies/%7Bcompany-id%7D/documents/%7Bdocument-id%7D/content" class="external-link" rel="nofollow">http://luz-webclient:8081/luz/api/{tenant-id}/companies/{company-id}/documents/{document-id}/content</a></p></li>
</ul></td>
<td></td>
<td><p>DONE<br />
</p></td>
</tr>
<tr>
<td>30</td>
<td><p>Direct call to FileManager</p></td>
<td><p><code>src/main/java/ch/axonivy/filemanager/document/DocumentResource.java</code></p>
<ul>
<li><p>Get content: <code>{tenant-id}/documents/{document-id}?withContent=true</code></p></li>
</ul></td>
<td></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>31</td>
<td></td>
<td><p>luz_docs_prcoess</p></td>
<td><p>Direct call to FileManager</p></td>
<td><p>/luz_docs_process/src/com/axonivy/luz/document/rest/ReporterPictureResource.java</p>
<p>uploadReporterPicture API</p>
<p>deleteReporterPictureById API</p></td>
<td></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>32</td>
<td></td>
<td><p>luz_finance</p></td>
<td><p>Direct call to FileManager</p></td>
<td><p>/luz_finance/src/com/axonivy/luz/fin/migration/FinanceMigrationResource.java<br />
<br />
getReferenceNumberByInvoiceDocumentId API</p></td>
<td></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>33</td>
<td></td>
<td><p>luz_docs_process</p></td>
<td><p>Direct call to FileManager</p></td>
<td><p>/luz_docs_process/src/com/axonivy/luz/document/rest/OrderUploadedFileResource.java</p>
<p>uploadOrderDocumentWithoutDocumentText API</p>
<p>uploadInvoiceDocumentFor API</p></td>
<td></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>34</td>
<td></td>
<td><p>luz_docs_process</p></td>
<td><p>Direct call to FileManager</p></td>
<td><p>/luz_docs_process/src/com/axonivy/luz/document/rest/PostDocumentResource.java</p>
<p>uploadDocument API</p></td>
<td></td>
<td><p>DONE</p></td>
</tr>
<tr>
<td>35</td>
<td><p>Pixels</p></td>
<td><p>spay_web</p></td>
<td><p>Direct call to FileManager</p></td>
<td><p>ch/soreco/spay/web/admin/CompanyDataMigration/sources/CompanyDataMigrationController.java</p>
<p>storeMigrationDocumentOfCompany</p></td>
<td><p>fileType: MIGRATION</p>
<p>DocumentCategory.MIGRATION</p></td>
<td></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Investigate Analyze the API's which call to FileManager]]
- [[Impact of code changes on common components]]
- [[Merging process]]
- [[One API Module Responsibilities]]
- [[Uploading documents]]

%% ai-graph-end %%