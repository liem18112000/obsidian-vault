---
ai_hash: 8ca5646f3c4e4a0a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.56
entities: []
relevance: 0.775
source: https://axonivy.atlassian.net/wiki/spaces/TK/pages/30671885750/Intergration+test+for+MicroProfile+OpenAPI
space: TK
status: reference
tags:
- confluence
- programming
- space/tk
title: Intergration test for MicroProfile OpenAPI
topic: programming
type: source
updated: 2021-01-25
---

# Intergration test for MicroProfile OpenAPI

> [!info] Imported from Confluence
> Space **TK** · updated 2021-01-25 · [open original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/30671885750/Intergration+test+for+MicroProfile+OpenAPI)
> Relevance 0.775 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="f7f495e9-943c-47bf-8b61-9541d6c8b9a4" macro-name="toc" numberedoutline="true">

</div>

# Problem statement

Check that how to wirte intergration test for MicroProfile OpenAPI which using MinIO Server

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_30671885750_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-49192" macro-id="27a1e1ba-59ce-4daf-bc83-504649a6e5f0" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-49192" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-49192</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

  

# Analyze

API testing requires an application to interact with API. To test an API, you require two things:

- Testing Tool/Framework to drive the API
- Writing down your own code to test the API

<div>

<table style="width: 57.4168%;">
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
<td><br />
</td>
<td><strong>Api testing framework </strong></td>
<td><strong>Work with MinIO Server</strong></td>
<td><strong>AWS</strong></td>
<td><strong>GCS</strong></td>
<td>Links</td>
</tr>
<tr>
<td><span>Testcontainers<span> JUnit</span></span></td>
<td>Yes</td>
<td>Yes</td>
<td>Yes</td>
<td>?</td>
<td><a href="https://dzone.com/articles/an-efficient-object-storage-for-junit-tests" class="external-link" rel="nofollow">https://dzone.com/articles/an-efficient-object-storage-for-junit-tests</a></td>
</tr>
<tr>
<td><p>Google in-memory emulator</p></td>
<td>No</td>
<td>?</td>
<td>?</td>
<td>Yes</td>
<td><h3 id="IntergrationtestforMicroProfileOpenAPI-TestingcodethatusesStorage">Testing code that uses Storage</h3>
<h3 id="IntergrationtestforMicroProfileOpenAPI-https://github.com/googleapis/google-cloud-java/blob/master/TESTING.md#testing-code-that-uses-storage"><a href="https://github.com/googleapis/google-cloud-java/blob/master/TESTING.md#testing-code-that-uses-storage" class="external-link" rel="nofollow">https://github.com/googleapis/google-cloud-java/blob/master/TESTING.md#testing-code-that-uses-storage</a></h3>
<p><a href="https://www.programcreek.com/java-api-examples/?api=com.google.api.services.storage.Storage" class="external-link" rel="nofollow">https://www.programcreek.com/java-api-examples/?api=com.google.api.services.storage.Storage</a></p></td>
</tr>
<tr>
<td>Cucumber</td>
<td>Yes</td>
<td>?</td>
<td>Yes</td>
<td>Yes</td>
<td><br />
</td>
</tr>
<tr>
<td>Citrus Framework</td>
<td>Yes</td>
<td>?</td>
<td>Yes</td>
<td>Yes</td>
<td><br />
</td>
</tr>
<tr>
<td>REST-Assured</td>
<td>Yes</td>
<td>?</td>
<td>Yes</td>
<td>Yes</td>
<td><br />
</td>
</tr>
<tr>
<td>Karate</td>
<td>Yes</td>
<td>?</td>
<td>Yes</td>
<td>Yes</td>
<td><br />
</td>
</tr>
</tbody>
</table>

</div>

# **Result**

<div>

<table style="width: 52.4763%;">
<colgroup>
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
</colgroup>
<tbody>
<tr>
<th><br />
</th>
<th><br />
</th>
<th><br />
</th>
<th><br />
</th>
<th><br />
</th>
<th><br />
</th>
<th><br />
</th>
<th><br />
</th>
<th><br />
</th>
<th><br />
</th>
</tr>
&#10;<tr>
<td>Updating...</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[One API end to end testing]]
- [[Load test]]
- [[Test and code review report template.2.93]]
- [[06 - How to test a Rest API with authorization]]
- [[Test and code review report template.2]]

%% ai-graph-end %%