---
ai_hash: 68a4571e5df4db71
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 6
depth: 2.82
entities: []
relevance: 0.736
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519728473/Analyze+N+1+queries+for+REST+API+calculate+payslips+for+overview+salary+processing
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: Analyze N+1 queries for REST API calculate payslips for overview salary processing
topic: programming
type: source
updated: 2021-01-07
---

# Analyze N+1 queries for REST API calculate payslips for overview salary processing

> [!info] Imported from Confluence
> Space **LUZ** · updated 2021-01-07 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519728473/Analyze+N+1+queries+for+REST+API+calculate+payslips+for+overview+salary+processing)
> Relevance 0.736 · topic `programming`

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th>Ticket</th>
<th><div class="content-wrapper">
<p><span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_20519728473_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-34099" data-macro-id="b13ee084-017e-45ba-90e0-87105b119b68" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-34099" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-34099</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span></p>
</div></th>
</tr>
&#10;<tr>
<td>Team</td>
<td><strong>WOW</strong></td>
</tr>
</tbody>
</table>

</div>

### **Outline:**

- **Overview**
- **Service call**
- **Access logs**
- **Query logs**
- **Analyze details and suggestion solution**

### **Overview of issue**

During calculate payslips for salary processing overview, we might have some N+1 query problems. This might effect to performance of this service.

### **Service call**

<span class="legacy-color-text-blue3">When a request to calculate hits the server, the following REST APIs are called:</span>

<span class="legacy-color-text-blue4">**/luz_compensation/api/{company-tenant-id}/companies/{company-id}/payslips?option=salary-processing-overview&latest=true&recalculate=true**</span>

### Access logs

The whole access log for this service call following the below logs:

<div hasbody="true" macro-id="3d3fb7a6-fef8-49a3-ba64-6c4b8cef4373" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

\[04/Jan/2021:09:31:47 +0700\] "GET /luz_person/api/states?ids=4 HTTP/1.1" 200 28  
\[04/Jan/2021:09:31:47 +0700\] "GET /luz_person/api/777bfa0f-eef1-4641-8041-6853d026f57f/companies?module=luz_compensation&ids=1%2C+6%2C+1 HTTP/1.1" 200 86  
\[04/Jan/2021:09:31:47 +0700\] "GET /luz_person/api/cities?ids=1113%2C+3158%2C+3121%2C+2040 HTTP/1.1" 200 38  
\[04/Jan/2021:09:31:47 +0700\] "GET /luz_person/api/states?ids=27 HTTP/1.1" 200 25  
\[04/Jan/2021:09:31:47 +0700\] "POST /luz_person/api/777bfa0f-eef1-4641-8041-6853d026f57f/persons/fetch HTTP/1.1" 200 144  
\[04/Jan/2021:09:31:52 +0700\] "GET /luz_compensation/api/777bfa0f-eef1-4641-8041-6853d026f57f/companies/1/payslips?option=salary-processing-overview&latest=true&recalculate=true HTTP/1.1" 200 5897

</div>

</div>

**Note: <span class="legacy-color-text-blue4">The environment having 15 employees.</span>**

### Query logs

<span class="legacy-color-text-blue3">The whole postgresql log for this service call following the attachment file <a href="../_attachments/20519728473-Salary_processing_overview_log.txt" data-nice-type="Text File">Salary_processing_overview_log.txt</a></span>

### **Analyze details and suggestion solution**

****1. N + 1****

- <span class="legacy-color-text-default">**Getting contracts of company (PayslipService.getContractsOfCompany)**</span>

The first step of calculate payslip for the salary processing overview is getting all contracts information of the company.

For each contract, it automatically trigger queries for getting employee, company, workpermit, civil info... of this contract.

Detail logs:  <a href="../_attachments/20519728473-SalaryProcessing_GetContracts.txt" data-nice-type="Text File">SalaryProcessing_GetContracts.txt</a>

<span class="legacy-color-text-red2">=\> Should get all contracts and related information at once(using entity graph/split query/customize query)</span>

- **Get person information in luz_person by uris(CompanyService.getEmployeeInfos =\>PersonService.findPersonsByURIs)**

**Note: Can refer to Other issues in [Analyze N+1 queries for REST API calculate payslip for 1 employee](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519727808/Analyze+N+1+queries+for+REST+API+calculate+payslip+for+1+employee)**

For each contract, we need person information. During getting person info by URIs, the person_reference info is triggered immediately by FetchType.EAGER


![[20519728473-image2021-1-4_10-48-2.png]]



Detail logs: <a href="../_attachments/20519728473-SalaryProcessing_GetPersonInfo_List.txt" data-nice-type="Text File">SalaryProcessing_GetPersonInfo_List.txt</a>

<span class="legacy-color-text-red2">=\> Should change to FetchType.Lazy, put it to entity graph to load it </span>

- **Get Tax Record(TaxService.findTaxRecords =\> TaxAtSourceService.getCantonsMayAffectToCalculation)**

Before getting tax data, we need to find which cantons from contracts effect to QST calculation.

Some N+1 queries trigger to get these cantons info.

Detail logs:<a href="../_attachments/20519728473-FindTaxRecord_Nplus1queries.txt" data-nice-type="Text File">FindTaxRecord_Nplus1queries.txt</a>

<span class="legacy-color-text-red2">=\> Load contract with enough data for extracting canton</span>

- **Getting salary configuration(during execute groovy for Salary Items)**

Salary configuration is loaded with list of salary item type, each salary item type will trigger another query to load itself.

Detail logs: <a href="https://jira.axonivy.com/confluence/download/attachments/265958081/Salary_configuration_Nplus1.txt?version=1&amp;modificationDate=1609310635000&amp;api=v2" class="external-link" rel="nofollow" style="text-decoration: none;">Salary_configuration_Nplus1.txt</a>

<span class="legacy-color-text-red2">=\></span> <span class="legacy-color-text-red2">Should get the salary configuration include salary item type at once(using entity graph/split query/customize query)</span>

- <span class="legacy-color-text-default">**Convert  ContractTemporal data(SalaryProcessingOverviewService.fulfillOverviewContractInfo)**</span>

<span class="legacy-color-text-default">After calculate payslip for all contracts, for each contract we need convert from entity to model.</span>

<span class="legacy-color-text-default">During convert contract temporal, some data accidental trigger N+1 query(costcenter, taxAtSource...)</span>

<span class="legacy-color-text-default">Detail logs: <a href="../_attachments/20519728473-ContractTemporalInfo_convertToModel.txt" data-nice-type="Text File">ContractTemporalInfo_convertToModel.txt</a></span>

<span class="legacy-color-text-red2">=\>   - Load contract temporal with with necessary data.</span>

<span class="legacy-color-text-red2">- Convert enough data for using.</span>

%% ai-graph-start %%

**Related notes:**
- [[Analyze N+1 queries for REST API calculate payslip for 1 employee]]
- [[Analyze performance for REST API calculate payslips for overview salary processing]]
- [[REST API calculate 1 employee's pay-slip]]
- [[Helios myKLARA app(luz-mobile) - API Response Performance Analysis]]
- [[N+1 hides at the service-call layer too, not just in the ORM]]

%% ai-graph-end %%