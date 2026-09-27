---
ai_hash: 27b8e4bcc9c21a94
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 1
depth: 3
entities: []
relevance: 0.792
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47374664204/Export+CRM+statistics+by+API
space: TS
status: reference
tags:
- confluence
- programming
- space/ts
title: Export CRM statistics by API
topic: programming
type: source
updated: 2023-05-10
---

# Export CRM statistics by API

> [!info] Imported from Confluence
> Space **TS** · updated 2023-05-10 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47374664204/Export+CRM+statistics+by+API)
> Relevance 0.792 · topic `programming`

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47374664204_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-98151" macro-id="6264af91-97db-41f9-9753-5373242ca2a0" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-98151" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-98151</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

1\. Get tenant token

<span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-current conf-macro output-inline" hasbody="false" macro-id="acaad7ee-f7e9-4bad-bb1a-5c31f3c5dd79" macro-name="status">POST</span> **\<endpoint\>**/luzsec/api/**\<tenant_id\>**/access/tokens

Basic Auth: `<username>` `<password>`

<div hasbody="true" macro-id="600f91d4-2ebe-4dd4-939c-1e45ba85cbbe" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

curl --request POST -u **\<username\>**:**\<password\>** "**\<endpoint\>**/luzsec/api/**\<tenant_id\>**/access/tokens"

</div>

</div>

=\> Username and password of CRON USER (the user gets a generic token)

=\> Get **token** in the response =\> `tenant_token`

2\. Get company data

<span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-progress conf-macro output-inline" hasbody="false" macro-id="9d9785d6-3301-40d6-94c7-d4fe9402652b" macro-name="status">GET</span> **\<endpoint\>**/luz_compensation/api/**\<tenant_id\>**/companies/1

Bearer Token: `tenant_token`

<div hasbody="true" macro-id="81d0c946-cf48-4029-9411-e75572f6ba73" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

curl "**\<endpoint\>**/luz_compensation/api/**\<tenant_id\>**/companies/1" --header "Authorization: Bearer **\<tenant_token\>**"

</div>

</div>

=\> Save response as JSON file =\> `company.json`

3\. Get invoice data

<span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-progress conf-macro output-inline" hasbody="false" macro-id="c90498ea-afa3-4492-8725-48668eac5f97" macro-name="status">GET</span> **\<endpoint\>**/luzfin_finance/api/**\<tenant_id\>**/companies/1/invoices-for-crm-report

Bearer Token: `tenant_token`

<div hasbody="true" macro-id="422ada86-51e4-4807-98cf-8697f0375784" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

curl "**\<endpoint\>**/luzfin_finance/api/**\<tenant_id\>**/companies/1/invoices-for-crm-report" --header "Authorization: Bearer **\<tenant_token\>**"

</div>

</div>

=\> Save response as JSON file =\> `invoice.json`

4\. Export CRM statistic

`kubectl port-forward --address 0.0.0.0 services/luz-reporting-dotnet 5000:5000 -n dev`

<span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-error conf-macro output-inline" hasbody="false" macro-id="acb78692-1376-4112-be2a-f16cfb39c96b" macro-name="status">LUZ_TEMPLATES</span> `data/template/luzReporting/CRMStatistics.mrt`

<span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-current conf-macro output-inline" hasbody="false" macro-id="5d85ef23-845a-47e7-9636-002e300bca2b" macro-name="status">POST</span> **\<endpoint\>**/luz_reporting/report/stimulsoft


![[47374664204-image-20230510-042914.png]]



<div hasbody="true" macro-id="09e66fb9-9eb3-47c2-9d55-5b9f272c0025" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

curl "**\<endpoint\>**/luz_reporting/report/stimulsoft" --form "report_template=@'**\<file_path\>**/CRMStatistics.mrt'“ --form "company=@'**\<file_path\>**/response-company.json'" --form "invoices=@'**\<file_path\>**/response-invoice.json'"

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[How to run export API for specific tenant and date - Manual export]]
- [[Employee Report Implementation (10.05.2023)]]
- [[14. Create companies by tenant id]]
- [[16. Export unsynchronized companies which missing from last synchronization]]
- [[15. Update companies by tenant id]]

%% ai-graph-end %%