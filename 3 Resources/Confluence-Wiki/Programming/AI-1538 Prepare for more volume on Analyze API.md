---
title: "AI-1538 Prepare for more volume on Analyze API"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/48682237968/AI-1538+Prepare+for+more+volume+on+Analyze+API
space: "AI"
topic: programming
relevance: 0.79
depth: 2.76
updated: 2025-09-24
attachments: 0
tags:
  - confluence
  - programming
  - space/ai
---

# AI-1538 Prepare for more volume on Analyze API

> [!info] Imported from Confluence
> Space **AI** · updated 2025-09-24 · [open original](https://axonivy.atlassian.net/wiki/spaces/AI/pages/48682237968/AI-1538+Prepare+for+more+volume+on+Analyze+API)
> Relevance 0.79 · topic `programming`

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_48682237968_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="AI-1538" macro-id="cf396bfc-5dc3-443f-a648-78c319420ad7" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/AI-1538" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>AI-1538</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

*See also:* [Impact Analysis - ePost to Post App onboarding](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48664215700/Impact+Analysis+-+ePost+to+Post+App+onboarding) , and [ePost goes 2+M](https://axonivy.atlassian.net/wiki/spaces/IO/pages/48659136631/ePost+goes+2+M)

# Introduction

On 10.11.2025 the new Post app will be released with ePost embedded. The Post app has 2M users, and on average 200.000 sessions per day (with peak of 300.000 sessions).

# Paths leading to load on Analyze API

All Post app users will be at least *Basic* epost users. Other users can upgrade to *Standard* or *Premium* users.

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
<th><p><strong>ID</strong></p></th>
<th><p><strong>User journey</strong></p></th>
<th><p><strong>Path to Analyze API</strong></p></th>
<th><p><strong>ePost app release peak impact estimate</strong></p></th>
<th><p><strong>Components affected</strong></p></th>
<th><p><strong>Issues / Questions</strong></p></th>
</tr>
&#10;<tr>
<td><p>J0</p></td>
<td><p>New user is created</p></td>
<td><p>No path via Analyze API</p>
<p>but via Rhine API</p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="0516817d-1897-4c38-b652-0def296fc9b7" data-macro-name="status">LOW</span> (real-time user record ingestion can be switched off and should be non-blocking)</p></td>
<td><ul>
<li><p>Flink</p></li>
</ul></td>
<td><p><em>Questions</em></p>
<p>Do we get a User record for new ePost users? (checking data, yes we do but data looks wrong - does not match hubspot numbers)</p></td>
</tr>
<tr>
<td><p>J1</p></td>
<td><p><em>Basic</em> user sign up to be <em>Standard</em> or <em>Premium</em> user</p></td>
<td><p>This will add two welcome letters to the user’s letterbox</p>
<p>These documents will be enriched via luz_doc by Analyze API</p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-error conf-macro output-inline" data-hasbody="false" data-macro-id="8c9976b7-d19b-461b-b500-d1ea8d2e9beb" data-macro-name="status">HIGH</span></p>
<p>The app per default shows the “standard” offer, it is for free so likely many users select that.</p></td>
<td><ul>
<li><p>Basic PUs</p></li>
<li><p>Flink processing</p></li>
</ul></td>
<td><p>Welcome letters are currently very heavy to process.</p>
<ul>
<li><p>1 sec for “Welcome to ePost”</p></li>
<li><p>10 sec for “Scanning service letter”</p></li>
</ul></td>
</tr>
<tr>
<td><p>J2</p></td>
<td><p><em>Basic</em> user uploads (scan) a document to the letterbox</p></td>
<td><p>These documents will be enriched via luz_doc by Analyze API</p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-current conf-macro output-inline" data-hasbody="false" data-macro-id="acf02e28-52c2-431b-8c8f-927cdd963eea" data-macro-name="status">MEDIUM</span></p>
<p>All users can scan and upload documents. Many might try that in the beginning.</p></td>
<td><ul>
<li><p>Basic PUs</p></li>
<li><p>Flink</p></li>
</ul>
<p>If scanned</p>
<ul>
<li><p>OCR service</p></li>
</ul>
<p>If invoice</p>
<ul>
<li><p>Invoice processing (with call to uid.admin)</p></li>
</ul></td>
<td><p>Would make sense to add daily limit or similar on scans per tenants to avoid costly use of the OCR</p></td>
</tr>
<tr>
<td><p>J3</p></td>
<td><p><em>Standard</em> / <em>Premium</em> user receives / uploads new documents in the letterbox</p></td>
<td><p>These documents will be enriched via luz_doc by Analyze API</p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="eb03b051-3838-4459-bb9c-bc43719cd747" data-macro-name="status">LOW</span></p>
<p>This is a 2nd order effect, as most of these letter first will come later.</p></td>
<td><ul>
<li><p>Basic PUs</p></li>
<li><p>Flink</p></li>
</ul>
<p>If scanned</p>
<ul>
<li><p>OCR service</p></li>
</ul>
<p>If invoice</p>
<ul>
<li><p>Invoice processing (with call to uid.admin)</p></li>
</ul></td>
<td></td>
</tr>
<tr>
<td><p>J4</p></td>
<td><p><em>Premium</em> user receives documents via the scancenter</p></td>
<td><p>These documents are process by Analyze API for tenant id identification</p>
<p>Later the documents are enriched via luz_doc by Analyze API</p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="d21a1c03-1d9a-4481-9dd3-29313aaa1076" data-macro-name="status">LOW</span></p>
<p>This is a 2nd order effect, as most of these letter first will come later.</p></td>
<td><ul>
<li><p>Basic PUs</p></li>
<li><p>Flink</p></li>
<li><p>Envelope model</p></li>
<li><p>Magazine model</p></li>
<li><p>ePost scancenter API (for tenant id lookup)</p></li>
<li><p>Matching API (for tenant id lookup)</p></li>
</ul></td>
<td><p>Note that identity matching may slow downbwith more tenants in the DB (to test)</p></td>
</tr>
</tbody>
</table>

</div>

# Volume estimate

Rough estimate of volume. It is difficult to predict how the new users will behave, so here we try to give estimate at the higher end of what we expect. Goal is not a precise estimate but to give a hint about the magnitudes.

(August - September)

Post app users about 2M

Current ePost users: about 270k

Scancenter to Analyze API hourly peak: median 1.2k (max 1.6k) jobs submitted

Luz_Doc to Analyze API hourly peak : median 1.4k (max 5.9k) jobs submitted

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>ID</strong></p></th>
<th><p><strong>Rough estimate of volume</strong></p></th>
<th><p><strong>Comments</strong></p></th>
</tr>
&#10;<tr>
<td><p>J0</p></td>
<td><p>roughly 2M new users</p>
<p>Estimating an overlap of current ePost users and app users to 70k</p>
<p>ie. roughly 1.9M-2M new users</p></td>
<td><p><strong>Flink</strong>: 2M new user records ingested over X period</p></td>
</tr>
<tr>
<td><p>J1</p></td>
<td><p>2M users are given the choice to select account type - “Standard” is shown by default and is for free &gt; expect high selection rate &gt; 90%</p>
<p>Estimating an overlap of current ePost users and app users to 70k</p>
<p>ie. 2M *0.9 - 70k = 1.7M new users</p>
<p>This will trigger two welcome letters</p>
<p><strong>1.7M *2 = 3.4 M letters</strong></p></td>
<td><p>Welcome letters:</p>
<p>Analyze API: 3.4 M job requests over X days</p>
<p><strong>Flink:</strong> ?</p></td>
</tr>
<tr>
<td><p>J2</p></td>
<td><p>Total users after release:<br />
Post App users 2M<br />
Est exiting ePost app users = 70k<br />
Existing ePost users = 270k</p>
<p>= 2M -70k + 270k = <strong>2.2M total users</strong></p>
<p>Ie. 2.2M/270k = <strong>a factor 8.1 more volume</strong></p>
<p><strong>Relevant documents are only “scanned”</strong></p>
<p>luz-docs 1.9% are “scanned”</p>
<p>ie. 1.4k * 0.019 = 27 scanned / hour in peak hour &gt; *8.1 = 219 scanned/hour in peak hour (estimated)</p>
<p>Total scanned in August: 5977 documents</p></td>
<td><p>This would mean about 5977*8.1 = <span style="background-color: rgb(253,208,236);">48k OCR document processed per month</span></p>
<p>Mainly affects OCR webservice</p>
<p><strong>Flink:</strong> ?</p></td>
</tr>
<tr>
<td><p>J3</p></td>
<td><p>Same calculation as for Basic users, but 10% do not convert to Standard / Premium</p>
<p>2M *0.9 + 270k -70k = <strong>2M total Standard/Premium users</strong></p>
<p>i.e. 2M/270k = <strong>a factor 7.4 more volume</strong></p></td>
<td><p>Current load peak: median 1.4k job in peak</p>
<p>1.4k*7.4 = 10.4k jobs /hour in peak hour</p>
<p>about 23% jobs processed “as invoice” = 2.4k job/hour in peak hour</p>
<p><strong>uid.admin requests</strong> &gt; via NAT gateway → we currently peak at 21 open connections &gt; about 21*7.4 = 155 connections (rough guess). That should be <strong>OK.</strong></p>
<p><strong>Flink:</strong> ?</p></td>
</tr>
<tr>
<td><p>J4</p></td>
<td><p>roughly 10k scanning users</p>
<p>assuming same conversion rate</p>
<p>10k/270k * 2M = <strong>74k scanning users</strong></p>
<p>74k/10k = <strong>a factor 7.4 more volume</strong></p></td>
<td><p>Current load peak: median 1.2k job in peak</p>
<p>1.2k*7.4 = 8.9k jobs /hour in peak hour</p>
<p>(overlap with luz-docs peak - 19.2k job/hour peak)</p>
<p><strong><span style="background-color: rgb(253,208,236);">Envelope model: current about 10% response are 504 in peak (issue!)</span></strong></p>
<p><strong>Magazine model: ? (not deployed on prod yet)</strong></p>
<p><strong>ePost scancenter API (for tenant id lookup):</strong> via PSC - should not be limiting factor <strong></strong></p>
<p><strong>Matching API (for tenant id lookup):</strong> via NAT gateway &gt; see above - should be <strong>OK</strong></p>
<p><strong>Flink:</strong> ?</p></td>
</tr>
</tbody>
</table>

</div>
