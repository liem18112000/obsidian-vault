---
title: "KLARA Documents solution implementation"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20448090771/KLARA+Documents+solution+implementation
space: "LUZ"
topic: programming
relevance: 0.703
depth: 2.38
updated: 2017-05-30
attachments: 1
tags:
  - confluence
  - programming
  - space/luz
---

# KLARA Documents solution implementation

> [!info] Imported from Confluence
> Space **LUZ** · updated 2017-05-30 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20448090771/KLARA+Documents+solution+implementation)
> Relevance 0.703 · topic `programming`

# 

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="3096cf20-69df-419e-94ed-5f7e3081eb52" macro-name="toc">

</div>

  
Purpose

Since the existing information at the uploadedfiles table in filemanager database is not sufficient for KLARA document implementation, so this document is the way to implement KLARA Document described in <a href="https://axonivy.atlassian.net/wiki/display/LUZ/KLARA+Documents" rel="nofollow">KLARA_Documents</a>.

# Design

## Persistence

<div>

<table style="width: 32.4051%;">
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th colspan="3">document_general_property</th>
</tr>
&#10;<tr>
<td>id</td>
<td>Long</td>
<td><br />
</td>
</tr>
<tr>
<td>fileid</td>
<td>Long</td>
<td>reference fileid to the uploadedfiles table in filemanager database</td>
</tr>
<tr>
<td><span>company_id</span></td>
<td>Long</td>
<td><br />
</td>
</tr>
<tr>
<td>employee_id</td>
<td>Long</td>
<td><br />
</td>
</tr>
<tr>
<td>read_only</td>
<td>String (Enum)</td>
<td>True, false</td>
</tr>
<tr>
<td>category</td>
<td>String (Enum)</td>
<td>SALARY_STATEMENTS, YEARLY_REPORTS, PAYMENT_FILES, INSURANCE_CERTIFICATES, SALARY_TRANSMISSIONS, OWN_DOCUMENTS</td>
</tr>
<tr>
<td>owner</td>
<td>String (Enum)</td>
<td>EMPLOYEE, COMPANY, GENERAL, UNKNOWN</td>
</tr>
<tr>
<td>effective_date</td>
<td>Datetime</td>
<td><p>E.g. A payslip was created in 05.2017, but the effective date might in 05.2014. In case of needed we can use this column to query document by effective date.</p></td>
</tr>
</tbody>
</table>

</div>

**Description**

- This table will be created in luz-doc_manager database.
- The persistence will be implemented in luz_doc_manager project.

**Benefit**

- Easier to count the number of documents group by **category** column. (Mock-up No.1, and  No.4 in <a href="https://axonivy.atlassian.net/wiki/display/LUZ/KLARA+Documents" rel="nofollow">KLARA_Documents</a>)
- Easier to count the number of documents belong to an employee of a company by **tenant_id, company_id, employee_id** columns.
- Easier to distinguish the document belong to employee/company by **owner** column.
- Easier to determine whether the document could be deleted or not by **permission** column.
- Easier to filter or group documents by **effective_date** column**.**

# Usage

## Get documents

Call this service to get a list of fileid can satisfy the condition from mockup in <a href="https://axonivy.atlassian.net/wiki/display/LUZ/KLARA+Documents" rel="nofollow">KLARA_Documents</a>.

Put this list of fileid to query on uploadedfiles table in filemanager database.

## Insert new document

After inserting new row successfully in document_general_property, inserting the new document to uploadedfiles table.

# Migration exist data

We can execute the SQL statements below on uploadedfiles table to get the corresponding **category**, **owner**.

## Salary statements

**Company salary statements**

SELECT \* FROM uploadedfiles WHERE filepath LIKE 'luz/{0}/companies/{1}/documents/importance/Payslips\_%.pdf'

**Employee salary statements**

SELECT \* FROM uploadedfiles WHERE filepath LIKE 'luz/{0}/companies/{1}/employees/{2}/payslip/Payslip\_%.pdf'

## Company yearly reports

<span class="inline-comment-marker" ref="dee03540-bebc-452c-ad4e-7d4855e070be">SELECT \* FROM uploadedfiles WHERE filepath LIKE 'luz/{0}/companies/{1}/documents/reports/%.pdf'</span>

## Company payment files

SELECT \* FROM uploadedfiles WHERE filepath LIKE '"luz/{0}/companies//{1}/documents/importance/Payment\_%.txt"'

## Insurance certificates

Call an API in klee_event_broker to get all insurance certificates.

## Salary transmissions

**Company salary statements**

SELECT \* FROM uploadedfiles WHERE filepath LIKE 'luz/{0}/companies/{1}/documents/importance/transmissions/ELM-%.pdf'

**Employee salary statements**

SELECT \* FROM uploadedfiles WHERE filepath LIKE 'luz/{0}/companies/{1}/employees/{2}/documents/transmissions/ELM-%.pdf'

## Own documents

**Company**

SELECT \* FROM uploadedfiles WHERE filepath LIKE 'luz/{0}/companies/{1}/documents/%'  
AND fileid NOT IN (SELECT fileid FROM uploadedfiles WHERE filepath LIKE '%/importance/%')  
AND fileid NOT IN (SELECT fileid FROM uploadedfiles WHERE filepath LIKE '%/reports/%')

****Employee****

SELECT \* FROM uploadedfiles WHERE filepath LIKE 'luz/{0}/companies/{1}/employees/{2}/documents/%'  
AND fileid NOT IN (SELECT fileid FROM uploadedfiles WHERE filepath LIKE '%/payslip/%')  
AND fileid NOT IN (SELECT fileid FROM uploadedfiles WHERE filepath LIKE '%/transmissions/%')

## Meeting on the 30.05.2017

Team Next + Emmanuel

Team Next  
- bitbucket IvyAddons with the current used version  
- try to use the API for getting the Documents under some paths with some conditions  
- see what you exactly need as new methods: example filter&search for documents under some path which have a given Caegory (filetype) or some given filetags.  
- get used to the API: try to add some functionalities in the AbstractFileManagemntHandler (DocumentOnServer Controller & SQLPersistence...) and filetype or tags controller

klara_document api (luz_components)  
-- DocumentService  
\|-- configuration (BasicConfigurationController) = storeFilesInDb =true, activateFileType = true, activateFileTags = true  
\|-- FileManagementHandlersFactory -\> get AbstractFileManagementHandler (config)  
\|-- FileManagementHandlersFactory -\> get filetype and tag controllers

  
public List\<Document\> getAllDocumentsWithCategory(String tenantid, long companyid, String categoryName) {

fmh.getDocuments("luz/tenants/"+tenantid"+/companies/{company-id}/employees/{id}/%"

}

Emmanuel:  
- documentOnServer: add id / long and adapt the underlying API  
- create a DocumentWrapper : DocumentOnServer + fileTags  
- I help also by the API  
- I will get some info about the way we can release and have our fork ....
