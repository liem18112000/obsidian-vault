---
title: "Helios: myKLARA app(luz-mobile) - API Response Performance Analysis"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47514419201/Helios+myKLARA+app+luz-mobile+-+API+Response+Performance+Analysis
space: "FUT"
topic: programming
relevance: 0.864
depth: 3
updated: 2023-12-04
attachments: 12
tags:
  - confluence
  - programming
  - space/fut
---

# Helios: myKLARA app(luz-mobile) - API Response Performance Analysis

> [!info] Imported from Confluence
> Space **FUT** · updated 2023-12-04 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47514419201/Helios+myKLARA+app+luz-mobile+-+API+Response+Performance+Analysis)
> Relevance 0.864 · topic `programming`

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
<th><p><strong>Priority</strong></p></th>
<th><p><strong>Nginx log</strong></p></th>
<th><p><strong>API / tracing sample</strong></p></th>
<th><p><strong>Short analysis</strong></p></th>
<th><p><strong>Solutions</strong></p></th>
<th><p><strong>Status</strong></p></th>
<th><p><strong>Team/ticket</strong></p></th>
</tr>
&#10;<tr>
<td></td>
<td><p>@GET https://app.klara.ch/luz_mobile/api/contract-working-time/{tenant-id}/companies/1/employees/1/contracts/1/?isExcludeTransferred=false&amp;firstResult=40&amp;maxResult=21&amp;t=1694638281852</p>
<hr />
<p>last 7 days:</p>
<p>&gt;10s: 34 entries</p></td>
<td><p>@GET /luz_mobile/api/contract-working-time/{company-tenant-id}/companies/{companyId}/employees/{employeeId}/contracts/{contractId}</p>
<p><strong>Tracing sample</strong></p>
<p><a href="https://console.cloud.google.com/traces/list?authuser=0&amp;hl=vi&amp;project=klara-nonprod&amp;start=1695027988469&amp;end=1695031588469&amp;pageState=(%22traceFilter%22:(%22chips%22:%22%255B%255D%22))&amp;tid=6f63c5e793852d3bc757c6d9c5ee8f81" class="external-link" rel="nofollow">https://console.cloud.google.com/traces/list?authuser=0&amp;hl=vi&amp;project=klara-nonprod&amp;start=1695027988469&amp;end=1695031588469&amp;pageState=("traceFilter":("chips":"%255B%255D"))&amp;tid=6f63c5e793852d3bc757c6d9c5ee8f81</a></p></td>
<td><ul>
<li><p>/luz_compensation/api/{tenant-id}/companies/{company-id}/employees/{employee-id}/contracts/active many select queries: select company, select workplace, select cost center…</p></li>
<li><p>many calls to luz-compensation, and the call /luz_compensation/api/{tenan-id}/companies/{id}/contracts/{id}/payslips?latestOnly=true are duplicated many times</p></li>
</ul>

![[47514419201-image-20230918-101848.png]]


<ul>
<li><p>the call @GET /luz_compensation/api/{tenan-id}/companies/{id}/contracts/{id}/payslips?latestOnly=true took long time. Does it calculate something?</p></li>
</ul>

![[47514419201-image-20231016-051915.png]]

</td>
<td><ul>
<li><p>improve API /luz_compensation/api/{tenant-id}/companies/{company-id}/employees/{employee-id}/contracts/active to get enough necessary data</p></li>
<li><p>reduce calls to luz_compensation</p></li>
<li><p>improve API @GET /luz_compensation/api/{tenan-id}/companies/{id}/contracts/{id}/payslips?latestOnly=true. After querying database, it took a lot of time to group and sort</p></li>
</ul></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-complete conf-macro output-inline" data-hasbody="false" data-macro-id="3e387387-844e-47f4-9b55-5abc13d09833" data-macro-name="status">RESOLVED</span></p></td>
<td><p><span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47514419201_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="MYK-8694" data-macro-id="171ef5dc-30dc-4dc1-9154-35de57153757" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/MYK-8694" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>MYK-8694</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span></p>
<p>status will be updated in the story</p>
<p><span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47514419201_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="MYK-8745" data-macro-id="b61cbd20-3059-4048-bb68-6a8922b249a4" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/MYK-8745" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>MYK-8745</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span></p></td>
</tr>
<tr>
<td></td>
<td><p>@GET</p>
<p><code>app.klara.ch/luz_mobile/api/online-booking/{company-tenant-id}/companies/{company-id}/appointments/{id}/thumbnail</code></p>
<hr />
<p>last 7 days:</p>
<p>&gt;10s: 251 log entries</p></td>
<td></td>
<td><ul>
<li><p>transfer refresh token to access token always take time. But it is general performance issue.</p></li>
<li><p>get all subscriptions may cause performance issue because it has to load all subscriptions and all their relevant data. But what we need is to know the tenant already subscribes a specific widget or not.</p>
<ul>
<li><p>→ subscriptions already cached within a day, take no action with this</p>

![[47514419201-image-20231012-094451.png]]

</li>
</ul></li>
</ul>

![[47514419201-image-20230921-115916.png]]


<ul>
<li><p>find the partner, then find the partner thumbnail. Why don’t directly find the partner thumbnail?</p>
<ul>
<li><p>→ data get from booking flow, it does not contain info in detail, therefore, it’s a must to go around like this. Basically, partners have appointment found in luz-booking, then we also need the thumbnail after that. Anyway, Helios will continue to investigate more to find another way if we could.</p></li>
</ul></li>
</ul>

![[47514419201-image-20230921-120441.png]]


<ul>
<li><p>the thumbnail is not streamed in the correct way</p>
<ul>
<li><p>→ the function also support for very old version of myKlara, so it need to be separate in 2 kinds of response content, Helios will check to see if we could remove the old content.</p></li>
<li><p>However, the final endpoint still ivy side, from it, we might have the same situation at KLARA level</p></li>
</ul></li>
</ul>

![[47514419201-image-20230921-120544.png]]

</td>
<td></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh conf-macro output-inline" data-hasbody="false" data-macro-id="60e7386b-0950-4abf-9582-9ca97ba2889b" data-macro-name="status">TODO</span></p></td>
<td><p><span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47514419201_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="MYK-8720" data-macro-id="46cbfa89-7455-47f3-9675-e4cd8a98b068" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/MYK-8720" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>MYK-8720</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span></p>
<p><span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47514419201_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="MYK-8746" data-macro-id="b8d5f9c6-47a8-482b-893e-f36e1c9d33b3" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/MYK-8746" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>MYK-8746</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span></p></td>
</tr>
<tr>
<td></td>
<td><p>@GET</p>
<p><code>app.klara.ch/luz_mobile/api/online-booking/{company-tenant-id}/companies/{company-id}/resources/{id}/thumbnail?type=Freelancer&amp;t=1695290426379</code></p>
<hr />
<p>last 7 days:</p>
<p>&gt;10s: 310 log entries</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td><p>@GET</p>
<p><code>app.klara.ch/luz_mobile/api/customers/{company-tenant-id}/companies/{company-id}/partner-notes/note-status?partner-ids=41&amp;partner-ids=...&amp;client-current-time=2023-09-21T12%3A40%3A32Z&amp;t=1695292832773 </code></p>
<hr />
<p>last 7 days:</p>
<p>&gt;10s: 7 log entries</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td><p>@POST</p>
<p><code>app.klara.ch/luz_mobile/api/online-booking/{company-tenant-id}/companies/{company-id}/appointments?t=1695290426854</code></p>
<hr />
<p>last 7 days:</p>
<p>&gt;10s: 4 log entries, but 1 got status 499</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

</div>
