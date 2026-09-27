---
title: "Investigate: Analyze the API's which call to FileManager"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47127134261/Investigate+Analyze+the+API+s+which+call+to+FileManager
space: "LUZ"
topic: programming
relevance: 0.703
depth: 2.41
updated: 2022-12-14
attachments: 2
tags:
  - confluence
  - programming
  - space/luz
---

# Investigate: Analyze the API's which call to FileManager

> [!info] Imported from Confluence
> Space **LUZ** · updated 2022-12-14 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47127134261/Investigate+Analyze+the+API+s+which+call+to+FileManager)
> Relevance 0.703 · topic `programming`

**Purpose**: Collect information about APIs which used FileManager

**Intended Audience**: Developers  

1.  **Summarize**: There are about 21 functionalities, <span class="inline-comment-marker" ref="fb4b23c6-157e-4fe0-8fb7-1bc651542e34">8 modules</span> that related to FileManager

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Module</strong></p></th>
<th><p><strong>Functionalities</strong></p></th>
</tr>
&#10;<tr>
<td><p>luz_news</p></td>
<td><ul>
<li><p>Load list of posts (preview thumbnail) and display on online dashboard</p></li>
<li><p>Get post detail</p></li>
<li><p>Get image/document for post</p></li>
<li><p>Create post (upload image/document)</p></li>
<li><p>Update post (update images/document)</p></li>
</ul></td>
</tr>
<tr>
<td><p>luz_marketing</p></td>
<td><ul>
<li><p>APIs for luz_news (used to call to FileManager)</p></li>
</ul></td>
</tr>
<tr>
<td><p>luz_mobile</p></td>
<td><ul>
<li><p>Upload document</p></li>
<li><p>Upload Employee Document</p></li>
<li><p>Get document By employee</p></li>
<li><p>Download document</p></li>
<li><p>Download employee Document</p></li>
<li><p>Delete Document</p></li>
</ul></td>
</tr>
<tr>
<td><p>luz_finance</p></td>
<td><ul>
<li><p>Fetch Data Booking Process</p></li>
<li><p>Fetch Document for Order</p></li>
<li><p>Fetch Document for Offers</p></li>
<li><p>Upload/ Delete avatar of Customer/ Partner</p></li>
<li><p>Upload document of Customer/ Partner</p></li>
<li><p>Delete document of Customer/ Partner</p></li>
</ul></td>
</tr>
<tr>
<td><p>luz_store</p></td>
<td><ul>
<li><p>Get product document</p></li>
<li><p>Upload product document</p></li>
</ul></td>
</tr>
<tr>
<td><p>luz_xhrm_processes</p></td>
<td><ul>
<li><p>Upload file Mobiliar policy</p></li>
</ul></td>
</tr>
<tr>
<td><p>luz_component</p></td>
<td><ul>
<li><p>Fetch Own Document for Employee</p></li>
</ul></td>
</tr>
<tr>
<td><p>luz_docs_process</p></td>
<td><ul>
<li><p>Backend for the other</p></li>
</ul></td>
</tr>
</tbody>
</table>

</div>

2\. **Details:**

<div>

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
<th><p><strong>No.</strong></p></th>
<th><p><strong>Module</strong></p></th>
<th><p><strong>Feature</strong></p></th>
<th><p><strong>Flow</strong></p></th>
<th><p><strong>APIs/Methods/Module that call to FileManager</strong></p></th>
<th><p><strong>Input/Output</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p>luz_news<br />
luz_marketing<br />
<strong>luz_component</strong></p></td>
<td><p>Load list of posts (preview thumbnail) and display on online dashboard<br />
Team: Hacka</p></td>
<td><ol>
<li><p>Step by step<br />
-&gt; Open Klara<br />
-&gt; Go to Online</p></li>
<li><p>Flow<br />
<strong>&lt;luz_news&gt;</strong><br />
-&gt; PostLazyItem.load(int first, int pageSize)<br />
-&gt; PostLazyItem.loadPosts(PostCriteria postCriteria)<br />
-&gt; PostController.getPostOverviewBean(PostCriteria postCriteria)<br />
-&gt; Then call to <strong>luz_marketing</strong> with uri (/luz_marketing/api/{tenant-id}/companies/{company-id}/posts) to get list of posts (data in table post, luzmarketing database)<br />
-&gt; Then pass value to to PostOverview.xhml<br />
</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="555264f8-f61a-4795-827d-f7ec04ec223c" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&lt;div class=&quot;news-image-container&quot;&gt;
    &lt;p:graphicImage
        styleClass=&quot;news-image&quot;
        value=&quot;#{thumbnailBean.thumbnail}&quot;&gt;
        &lt;f:param name=&quot;fileId&quot; value=&quot;#{cc.attrs.postBean.post.imageId}&quot; /&gt;
        &lt;f:param name=&quot;defaultIconName&quot; value=&quot;default-image-thumbnail.png&quot; /&gt;
    &lt;/p:graphicImage&gt;
&lt;/div&gt;</code></pre>
</div>
</div>
<p><br />
<strong>&lt;luz_component&gt;</strong><br />
-&gt; Invoke ThumbnailBean.getThumbnail()<br />
-&gt; call ThumbnailBean.tryToGetThumbnailFromDB(Long fileId)<br />
-&gt; <strong>Get file from FileManager</strong> (call directly via CompanyDocumentService)<br />
</p></li>
</ol></td>
<td><p>Call directly</p>
<p>Method: ThumbnailBean.tryToGetThumbnailFromDB</p></td>
<td><p>Input: Long fileId</p>
<p>Output: Optional&lt;File&gt;</p></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>luz_news<br />
<strong>luz_component</strong></p></td>
<td><p>Get post detail<br />
Team: Hacka</p></td>
<td><ol>
<li><p>Step by step<br />
-&gt; Open Klara<br />
-&gt; Go to Online<br />
-&gt; Click to post</p></li>
<li><p>Flow<br />
&lt;<strong>luz_news</strong>&gt;<br />
……<br />
-&gt; start CreatPostProcess.mod<br />
</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="feec850d-b0be-41ab-8a37-012fafc1aa7e" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>out.bean = PostController.getPostDetailBean(param.postId);</code></pre>
</div>
</div>
<p><br />
-&gt; call PostController.getPostDetailBean(String postId)<br />
-&gt; call PostDetailBeanConverter.convertToBean(Post post, List&lt;Sector&gt; listOfSectors)<br />
-&gt; call PostDetailBeanConverter.convertToBean(Post post)<br />
&lt;<strong>luz_component</strong>&gt;<br />
-&gt; call UploadedFileConverter.loadFile(Long imageid), UploadedFileConverter.loadFile(Long documentid)<br />
-&gt; call UploadedFileConverter.getIvyFile(Long id)<br />
-&gt; call FileManagerUtils.getIvyFileByFileId(Long id)<br />
-&gt; <strong>Get file from FileManager</strong> (call directly via FileStoreDBHandler.getDocumentOnServerById(id, true))</p></li>
</ol></td>
<td><p>Call directly</p>
<p>Method:</p>
<ol>
<li><p>UploadedFileConverter.loadFile(Long id)</p></li>
</ol></td>
<td><p>Input: Long id</p>
<p>Output: NewFileUploadBean</p></td>
</tr>
<tr>
<td><p>3</p></td>
<td><p>luz_news<br />
<strong>luz_docs_process</strong><br />
luz_marketing</p></td>
<td><p>Get image/document for post<br />
Team: Hacka</p></td>
<td><ol>
<li><p>Step by step</p></li>
<li><p>Flow<br />
-&gt; …<br />
&lt;<strong>luz_marketing</strong>&gt;<br />
-&gt; call rest client IvyWebClient.getDocumentById(String tenantId, Long companyId, Long fileId)<br />
-&gt; endpoint: /{company-tenant-id}/companies/{company-id}/documents/{file-id}<br />
&lt;<strong>luz_docs_process</strong>&gt;<br />
-&gt; invoke DocumentResource.getCompanyDocumentById(DownloadDocumentByIdRequestParam documentRequestParam)<br />
-&gt; call DocumentRestService.getDocumentByIdWithPathVerification(long documentId, String documentPath)<br />
-&gt; <strong>Get file from FileManager</strong> call FileManagerDocumentRepo.getDocumentById(documentId, true)<br />
……<br />
</p></li>
</ol></td>
<td><p>Call directly</p>
<p>Method: DocumentResource.getCompanyDocumentById(DownloadDocumentByIdRequestParam documentRequestParam)</p></td>
<td><p>Input:<br />
String tenantId, Long companyId, Long fileId<br />
Output: Binary Stream</p></td>
</tr>
<tr>
<td><p>4</p></td>
<td><p>luz_news<br />
<strong>luz_component</strong></p></td>
<td><p>Create post (upload image/document)</p></td>
<td><ol>
<li><p>Step by step</p></li>
<li><p>Flow<br />
&lt;<strong>luz_news&gt;</strong><br />
-&gt; call PostController.create(PostDetailBean postDetailBean)<br />
-&gt; call PostConverter.convertToModel(PostConverter.convertToModel)<br />
-&gt; call handleImageFile() or handleDocumentFile()<br />
-&gt; call PostFileUtil.saveToFileManager(NewFileUploadBean fileBean)<br />
&lt;<strong>luz_component</strong>&gt;<br />
-&gt; call DocumentManager.insert(String path, File file)<br />
-&gt; call DocumentHelper.insertFile(File file, String destinationPath, String user)<br />
-&gt; call DocumentHelper.insertFile(File file, String destinationPath, String user, Boolean shouldCreateThumbnail)<br />
-&gt; <strong>Insert file to FileManager</strong> call AbstractFileManagementHandler.insertFile(java.io.File _file, String _destinationPath,<br />
String _user)<br />
….</p></li>
</ol></td>
<td><p>Call directly</p>
<p>Method: DocumentManager.insert(String path, File file)</p></td>
<td><p>Input: String path, File file</p>
<p>Output: int (image/doc id)</p></td>
</tr>
<tr>
<td><p>5</p></td>
<td><p><strong>luz_news</strong><br />
luz_component</p></td>
<td><p>Update post (update images/document)</p>
<p>Team: Hacka</p></td>
<td><ol>
<li><p>Step by step</p></li>
<li><p>Flow<br />
&lt;<strong>luz_news&gt;</strong><br />
-&gt; call PostController.editPost(PostDetailBean postDetailBean)<br />
-&gt; call PostConverter.convertToModel(PostConverter.convertToModel)<br />
-&gt; call handleImageFile() or handleDocumentFile()<br />
-&gt; call PostFileUtil.saveToFileManager(NewFileUploadBean fileBean)<br />
&lt;<strong>luz_component</strong>&gt;<br />
-&gt; call DocumentManager.insert(String path, File file)<br />
-&gt; call DocumentHelper.insertFile(File file, String destinationPath, String user)<br />
-&gt; call DocumentHelper.insertFile(File file, String destinationPath, String user, Boolean shouldCreateThumbnail)<br />
-&gt; <strong>Insert file to FileManager</strong> call AbstractFileManagementHandler.insertFile(java.io.File _file, String _destinationPath,<br />
String _user)<br />
….</p></li>
</ol></td>
<td><p>Call directly</p>
<p>Method: <code>DocumentManager.insert(String path, File file)</code></p></td>
<td><p>Input: String path, File file</p>
<p>Output: int (image/doc id)</p></td>
</tr>
<tr>
<td><p>6</p></td>
<td><p><strong>luz_mobile</strong><br />
<br />
<strong>luz_docs_process</strong></p></td>
<td><p>Upload document</p>
<p>Team: Helios</p></td>
<td><ol>
<li><p>Step by step</p></li>
<li><p>Flow</p>
<ul>
<li><p>&lt;<strong>luz_mobile</strong>&gt;<br />
→ call DocumentService.uploadDocument(DocumentData documentData)<br />
→ call LuzApiIvyRestClient.uploadDocument(<code>MultipartFormDataOutput</code>): <code>{tenant-id}/companies/{company-id}/documents</code><br />
<strong>Note</strong>: <code>MultipartFormDataOutput</code>: <code>String fileName, InputStream inputStream, String category</code></p></li>
<li><p><strong>&lt;luz_docs_process&gt;</strong><br />
-&gt;call DocumentResource.uploadCompanyDocument(InputStream,FormDataContentDisposition)<br />
-&gt;call DocumentManager.insert(String path, File file)<br />
-&gt; call DocumentHelper.insertFile(File file, String destinationPath, String user)<br />
-&gt; call DocumentHelper.insertFile(File file, String destinationPath, String user, Boolean shouldCreateThumbnail)<br />
-&gt; <strong>Insert file to FileManager</strong> call AbstractFileManagementHandler.insertFile(java.io.File _file, String _destinationPath,<br />
String _user)<br />
</p></li>
</ul></li>
</ol></td>
<td><p>Call via <code>LuzApiIvyRestClient</code></p></td>
<td><p>Input: String path, File file<br />
Output: int (image/doc id)</p></td>
</tr>
<tr>
<td><p>7</p></td>
<td><p><strong>luz_mobile</strong><br />
</p>
<p><strong>luz_docs_process</strong></p></td>
<td><p>Upload Employee Document<br />
<br />
Team: Helios</p></td>
<td><ol>
<li><p>Step by step</p></li>
<li><p>Flow</p>
<ul>
<li><p><strong>&lt;luz_mobile&gt;</strong><br />
-&gt; call DocumentService.uploadEmployeeDocument(DocumentData documentData)<br />
-&gt; call LuzApiIvyRestClient.uploadEmployeeDocument(<code>MultipartFormDataOutput</code>): <code>{company-tenant-id}/companies/{company-id}/employees/{employee-id}/documents</code><br />
<strong>Note</strong>: <code>MultipartFormDataOutput</code>: <code>String fileName, InputStream inputStream, String category</code></p></li>
<li><p><strong>&lt;luz_docs_process&gt;</strong><br />
-&gt;call DocumentResource.uploadEmployeeDocument(EmployeeUploadDocumentRequestParam)<br />
-&gt;call DocumentRestService.uploadDocument(String documentPath, File file)<br />
-&gt;call DocumentManager.insert(String path, File file)<br />
-&gt; call DocumentHelper.insertFile(File file, String destinationPath, String user)<br />
-&gt; call DocumentHelper.insertFile(File file, String destinationPath, String user, Boolean shouldCreateThumbnail)<br />
-&gt; <strong>Insert file to FileManager</strong> call AbstractFileManagementHandler.insertFile(java.io.File _file, String _destinationPath,<br />
String _user)<br />
</p></li>
</ul></li>
</ol></td>
<td><p>Call via <code>LuzApiIvyRestClient</code></p></td>
<td><p>Input: String path, File file<br />
Output: int (image/doc id)</p></td>
</tr>
<tr>
<td><p>8</p></td>
<td><p>luz_finance<br />
<strong>luz_component</strong><br />
luz_docs_processs</p></td>
<td><p>Fetch Data Booking Process</p>
<p>Team: N/A</p></td>
<td><ol>
<li><p>Step by step<br />
-&gt; Go to Accounting menu<br />
-&gt; Click Booking</p></li>
<li><p>Flow<br />
&lt;<strong>luz_finance</strong>&gt;<br />
-&gt; AccountingDashboardNew.xhtml<br />
-&gt; MainAccountingWidget.xhtml<br />
-&gt; approveFromNewDashboard()<br />
-&gt; InvoiceController.approveFromNewDashboard()<br />
-&gt; redirect to Pages: LUZ_DOCS_BOOKING<br />
&lt;<strong>luz_component</strong>&gt;<br />
-&gt; RedirectPageUtils.redirect(Pages.LUZ_DOCS_BOOKING)<br />
-&gt; call to luz_docs_process<br />
&lt;<strong>luz_docs_processs</strong>&gt;<br />
-&gt; call DocumentAssignmentController(docId)<br />
-&gt; call DocumentAssignmentController.initialize(docId)<br />
-&gt; DocumentAssignmentController.getDocumentOnServer(docId)<br />
-&gt; call to luz_component<br />
&lt;<strong>luz_component</strong>&gt;<br />
-&gt; call DocumentService.getDocumentOnServerById(long fileid, boolean getJavaFile)<br />
-&gt; call FileManagerDocumentRepo.getDocumentById(long fileid, boolean getJavaFile)<br />
-&gt; <strong>Get file from FileManager</strong><br />
…..</p></li>
</ol></td>
<td><p>Call directly</p>
<p>Method: DocumentService.getDocumentOnServerById(long fileid, boolean getJavaFile)</p></td>
<td><p>Input: long fileid, boolean getJavaFile<br />
Output: DocumentOnServer<br />
Sample Output:<br />
</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="7df5d4ef-0052-4fee-bacc-5fdf0561b7d9" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>Object DocumentOnServer
&#10;fileID=653375,
path=luz/114f1fba-1520-420c-a495-7acea20d1dde/companies/1/documents/liability/InvoiceSimple-PDF-Template.pdf,
filename=InvoiceSimple-PDF-Template.pdf,
fileSize=258 Kb,
userID=au.nguyenphuoc@axonactive.com,
creationDate=19.05.2022,
creationTime=06:29:16,
modificationUserID=,
modificationDate=19.05.2022,
modificationTime=06:29:16,
locked=0,
lockingUserID=,
description=,
ivyFile=tmp/870268459538500/InvoiceSimple-PDF-Template.pdf (temporary),
javaFile=C:\Users\npau\Desktop\work-space\ivy\AxonIvyDesigner9.1.1\files\session\1\tmp\870268459538500\InvoiceSimple-PDF-Template.pdf,
versionnumber=1,
isLocked=false,
displayedPath=luz/114f1fba-1520-420c-a495-7acea20d1dde/companies/1/documents/liability/InvoiceSimple-PDF-Template.pdf,
id=653375</code></pre>
</div>
</div></td>
</tr>
<tr>
<td><p>9</p></td>
<td><p><strong>luz_component</strong></p></td>
<td><p>Fetch Own Document for Employee</p>
<p>Team: N/A</p></td>
<td><ol>
<li><p>Step by step<br />
-&gt; Employee<br />
-&gt; Managed Employees<br />
-&gt; Own Document</p></li>
<li><p>Flow</p>
<p><strong>&lt;luz_component&gt;</strong><br />
-&gt; DocumentCategoryOverviewProcess.mod<br />
-&gt; DocumentViewProcess.mod<br />
-&gt; <strong>Get file from FileManage</strong><br />
-&gt; call DocumentPreviewController.getDocumentForDisplaying(DocumentDisplay documentDisplay, List&lt;DocumentDisplay&gt; documentsList)<br />
-&gt; call DocumentService.getDocumentOnServerById(documentDisplay.getFileId(), true)<br />
</p></li>
</ol></td>
<td><p>Call directly</p>
<p>Method: DocumentService.getDocumentOnServerById(documentDisplay.getFileId(), true)</p></td>
<td><p>Input: long fileid, boolean getJavaFile<br />
Output: DocumentOnServer<br />
<br />
Sample Input:</p>
<ul>
<li><p><code>DocumentDisplay</code></p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="33edd235-ed7e-4660-9b31-55ed8b28e518" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>employeeName=Dinh Ha,
fileName=InvoiceSimple-PDF-Template.pdf,
period=2022-06,
fileType=PDF,
fileId=671751,
thumbnail=C:\Users\npau\Desktop\work-space\ivy\AxonIvyDesigner9.1.1\files\application\thumb\671751.jpg,
creationDate=15.06.2022,
readOnly=false,
userId=au.nguyenphuoc@axonactive.com,
isSelected=false,
fileSize=258 Kb</code></pre>
</div>
</div></li>
<li><p><code>List&lt;DocumentDisplay&gt;</code></p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="a39a7ee3-6317-4ac9-a6dc-4f8cdf4cd3e0" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[employeeName=Dinh Ha,
fileName=InvoiceSimple-PDF-Template.pdf,
period=2022-06,
fileType=PDF,
fileId=671751,
thumbnail=C:\Users\npau\Desktop\work-space\ivy\AxonIvyDesigner9.1.1\files\application\thumb\671751.jpg,
creationDate=15.06.2022,
readOnly=false,
userId=au.nguyenphuoc@axonactive.com,
isSelected=false,
fileSize=258 Kb]</code></pre>
</div>
</div></li>
<li><p>Sample Output:</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="bd52b045-089a-437f-85a6-67e65a2bcce2" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>fileID=671751,
path=luz/114f1fba-1520-420c-a495-7acea20d1dde/companies/1/employees/1/documents/InvoiceSimple-PDF-Template.pdf, 
filename=InvoiceSimple-PDF-Template.pdf, 
fileSize=258 Kb, 
userID=au.nguyenphuoc@axonactive.com, 
creationDate=15.06.2022, 
creationTime=03:52:06, 
modificationUserID=, 
modificationDate=15.06.2022, 
modificationTime=03:52:06, 
locked=0, 
lockingUserID=, 
description=, 
ivyFile=tmp/871088824841300/InvoiceSimple-PDF-Template.pdf (temporary), 
javaFile=C:\Users\npau\Desktop\work-space\ivy\AxonIvyDesigner9.1.1\files\session\1\tmp\871088824841300\InvoiceSimple-PDF-Template.pdf, 
versionnumber=1, 
isLocked=false, 
displayedPath=luz/114f1fba-1520-420c-a495-7acea20d1dde/companies/1/employees/1/documents/InvoiceSimple-PDF-Template.pdf, 
id=671751</code></pre>
</div>
</div></li>
</ul>
<p><br />
</p></td>
</tr>
<tr>
<td><p>10</p></td>
<td><p>luz_finance<br />
<strong>luz_component</strong></p></td>
<td><p>Fetch Document for Order</p>
<p>Team: N/A</p></td>
<td><ol>
<li><p>Step by step<br />
-&gt; User click to Order Management<br />
-&gt; Orders<br />
-&gt; Order Detail</p></li>
<li><p>Flow<br />
&lt;<strong>luz_finance&gt;</strong><br />
-&gt; OrderDetailPage.xhtml<br />
-&gt; DocumentViewProcess.mod<br />
&lt;<strong>luz_component&gt;</strong><br />
-&gt; call DocumentPreviewController.getDocumentForDisplaying(DocumentDisplay documentDisplay, List&lt;DocumentDisplay&gt; documentsList)<br />
-&gt; call CompanyDocumentService.getDocumentOnServerById(documentDisplay.getFileId(), true);</p></li>
</ol></td>
<td><p>Call directly<br />
Method: DocumentPreviewController.getDocumentForDisplaying</p></td>
<td><p>Input: DocumentDisplay documentDisplay, List&lt;DocumentDisplay&gt; documentsList</p>
<p>Input: document id</p>
<p>Sample Output:<br />
</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="cad620b5-3d31-4b2b-be23-255b1a263781" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>fileID=671813, 
path=luz/114f1fba-1520-420c-a495-7acea20d1dde/companies/1/documents/order/4/uploaded-documents/General Information.pdf, 
filename=General Information.pdf, 
fileSize=549 Kb, 
userID=au.nguyenphuoc@axonactive.com, 
creationDate=15.06.2022, 
creationTime=09:47:28, 
modificationUserID=au.nguyenphuoc@axonactive.com, 
modificationDate=15.06.2022, 
modificationTime=09:47:28, 
locked=0, 
lockingUserID=, 
description=, 
versionnumber=1, 
isLocked=false, 
displayedPath=luz/114f1fba-1520-420c-a495-7acea20d1dde/companies/1/documents/order/4/uploaded-documents/General Information.pdf, 
id=671813</code></pre>
</div>
</div></td>
</tr>
<tr>
<td><p>11</p></td>
<td><p>luz_finance<br />
<strong>luz_component</strong></p></td>
<td><p>Fetch Document for Offers</p>
<p>Team: N/A</p></td>
<td><ol>
<li><p>Step by step<br />
-&gt; User click to Order Management<br />
-&gt; Offer<br />
-&gt; Offer Detail</p></li>
<li><p>Flow<br />
&lt;<strong>luz_finance&gt;</strong><br />
- OfferPageProcess.mod<br />
- OfferPage.xhtml<br />
&lt;<strong>luz_component&gt;</strong><br />
-&gt; AttachmentProcess.mod<br />
-&gt; <strong>get document on server</strong> FileManagerUtil.getListFileUploadByComponentId(String componentId)</p></li>
</ol></td>
<td><p>Call directly<br />
Method: FileManagerUtil.getListFileUploadByComponentId(String componentId)</p></td>
<td><p>Input: document id</p>
<p>Sample Output<br />
</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="dab21806-529a-4c31-9177-089770cc8e89" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>fileID=671813, 
path=luz/114f1fba-1520-420c-a495-7acea20d1dde/companies/1/documents/order/4/uploaded-documents/General Information.pdf, 
filename=General Information.pdf, 
fileSize=549 Kb, 
userID=au.nguyenphuoc@axonactive.com, 
creationDate=15.06.2022, 
creationTime=09:47:28, 
modificationUserID=au.nguyenphuoc@axonactive.com, 
modificationDate=15.06.2022, 
modificationTime=09:47:28, 
locked=0, 
lockingUserID=, 
description=, 
versionnumber=1, 
isLocked=false, 
displayedPath=luz/114f1fba-1520-420c-a495-7acea20d1dde/companies/1/documents/order/4/uploaded-documents/General Information.pdf, 
id=671813</code></pre>
</div>
</div></td>
</tr>
<tr>
<td><p>12</p></td>
<td><p><strong>luz_mobile</strong></p>
<p><strong>luz_docs_process</strong></p></td>
<td><p>Get document By employee</p></td>
<td><ol>
<li><p>Step by step</p></li>
<li><p>Flow</p>
<ul>
<li><p><strong>&lt;luz_mobile&gt;</strong><br />
-&gt; call DocumentService.getDocumentByEmployee(<code>...,String employeeId, String period, String category</code>)<br />
-&gt; call LuzApiIvyRestClient.getDocuments(<code>companyId, employeeId, category, period, REQUEST_HEADER_REQUEST_BY</code>): <code>{tenant-id}/companies/{company-id}/employees/{employee-id}/documents</code></p></li>
<li><p><strong>&lt;luz_docs_process&gt;</strong><br />
-&gt;call DocumentResource.getEmployeeDocumentInfosbyPeriod(EmployeeDownloadDocumentRequestParam)<br />
-&gt; call DocumentRestService.searchEmployeeDocumentInfos(EmployeeDownloadDocumentRequestParam)<br />
-&gt; call DocumentRestService.findDocumentsByFilterWithEffectiveDate(EmployeeDownloadDocumentRequestParam)<br />
-&gt; call EmployeeDocumentService.getDocumentsByTag(BasicQueryParameter, tagCondition)<br />
-&gt; call FileManagerDocumentRepo.getDocuments(QueryParaml)<br />
-&gt; <strong>get file from FileManager</strong> call FileStoreDBHandler.getDocumentsFilteredby(filepathCondition, filetypeNameCondition, tagNameCondition, creationDateCondition)</p></li>
</ul></li>
</ol></td>
<td><p>Call via <code>LuzApiIvyRestClient</code></p></td>
<td><p>Input: filepathCondition, filetypeNameCondition, tagNameCondition, creationDateCondition</p>
<p><br />
Output: DocumentOnServer</p></td>
</tr>
<tr>
<td><p>13</p></td>
<td><p>luz_finance<br />
<strong>luz_components</strong></p></td>
<td><p>Upload/ Delete avatar of Customer/ Partner</p></td>
<td><ol>
<li><p>Step by step<br />
-&gt; Customers/ Partners → Add customer/ partner → upload an image → Save<br />
-&gt; or Customers/ Partners → select customer/ partner → Edit info → Delete/Update avatar → Save</p></li>
<li><p>Flow<br />
<strong>&lt;luz_finance&gt;</strong><br />
-&gt; PartnerPage.xhtml<br />
-&gt; PartnerPageProcess.mod → PartnerSavingController.java<br />
<br />
<strong><u>UPLOAD</u></strong><br />
-&gt; <strong>calls luz_components</strong><br />
-&gt; FileUploadUtil.saveFile()<br />
-&gt; DocumentManager.insert()<br />
-&gt; DocumentHelper.insertFile()<br />
<br />
<strong><u>DELETE/UPDATE</u></strong><br />
- delete <strong>calls luz_components</strong><br />
-&gt; FileManagerUtil.deleteByDocumentId()<br />
- update <strong>calls luz_components</strong><br />
-&gt; FileUploadUtil.saveFile()</p></li>
</ol></td>
<td><p>Call directly</p>
<p>Method:<br />
FileUploadUtil.saveFile()</p>
<p>FileManagerUtil.deleteByDocumentId()</p></td>
<td><p>Input:<br />
Output: Long fileId</p></td>
</tr>
<tr>
<td rowspan="2"><p>14</p></td>
<td rowspan="2"><p><strong>luz_finance</strong></p></td>
<td><p>Upload document of Customer/ Partner</p></td>
<td><ol>
<li><p>Step by step<br />
<code>-&gt; Customers/ Partners -&gt; select customer/ partner -&gt; add a document</code></p></li>
<li><p>Flow<br />
&lt;<strong>luz_finance</strong>&gt;<br />
<code>-&gt; PartnerDocumentAreaProcess.mod</code><br />
<code>-&gt; PartnerDocumentController.prepareAndSavePartnerDocument(long partnerId, PartnerDocumentBean partnerDocumentBean) -&gt; saveDocumentFile(NewFileUploadBean fileBean)</code><br />
-&gt; <strong>calls luz_components</strong><br />
<code>-&gt; DocumentManager.insert(String path, File file)</code><br />
<code>-&gt; DocumentHelper.insertFile(File file, String destinationPath, String user)</code><br />
<br />
<br />
</p></li>
</ol></td>
<td><p>Call directly</p>
<p>Method:<br />
<code>DocumentHelper.insertFile(File file, String destinationPath, String user)</code></p></td>
<td><p>Input: String path, File file</p>
<p>Output: Long fileId</p></td>
</tr>
<tr>
<td><p>Delete document of Customer/ Partner</p></td>
<td><ol>
<li><p>Step by step<br />
<code>-&gt; Customers/ Partners -&gt; select customer/ partner -&gt; delete a document</code></p></li>
<li><p>Flow<br />
&lt;<strong>luz_finance</strong>&gt;<br />
<code>-&gt; PartnerDocumentAreaProcess.mod</code><br />
<code>-&gt; PartnerDocumentController.deletePartnerDocument(long partnerId, long partnerDocumentId, long dodumentServerId)</code><br />
<code>-&gt; PartnerDocumentContainer.deletePartnerDocument(long partnerId, long partnerDocumentId)</code><br />
-&gt; <strong>calls luzfin_finance (API)</strong><br />
<code>-&gt; PartnerDocumentResource.deletePartnerDocument(long companyId, long partnerId, long partnerDocumentId)</code><br />
<code>-&gt; IvyWebClientService.deletePartnerDocument(long companyId, long partnerId, long documentId)</code><br />
-&gt; <strong>calls luz_docs_process (API)</strong><br />
<code>-&gt; PartnerDocumentResource.deletePartnerDocumentById(BaseRequestParam baseRequestParam, long partnerId, long documentId)</code><br />
<code>-&gt; DocumentResourceHelper.delete(long documentId, String folder)</code><br />
<code>-&gt; DocumentResourceHelper.delete(long documentId, String folder)</code><br />
<code>-&gt; DocumentRestService.deleteDocumentByIdWithPathVerification(long documentId, String documentPath)</code><br />
-&gt; <strong>calls luz_components</strong><br />
<code>-&gt; FileManagerDocumentRepo.deleteDocument(DocumentOnServer document)</code></p></li>
</ol></td>
<td><p>Call via</p>
<p><code>PartnerDocumentContainer</code></p></td>
<td><p>Input: long partnerDocumentId</p>
<p>Output: <em>none</em></p></td>
</tr>
<tr>
<td><p>15</p></td>
<td><p><strong>luz_mobile</strong></p>
<p><strong>luz_docs_process</strong></p></td>
<td><p>Download document</p></td>
<td><ol>
<li><p>Step by step</p></li>
<li><p>Flow</p>
<ul>
<li><p><strong>&lt;luz_mobile&gt;</strong><br />
-&gt; call DocumentService.downloadDocument(<code>String tenantId, String companyId, String documentId</code>)<br />
-&gt; call LuzApiIvyRestClient.downloadDocument(<code>tenantId, companyId, documentId, REQUEST_HEADER_REQUEST_BY</code>): <code>{tenant-id}/companies/{company-id}/documents/{document-id}</code></p></li>
<li><p><strong>&lt;luz_docs_process&gt;</strong><br />
-&gt;call DocumentResource.getCompanyDocumentById(DownloadDocumentByIdRequestParam)<br />
-&gt; call DocumentRestService.getDocumentByIdWithPathVerification(long documentId, String documentPath)<br />
-&gt; call FileManagerDocumentRepo.getDocumentById(long fileid, boolean getJavaFile)<br />
-&gt; call FileManagerDocumentRepo.getDocuments(QueryParaml)<br />
-&gt; <strong>get file from FileManager</strong> call FileStoreDBHandler.getDocumentOnServerById(long fileid, boolean getJavaFile)</p></li>
</ul></li>
</ol></td>
<td><p>Call via <code>LuzApiIvyRestClient</code></p></td>
<td><p>Input: long fileid</p>
<p>Output: DocumentOnServer</p></td>
</tr>
<tr>
<td><p>16</p></td>
<td><p><strong>luz_mobile</strong></p>
<p><strong>luz_docs_process</strong></p></td>
<td><p>Download employee Document</p></td>
<td><ol>
<li><p>Step by step</p></li>
<li><p>Flow</p>
<ul>
<li><p><strong>&lt;luz_mobile&gt;</strong><br />
-&gt; call DocumentService.downloadEmployeeDocument(<code>String tenantId, String companyId, String employeeId, String documentId</code>)<br />
-&gt; call LuzApiIvyRestClient.downloadEmployeeDocument(tenantId, companyId, employeeId, documentId , REQUEST_HEADER_REQUEST_BY, HttpRequestHeaders.MEDIA_TYPE_APPLICATION_PDF): <code>{tenant-id}/companies/{company-id}/employees/{employee-id}/documents/{document-id}</code></p></li>
<li><p><strong>&lt;luz_docs_process&gt;</strong><br />
-&gt;call DocumentResource.getEmployeeDocumentById(EmployeeDownloadDocumentRequestParam, acceptHeader)<br />
-&gt; call DocumentRestService.getDocumentByIdWithPathVerification(long documentId, String documentPath)<br />
<strong>NOTE</strong>: documentPath = tenantId + companyId + employeeId<br />
-&gt; call FileManagerDocumentRepo.getDocumentById(long fileid, boolean getJavaFile)<br />
-&gt; call FileManagerDocumentRepo.getDocuments(QueryParaml)<br />
-&gt; <strong>get file from FileManager</strong> call FileStoreDBHandler.getDocumentOnServerById(long fileid, boolean getJavaFile)</p></li>
</ul></li>
</ol></td>
<td><p>Call via <code>LuzApiIvyRestClient</code></p></td>
<td><p>Input: long fileid</p>
<p>Output: DocumentOnServer</p></td>
</tr>
<tr>
<td><p>17</p></td>
<td><p><strong>luz_mobile</strong></p>
<p><strong>luz_docs_process</strong></p></td>
<td><p>Delete Document</p></td>
<td><ol>
<li><p>Step by step</p></li>
<li><p>Flow</p>
<ul>
<li><p><strong>&lt;luz_mobile&gt;</strong><br />
-&gt; call DocumentService.deleteDocument(<code>String tenantId, Long companyId, Long employeeId, Long documentId, String category</code>)<br />
-&gt; call LuzApiIvyRestClient.deleteDocument(tenantId, companyId, employeeId, documentId , REQUEST_HEADER_REQUEST_BY, HttpRequestHeaders.MEDIA_TYPE_APPLICATION_PDF): <code>{tenant-id}/companies/{company-id}/employees/{employee-id}/documents/{document-id}</code></p></li>
<li><p><strong>&lt;luz_docs_process&gt;</strong><br />
-&gt;call DocumentResource.deleteEmployeeDocumentById(EmployeeDeleteDocumentRequestParam)<br />
-&gt; call DocumentRestService.deleteDocumentByIdWithPathVerification(long documentId, String documentPath)<br />
-&gt; call FileManagerDocumentRepo.deleteDocument(DocumentOnServer)<br />
-&gt; call FileManagerDocumentRepo.getDocuments(QueryParaml)<br />
-&gt; <strong>delete file from FileManager</strong> call FileStoreDBHandler.deleteDocumentOnServer(DocumentOnServer)</p></li>
</ul></li>
</ol></td>
<td><p>Call via <code>LuzApiIvyRestClient</code></p></td>
<td><p>Input: DocumentOnServer</p>
<p>Output: ReturnedMessage</p></td>
</tr>
<tr>
<td><p>18</p></td>
<td><p>luz_store</p>
<p><strong>luz_docs_process</strong></p></td>
<td><p>Get document</p>
<p>Team: Wow</p></td>
<td><ol>
<li><p>Step by step</p></li>
<li><p>Flow<br />
&lt;<strong>luz_store</strong>&gt;<br />
-&gt; using IvyWebRestClient<br />
-&gt; endpoint: {tenant-id}/companies/{company-id}/documents/{document-id}<br />
-&gt; call to <strong>luz_docs_process</strong><br />
&lt;<strong>luz_docs_process&gt;</strong><br />
-&gt; call DocumentResource.getCompanyDocumentById(DownloadDocumentByIdRequestParam)<br />
-&gt; call DocumentRestService.getDocumentByIdWithPathVerification(long documentId, String documentPath)<br />
-&gt; call FileManagerDocumentRepo.getDocumentById(long fileid, boolean getJavaFile)<br />
-&gt; call FileManagerDocumentRepo.getDocuments(QueryParaml)</p></li>
</ol></td>
<td><p>Call via IvyWebRestClient</p></td>
<td><p>Input: String tenantId, Long companyId, long documentId</p>
<p>Output: byte[]</p></td>
</tr>
<tr>
<td><p>19</p></td>
<td><p>luz_store</p>
<p><strong>luz_docs_process</strong></p></td>
<td><p>Upload document</p>
<p>Team: Wow</p></td>
<td><ol>
<li><p>Step by step</p></li>
<li><p>Flow<br />
&lt;<strong>luz_store</strong>&gt;<br />
-&gt; using IvyWebRestClient<br />
-&gt; endpoint: {company-tenant-id}/companies/{company-id}/orders/{order-id}<br />
-&gt; or {company-tenant-id}/companies/{company-id}/orders/{order-id}/invoice-attachment-documents<br />
-&gt; call to <strong>luz_docs_process</strong><br />
-&gt; invoke method in OrderUploadedFileResource</p></li>
</ol></td>
<td><p>Call via IvyWebRestClient</p></td>
<td><p>Input: String tenantId, Long companyId, String orderId, MultipartFormDataOutput mdo</p>
<p>Output: <code>ch.klara.luz.store.rest.model.Document</code></p></td>
</tr>
<tr>
<td><p>20</p></td>
<td><p>luz_xhrm_processes</p></td>
<td><p>Upload file Mobiliar policy<br />
<br />
Team: Wow</p></td>
<td><ol>
<li><p>Step by step</p></li>
<li><p>Flow<br />
&lt;<strong>luz_xhrm_process</strong>&gt;<br />
-&gt; ChildrenPageProcess.mod/[call ws save children]<br />
-&gt; ChildrenInformationController.saveChildrenAndFile(ChildrenInformationComBean, List&lt;ChildContractInformationBean&gt;)<br />
-&gt; ChildrenInformationController.insertFiles(ChildrenInformationComBean, ChildContractInformationBean)<br />
-&gt; AdditionalChildrenInformationLogic.persistFilesForChildWhenContinue(List&lt;FileUpload&gt; files, String destinationPath)<br />
-&gt; DocumentHelper.insertFile(File file, String destinationPath, String user, Boolean shouldCreateThumbnail)<br />
-&gt; AbstractFileManagementHandler.insertFile(java.io.File _file, String _destinationPath,<br />
String _user)</p></li>
</ol></td>
<td><p>Call directly</p></td>
<td><p>Input:<br />
File: document.getJavaFile(), path: destinationPath, user: ""<br />
<br />
Output: fileId</p></td>
</tr>
<tr>
<td><p>21</p></td>
<td><p>luz_finance</p></td>
<td><p>Recurring invoice</p></td>
<td><ol>
<li><p>Step by step<br />
Order Management → Dashboard → Start recurring invoice run</p></li>
<li><p>Flow<br />
<code>RecurringInvoiceWidget.xhtml</code><br />
<code>-&gt; RecurringInvoiceWidget.sources.RecurringInvoiceWidgetHandler.startRecurringInvoiceRun()</code><br />
<code>-&gt; RecurringInvoiceContainer.excuteRecurringInvoiceRun(LocalDate runningDate, List&lt;String&gt; groups, boolean includeNoGroup, boolean notifyTomorrow)</code><br />
-&gt; call luzfin_finance (API)<br />
<code>-&gt; RecurringInvoiceResource.createRecurringInvoice(String tenantId, long companyId, LocalDate runningDate, List&lt;String&gt; groups, boolean includeNoGroup, boolean notifyTomorrow)</code><br />
<code>-&gt; RecurringInvoiceRunService.createInvoicesFromRecurringTemplatesByDate(String tenantId, long companyId, CreatingRecurringInvoiceParam param)</code><br />
<code>-&gt; ConvertTemplateToInvoiceService.updateAttachmentsInTemplates(String runAsRole, String authorization, String tenantId, Long companyId, List&lt;RecurringInvoiceTemplate&gt; templates)</code><br />
<code>-&gt; IvyWebClientService.getNewAttachments(String runAsRole, String authorization, String tenantId, long companyId, List&lt;OrderAttachment&gt; listOrderAttachments)</code><br />
-&gt; call luz_finance (API)<br />
<code>-&gt; PrintOrderTemplateResource.cloneAttachments(String companyTenantId, long companyId, List&lt;OrderAttachment&gt; attachments)</code><br />
<code>-&gt; OrderAttachmentService.cloneAttachmentInTheSameFolder(List&lt;OrderAttachment&gt; attachments)</code><br />
<code>-&gt; OrderAttachmentUtil.insertFileToDB(FileUpload fileUpload, OrderDetail orderDetail, AbstractFileManagementHandler fmh)</code><br />
<code>-&gt; AbstractFileManagementHandler.insertOneDocument(DocumentOnServer _document)</code></p></li>
</ol></td>
<td><p>Call directly</p></td>
<td><p>Input:<br />
DocumentOnServer</p></td>
</tr>
</tbody>
</table>

</div>
