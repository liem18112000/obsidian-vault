---
ai_hash: 763cfd2763137451
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 2
depth: 3
entities: []
relevance: 0.984
source: https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/47453503617/Research+Design+architecture+concept+for+the+service+to+generate+the+Generic+Interface+File
space: HACKA
status: reference
tags:
- confluence
- architecture
- space/hacka
title: 'Research: Design architecture concept for the service to generate the Generic
  Interface File'
topic: architecture
type: source
updated: 2023-08-11
---

# Research: Design architecture concept for the service to generate the Generic Interface File

> [!info] Imported from Confluence
> Space **HACKA** · updated 2023-08-11 · [open original](https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/47453503617/Research+Design+architecture+concept+for+the+service+to+generate+the+Generic+Interface+File)
> Relevance 0.984 · topic `architecture`

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="bb8ffa3a-6791-4b26-82c9-97117853b767" macro-name="toc">

</div>

# 1-Overview

Regarding the story: <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47453503617_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-103471" macro-id="a251389a-6805-4698-81ac-3bb0119491d7" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-103471" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-103471</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

This confluence describes the architecture concept for the service to generate the generic interface file.

The goal <span class="inline-comment-marker" ref="0c9ecd64-a336-435f-8339-aff45bb23be9">of this story</span> is to have an architecture concept for the service to generate a generic interface JSON document

# 2-Service and parameters

### 2.1-Concept

In the scope of this story:

An API will be exposed in luz_compensation to collect necessary data and then generating the Generic Interface File

Out of scope:

Later, the file may be saved to DB<span class="inline-comment-marker" ref="2ab409f2-96cd-4b50-920a-76c603fa22cd">.</span>

### 2.2-Where the service is located:

luz_compensation

### 2.3-How to use the serice:

<span class="inline-comment-marker" ref="bcb3ed23-7ab1-42aa-914e-ae591d38bac9">It can be used via </span><span class="inline-comment-marker" ref="9f0991bf-8c06-4fcd-96a6-94c0963bbbb6"><span class="inline-comment-marker" ref="bcb3ed23-7ab1-42aa-914e-ae591d38bac9">API</span></span>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="91c01bc9-09ad-4230-8005-07943751e6c0" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
GET /{company-tenant-id}/companies/{company-id}/accounting-interfaces/generic-interface-document
```

</div>

</div>

### 2.4-Input parameters:

Overall, we need:

- tenant id: String

- company id: Long

- salary run id: Long

- payslip ids: List\<Long\>

- <span class="inline-comment-marker" ref="d1a593b1-ce22-4c8a-9430-4bf8a7ce682f"><span class="inline-comment-marker" ref="25d8386f-0b98-4ca1-af4b-eadb4d35f7c5">language: String/Enum (EN/DE/IT/FR)</span></span>

Note:

Because salaryRunId is EXCLUSIVE OR payslipIds, if both of them exist in the parameter, a validation exception will be thrown

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ecddef06-d9f1-4f2a-9f6f-ab4fb9b26fda" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Code snippet

public Response generateGenericInterfaceFile(@BeanParam GenericInterfaceFileRequest request) {
...
}

public GenericInterfaceFileRequest {
    @HeaderParam("Accept-Language")
    @DefaultValue("DE")
    private String language;
    
    @Parameter(description = "Tenant id", required = true) 
    @PathParam("company-tenant-id") 
    private String tenantId;
    
    @Parameter(description = "Company id", required = true) 
    @PathParam("company-id") 
    private Long companyId;
    
    @QueryParam("salary-run-id")
    private Long salaryRunId;
    
    @QueryParam("payslip-ids")
    private List<Long> payslipIds;
}
```

</div>

</div>

# 3-The process flow


![[47453503617-Proccess of creating Generic Interface File.png]]



# 4-Which queries/services/APIs will be used

Find company

`/api/{company-tenant-id}/companies/{companyId}`

Find salary run

`/api/{company-tenant-id}/companies/{companyId}/salary-runs/{salaryRunId}`

Find payslip

`/api/{company-tenant-id}/companies/{companyId}/payslips`

Find contract

`/api/{company-tenant-id}/companies/{companyId}/contracts/{contractId}`

Find employee

`/api/{company-tenant-id}/companies/{companyId}/employees`

Note:

Currently, payslip API does not support find by list of payslipIds

The time to finish this process must be less than <span class="inline-comment-marker" ref="4996ed2c-5b31-47e6-ac25-60565c927c83">10s for 200 employees.</span>

# 5-The output of the service

<span class="inline-comment-marker" ref="a453f124-9ba6-4f15-85be-33193e1d2ee3">The output of the service is a JSON document</span>

To be more specific, we can reference [Generic Interface JSON file](https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/47453143184/Generic+Interface+JSON+file)

%% ai-graph-start %%

**Related notes:**
- [[Generic Interface JSON file]]
- [[Analyze performance for REST API calculate payslips for overview salary processing]]
- [[Analyze N+1 queries for REST API calculate payslip for 1 employee]]
- [[Analyze N+1 queries for REST API calculate payslips for overview salary processing]]
- [[How to call generic interface document API on dev]]

%% ai-graph-end %%