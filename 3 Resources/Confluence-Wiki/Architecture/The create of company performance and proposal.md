---
title: "The create of company performance and proposal"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20490587347/The+create+of+company+performance+and+proposal
space: "LUZ"
topic: architecture
relevance: 0.746
depth: 2.44
updated: 2019-06-11
attachments: 2
tags:
  - confluence
  - architecture
  - space/luz
---

# The create of company performance and proposal

> [!info] Imported from Confluence
> Space **LUZ** · updated 2019-06-11 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20490587347/The+create+of+company+performance+and+proposal)
> Relevance 0.746 · topic `architecture`

## Investing performance of the create company when clicking save button to finish step took around 18s, i check on my local(there is the difference from local and server but if i can improve on my local then server is the same result)

  

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
<th>Script plate</th>
<th>Script plate do</th>
<th>Time used in seconds</th>
<th>Proposal</th>
<th><p>time used in seconds after improvement</p></th>
</tr>
&#10;<tr>
<td>collect session user data for<br />
Hubspot JS submission</td>
<td>Send data to Hubspot </td>
<td>1.5</td>
<td>Should move to signal or move to finish step</td>
<td>0</td>
</tr>
<tr>
<td>do migration</td>
<td>Perform migrate all modules</td>
<td>7.5</td>
<td><div class="content-wrapper">
<p>Remove because we have ticket : <span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_20490587347_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-22772" data-macro-id="c5745c6e-1365-4ecb-9fc3-0ac4cac66ff7" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-22772" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-22772</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>  no need to perform migrate by manually<br />
the API create compnay will migrate 3 modules(luz_person, luz_compensation, luzfin_finance)</p>
</div></td>
<td>0</td>
</tr>
<tr>
<td>save register company</td>
<td>Call API to create company, upload logo, company vat</td>
<td>7.3</td>
<td><div class="content-wrapper">
<p>- <strong>Call API company it took around 5,3s</strong></p>
<p><br />
Create company from xent_rest :POST /rest/api/779205f5-ce7e-4ac5-9767-142bead65af5/companies HTTP/1.1 status-code=200 bytes-sent=3975 <strong>time-consuming=3680</strong><br />
-&gt; xent_rest just consume API of luz_compensation :POST /luz_compensation/api/779205f5-ce7e-4ac5-9767-142bead65af5/companies HTTP/1.1 status-code=200 bytes-sent=3727 <strong>time-consuming=2509</strong>-&gt; the service create company: find company again after created company , should check the reason.</p>
<p>Why <strong>xent_rest</strong> took much time → should check.<br />
<br />
Create company vat :POST /luzfin_finance/api/779205f5-ce7e-4ac5-9767-142bead65af5/companies/1/company-vats/default-for-new-company HTTP/1.1 status-code=200 bytes-sent=- time-consuming=1560<br />
-&gt; should not call this step, <strong>we can call later such as : finish step, when go to accounting screen or screen relate to company vat</strong><br />
<br />
- <strong>Call API upload logo 2s</strong><br />
-&gt; should not call this step, we can call later or after the process finish</p>
</div></td>
<td>4 or 5</td>
</tr>
<tr>
<td>GenerateTokenForRestAuthentication</td>
<td>Generate Token</td>
<td>1.5</td>
<td>Should remove second call, it is duplicate, i already test then there is no error</td>
<td>0.75</td>
</tr>
</tbody>
</table>

</div>

  

- **The proposal overview of whole process create company** **and the result around 7s, if we can reduce time to call API create company(luz_compensation,xent_rest) then the result around 3 to 5s**
