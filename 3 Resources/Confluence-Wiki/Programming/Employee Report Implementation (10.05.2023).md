---
title: "Employee Report Implementation (10.05.2023)"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47365259851/Employee+Report+Implementation+10.05.2023
space: "LUZ"
topic: programming
relevance: 0.755
depth: 2.81
updated: 2023-05-10
attachments: 12
tags:
  - confluence
  - programming
  - space/luz
---

# Employee Report Implementation (10.05.2023)

> [!info] Imported from Confluence
> Space **LUZ** · updated 2023-05-10 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47365259851/Employee+Report+Implementation+10.05.2023)
> Relevance 0.755 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="55b29d87-5f4d-4f20-9026-41ab644f0e97" macro-name="toc">

</div>

# Overview

Curently, we have 4 module that related to Employee Report Implementation as below:


![[47365259851-image-20230510-035525.png]]



**luz_report_web**: front end (Ivy) of reporting <a href="https://bitbucket.org/axonivy-prod/luz_report_web/src" class="external-link" rel="nofollow">axonivy-prod / luz_report_web — Bitbucket</a>.

**luz_report_service**: back end service of reporting <a href="https://bitbucket.org/axonivy-prod/luz_report_service/src/master/" class="external-link" rel="nofollow">axonivy-prod / luz_report_service — Bitbucket</a>.

**DB luzreport**: it is database will store data of reporting in luz-database.

**luz_compenstation**: it will provide all employee data for reporting <a href="https://bitbucket.org/axonivy-prod/luz_compensation/src/master/" class="external-link" rel="nofollow">axonivy-prod / luz_compensation — Bitbucket</a>.

# Dashboard

### View mode


![[47365259851-image-20230510-040900.png]]




![[47365259851-image-20230510-034813.png]]



#### **Ivy**

ReportingDashboard.xhtml

<a href="https://bitbucket.org/axonivy-prod/luz_report_web/src/master/src_hd/ch/klara/luz/report/dashboard/ReportingDashboard/ReportingDashboard.xhtml" class="external-link" rel="nofollow">axonivy-prod / luz_report_web / src_hd / ch / klara / luz / report / dashboard / ReportingDashboard / ReportingDashboard.xhtml — Bitbucket</a>

#### **Backend API**

**<u>GET</u>** <u>luz_report_service/companies/{company-id}/reports</u>

- Service <a href="https://bitbucket.org/axonivy-prod/luz_report_service/src/1b25761f59c21a03a35e841e70f51120aea78c76/src/main/java/ch/klara/luz/report/service/ReportService.java#lines-110" class="external-link" rel="nofollow">axonivy-prod / luz_report_service / src / main / java / ch / klara / luz / report / service / ReportService.java — Bitbucket</a> - reportService.java - `getReports(Long companyId, String language)`

# Master Report


![[47365259851-image-20230510-040939.png]]



### View mode


![[47365259851-image-20230510-034907.png]]



#### Ivy

ReportingDialog.xhtml - <a href="https://bitbucket.org/axonivy-prod/luz_report_web/src/master/src_hd/ch/klara/luz/report/dashboard/ReportingDialog/ReportingDialog.xhtml" class="external-link" rel="nofollow">axonivy-prod / luz_report_web / src_hd / ch / klara / luz / report / dashboard / ReportingDialog / ReportingDialog.xhtml — Bitbucket</a>

#### Backend API (luz_report_service)

**GET** luz_report_service/companies/{company-id}/master-data

<a href="https://bitbucket.org/axonivy-prod/luz_report_service/src/master/src/main/java/ch/klara/luz/report/datasources/EmployeeDataSourceService.java" class="external-link" rel="nofollow">axonivy-prod / luz_report_service / src / main / java / ch / klara / luz / report / datasources / EmployeeDataSourceService.java — Bitbucket</a> EmployeeDataSourceService.java - `getAllData(DataSourceCriteria param)`

DataSourceCriteria param


![[47365259851-image-20230510-045402.png]]




![[47365259851-image-20230510-045324.png]]



#### Backend API (luz_compensation)

**<u>GET</u>** <u>luz_compensation/{company-tenant-id}/companies/{company-id}/employees/report</u>


![[47365259851-image-20230510-045551.png]]



<a href="https://bitbucket.org/axonivy-prod/luz_compensation/src/f66039438140e30645cedb88a1d61730f37c26af/src/main/java/com/axonivy/compensation/service/EmployeeService.java#lines-1764" class="external-link" rel="nofollow">axonivy-prod / luz_compensation / src / main / java / com / axonivy / compensation / service / EmployeeService.java — Bitbucket</a> - `getAllEmployeeDataForReport(EmployeeReportFilter filterParam)`

# Normal Report


![[47365259851-image-20230510-041105.png]]



### View mode / Edit mode


![[47365259851-image-20230510-035220.png]]



#### Ivy

same as dialog of **Master report**

#### Backend API

**GET** luz_report_service/companies/{company-id}/{report-id}/columns/


![[47365259851-image-20230510-093741.png]]



<a href="https://bitbucket.org/axonivy-prod/luz_report_service/src/1b25761f59c21a03a35e841e70f51120aea78c76/src/main/java/ch/klara/luz/report/service/ReportColumnService.java#lines-22" class="external-link" rel="nofollow">axonivy-prod / luz_report_service / src / main / java / ch / klara / luz / report / service / ReportColumnService.java — Bitbucket</a> ReportColumnService.java - `getColumnsByReportId(long reportId, String language)`

**GET** luz_report_service/companies/{company-id}/master-data

We will get all columns that selected from user to Ivy and all master data from luz_compenstation to Ivy

After that Ivy will filter out the data that need to show in report

**(THIS IS TEMPORARY APPROACH - WE WILL ENHANCE THAT ONLY SEND FILTERED DATA TO IVY)**

We already made this logic in Export report feature.

### Save report

**POST** luz_report_service//companies/{company-id}/reports


![[47365259851-image-20230510-094125.png]]



<a href="https://bitbucket.org/axonivy-prod/luz_report_service/src/1b25761f59c21a03a35e841e70f51120aea78c76/src/main/java/ch/klara/luz/report/service/ReportService.java#lines-68" class="external-link" rel="nofollow">axonivy-prod / luz_report_service / src / main / java / ch / klara / luz / report / service / ReportService.java — Bitbucket</a> ReportService.java - `createReport(long companyId, Report report)`
