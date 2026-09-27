---
title: "Optimus: ePost/myLife app(luz_mylife_epost_adapter) - API Response Performance Analysis"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47491350668/Optimus+ePost+myLife+app+luz_mylife_epost_adapter+-+API+Response+Performance+Analysis
space: "FUT"
topic: programming
relevance: 0.891
depth: 3
updated: 2023-10-10
attachments: 10
tags:
  - confluence
  - programming
  - space/fut
---

# Optimus: ePost/myLife app(luz_mylife_epost_adapter) - API Response Performance Analysis

> [!info] Imported from Confluence
> Space **FUT** · updated 2023-10-10 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47491350668/Optimus+ePost+myLife+app+luz_mylife_epost_adapter+-+API+Response+Performance+Analysis)
> Relevance 0.891 · topic `programming`

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
<th><p><strong>API</strong></p></th>
<th><p><strong>Tracing sample</strong></p></th>
<th><p><strong>Short analysis</strong></p></th>
<th><p><strong>Solutions</strong></p></th>
<th><p><strong>Status</strong></p></th>
</tr>
&#10;<tr>
<td></td>
<td><p>@POST</p>
<p><code>app.epost.ch/luz_mylife_epost_adapter/api/v2/user/login</code></p>
<hr />
<p>last 7 days:</p>
<p>&gt;10s: 12 log entries</p></td>
<td></td>
<td><p><a href="https://console.cloud.google.com/traces/list?referrer=search&amp;authuser=0&amp;hl=vi&amp;project=klara-nonprod&amp;pageState=(%22traceIntervalPicker%22:(%22groupValue%22:%22P4D%22,%22customValue%22:null),%22traceFilter%22:(%22chips%22:%22%255B%257B_22k_22_3A_22SpanName_22_2C_22t_22_3A10_2C_22v_22_3A_22_5C_22Recv.%252Fluz_mylife_epost_adapter%252Fapi%252Fv2%252Fuser%252Flogin_5C_22_22_2C_22i_22_3A_22span_22%257D%255D%22))&amp;start=1695106270498&amp;end=1695107939403&amp;tid=6294d89cee726becd0b481bace116d48&amp;spanId=ada5977bdc73e699" class="external-link" rel="nofollow">https://console.cloud.google.com/traces/list?referrer=search&amp;authuser=0&amp;hl=vi&amp;project=klara-nonprod&amp;pageState=("traceIntervalPicker":("groupValue":"P4D","customValue":null),"traceFilter":("chips":"%255B%257B_22k_22_3A_22SpanName_22_2C_22t_22_3A10_2C_22v_22_3A_22_5C_22Recv.%252Fluz_mylife_epost_adapter%252Fapi%252Fv2%252Fuser%252Flogin_5C_22_22_2C_22i_22_3A_22span_22%257D%255D"))&amp;start=1695106270498&amp;end=1695107939403&amp;tid=6294d89cee726becd0b481bace116d48&amp;spanId=ada5977bdc73e699</a></p></td>
<td><ul>
<li><p>@PATCH /luz_profile/api/v2/{company-tenant-id}/profile</p>
<ul>
<li><p>@GET /luz_tenant_dir/api/tenant-entries/{participant-id} N+1 queries Select verification address</p></li>
<li><p>@PUT /luz_tenant_dir/api/tenant-entries/{participant-id}: too much queries delete from address and delete from verification</p></li>
</ul></li>
<li><p>@POST /luzsec/api/refreshtokens/initial</p>
<ul>
<li><p>post to keycloak takes long time. It is general issue → there is another story for it.</p></li>
</ul></li>
<li><p>@POST /luzsec/api/refreshtokens/tenants/{tenant-id}</p>
<ul>
<li><p>findIndividualTenant 2 times</p></li>
</ul></li>
</ul></td>
<td></td>
<td><p>READY_TO_DELIVER</p>
<p>Optimus</p></td>
</tr>
<tr>
<td></td>
<td><p>@POST</p>
<p><code>app.epost.ch/luz_mylife_epost_adapter/api/v2/user/tenants</code></p>
<hr />
<p>last 7 days:</p>
<p>&gt;10s: 14 log entries</p></td>
<td></td>
<td><p><a href="https://console.cloud.google.com/traces/list?referrer=search&amp;authuser=0&amp;hl=vi&amp;project=klara-nonprod&amp;pageState=(%22traceIntervalPicker%22:(%22groupValue%22:%22P1D%22,%22customValue%22:null),%22traceFilter%22:(%22chips%22:%22%255B%257B_22k_22_3A_22SpanName_22_2C_22t_22_3A10_2C_22v_22_3A_22_5C_22Recv.%252Fluz_mylife_epost_adapter%252Fapi%252Fv2%252Fuser%252Ftenants_5C_22_22_2C_22i_22_3A_22span_22%257D%255D%22))&amp;start=1695106270498&amp;end=1695107939403&amp;tid=8cd7b6a01289d303df82b2a269df904a&amp;spanId=8d0dd8aabcfd5857" class="external-link" rel="nofollow">https://console.cloud.google.com/traces/list?referrer=search&amp;authuser=0&amp;hl=vi&amp;project=klara-nonprod&amp;pageState=("traceIntervalPicker":("groupValue":"P1D","customValue":null),"traceFilter":("chips":"%255B%257B_22k_22_3A_22SpanName_22_2C_22t_22_3A10_2C_22v_22_3A_22_5C_22Recv.%252Fluz_mylife_epost_adapter%252Fapi%252Fv2%252Fuser%252Ftenants_5C_22_22_2C_22i_22_3A_22span_22%257D%255D"))&amp;start=1695106270498&amp;end=1695107939403&amp;tid=8cd7b6a01289d303df82b2a269df904a&amp;spanId=8d0dd8aabcfd5857</a></p></td>
<td><ul>
<li><p>call to hubspot took a lot of time. It should be asyn</p></li>
<li><p>why get profile then post → should make only one call to create if not existed</p></li>
</ul></td>
<td></td>
<td><p>READY_TO_DELIVER</p>
<p>Optimus</p></td>
</tr>
<tr>
<td></td>
<td><p>@GET app.epost.ch/luz_mylife_epost_adapter/api/v2/{tenant-id}/profile</p>
<hr />
<p>last 7 days:</p>
<p>&gt;10s: 6 log entries, but 1 of them was error 500</p></td>
<td></td>
<td><p><a href="https://console.cloud.google.com/traces/list?referrer=search&amp;authuser=0&amp;hl=vi&amp;project=klara-nonprod&amp;pageState=(%22traceIntervalPicker%22:(%22groupValue%22:%22P7D%22,%22customValue%22:null),%22traceFilter%22:(%22chips%22:%22%255B%257B_22k_22_3A_22SpanName_22_2C_22t_22_3A10_2C_22v_22_3A_22_5C_22Recv.%252Fluz_mylife_epost_adapter%252Fapi%252Fv2%252F%257Btenant-id%257D%252Fprofile_5C_22_22_2C_22i_22_3A_22span_22%257D%255D%22))&amp;start=1695106270498&amp;end=1695107939403&amp;tid=315bcb9a420076cbb3fa8bdb9dbf0b36&amp;spanId=bd43bdcdf8029039" class="external-link" rel="nofollow">https://console.cloud.google.com/traces/list?referrer=search&amp;authuser=0&amp;hl=vi&amp;project=klara-nonprod&amp;pageState=("traceIntervalPicker":("groupValue":"P7D","customValue":null),"traceFilter":("chips":"%255B%257B_22k_22_3A_22SpanName_22_2C_22t_22_3A10_2C_22v_22_3A_22_5C_22Recv.%252Fluz_mylife_epost_adapter%252Fapi%252Fv2%252F%257Btenant-id%257D%252Fprofile_5C_22_22_2C_22i_22_3A_22span_22%257D%255D"))&amp;start=1695106270498&amp;end=1695107939403&amp;tid=315bcb9a420076cbb3fa8bdb9dbf0b36&amp;spanId=bd43bdcdf8029039</a></p></td>
<td><ul>
<li><p>many calls to luztenant</p></li>
<li><p>get security class is not fast</p></li>
</ul>

![[47491350668-image-20230920-112614.png]]


<p>Why there is @POST /luz_jsonstore/api/mdb/{tenant-id}/{collection}? Is it search? If yes, is there any better search?</p></td>
<td></td>
<td><p>READY_TO_DELIVER</p>
<p>Optimus</p></td>
</tr>
<tr>
<td></td>
<td><p>@GET https://app.klara.ch/luz_mylife_epost_adapter/api/{tenant-id}/documents/{document-id}/files/reference</p>
<hr />
<p>last 7 days:</p>
<p>a lot of log entries are more than 10s</p></td>
<td></td>
<td></td>
<td>

![[47491350668-image-20230919-073746.png]]

![[47491350668-image-20230919-073831.png]]

![[47491350668-image-20230919-073910.png]]

![[47491350668-image-20230919-073944.png]]


<p>After analysis and research:</p>
<ul>
<li><p>the request hits luz-mylife-epost-adapter and luz-docs nearly at the same time → there is no queue up.</p></li>
<li><p>Not sure the file is streamed immediately via multiple clients or it has to wait to receive the whole file in each sub call.</p></li>
</ul></td>
<td><p>need to prove and find solution before giving to teams</p></td>
<td><p>NOT_READY_TO_DELIVER</p></td>
</tr>
<tr>
<td></td>
<td><p>@GET</p>
<p>https://app.epost.ch/luz_mylife_epost_adapter/api/v2/{tenant-id}/payment/all-accounts</p>
<hr />
<p>last 7 days:</p>
<p>&gt;10s: 23 log entries</p></td>
<td></td>
<td><p><a href="https://console.cloud.google.com/traces/list?referrer=search&amp;authuser=0&amp;hl=vi&amp;project=klara-nonprod&amp;pageState=(%22traceIntervalPicker%22:(%22groupValue%22:%22P7D%22,%22customValue%22:null),%22traceFilter%22:(%22chips%22:%22%255B%257B_22k_22_3A_22SpanName_22_2C_22t_22_3A10_2C_22v_22_3A_22_5C_22Recv.%252Fluz_mylife_epost_adapter%252Fapi%252Fv2%252F%257Btenant-id%257D%252Fpayment%252Fall-accounts_5C_22_22_2C_22i_22_3A_22span_22%257D%255D%22))&amp;start=1695024933464&amp;end=1695044241329&amp;tid=e143b0d1edead070609498f1186c5ce1" class="external-link" rel="nofollow">https://console.cloud.google.com/traces/list?referrer=search&amp;authuser=0&amp;hl=vi&amp;project=klara-nonprod&amp;pageState=("traceIntervalPicker":("groupValue":"P7D","customValue":null),"traceFilter":("chips":"%255B%257B_22k_22_3A_22SpanName_22_2C_22t_22_3A10_2C_22v_22_3A_22_5C_22Recv.%252Fluz_mylife_epost_adapter%252Fapi%252Fv2%252F%257Btenant-id%257D%252Fpayment%252Fall-accounts_5C_22_22_2C_22i_22_3A_22span_22%257D%255D"))&amp;start=1695024933464&amp;end=1695044241329&amp;tid=e143b0d1edead070609498f1186c5ce1</a></p></td>
<td><p>Most of the time is taken by a sub call to luz-cor-api: <a href="http://luz-corapi:8080/luz_cor_api/api/40ab5821-9163-4105-8202-2b63708f0248/ais/all-accounts" class="external-link" rel="nofollow">http://luz-corapi:8080/luz_cor_api/api/40ab5821-9163-4105-8202-2b63708f0248/ais/all-accounts</a></p>
<p>luz_corapi is using wildlfy 21 build 19, which is not applied tracing(wildlfy 21 build 24).</p></td>
<td></td>
<td><p>NOT_READY_TO_DELIVER</p></td>
</tr>
<tr>
<td></td>
<td><p>@GET app.epost.ch/luz_mylife_epost_adapter/api/{tenant-id}/letters/badge-count</p>
<hr />
<p>last 7 days:</p>
<p>64,860 log entries<br />
&gt;1s: 9,619 log entries<br />
&gt;10s: 204 log entries</p></td>
<td></td>
<td><p><a href="https://console.cloud.google.com/traces/list?referrer=search&amp;authuser=0&amp;hl=vi&amp;project=klara-nonprod&amp;pageState=(%22traceIntervalPicker%22:(%22groupValue%22:%22P1D%22,%22customValue%22:null),%22traceFilter%22:(%22chips%22:%22%255B%257B_22k_22_3A_22SpanName_22_2C_22t_22_3A10_2C_22v_22_3A_22_5C_22Recv.%252Fluz_mylife_epost_adapter%252Fapi%252F%257Btenant-id%257D%252Fletters%252Fbadge-count_5C_22_22_2C_22i_22_3A_22span_22%257D%255D%22))&amp;start=1695106270498&amp;end=1695107939403&amp;tid=a335640fdd07090bb9af0917d024e7e7" class="external-link" rel="nofollow">https://console.cloud.google.com/traces/list?referrer=search&amp;authuser=0&amp;hl=vi&amp;project=klara-nonprod&amp;pageState=("traceIntervalPicker":("groupValue":"P1D","customValue":null),"traceFilter":("chips":"%255B%257B_22k_22_3A_22SpanName_22_2C_22t_22_3A10_2C_22v_22_3A_22_5C_22Recv.%252Fluz_mylife_epost_adapter%252Fapi%252F%257Btenant-id%257D%252Fletters%252Fbadge-count_5C_22_22_2C_22i_22_3A_22span_22%257D%255D"))&amp;start=1695106270498&amp;end=1695107939403&amp;tid=a335640fdd07090bb9af0917d024e7e7</a></p></td>
<td>

![[47491350668-image-20230920-075218.png]]


<p>Those unknown gaps are from normal java code execution,<br />
in the above case, because the pod just started up,<br />
it needs a bit of time to warm up, so it took some time for the resource and injected services to init.</p>
<p><a href="https://console.cloud.google.com/logs/query;query=resource.type%3D%22k8s_container%22%0Aresource.labels.project_id%3D%22klara-nonprod%22%0Aresource.labels.location%3D%22europe-west6-a%22%0Aresource.labels.cluster_name%3D%22klara-nonprod%22%0Aresource.labels.namespace_name%3D%22dev%22%0Alabels.k8s-pod%2Fapp%3D%22luz-mylife-epost-adapter%22%0A--%20%22version%22%20OR%20%22badge-count%22;pinnedLogId=2023-09-19T13:13:45.636310527Z%2Fokyx89t026rlfm7g;cursorTimestamp=2023-09-19T13:13:52.634502194Z;startTime=2023-09-19T11:01:00.000Z;endTime=2023-09-19T14:13:00.000Z?project=klara-nonprod" class="external-link" rel="nofollow">https://console.cloud.google.com/logs/query;query=resource.type%3D"k8s_container" resource.labels.project_id%3D"klara-nonprod" resource.labels.location%3D"europe-west6-a" resource.labels.cluster_name%3D"klara-nonprod" resource.labels.namespace_name%3D"dev" labels.k8s-pod%2Fapp%3D"luz-mylife-epost-adapter" -- "version" OR "badge-count";pinnedLogId=2023-09-19T13:13:45.636310527Z%2Fokyx89t026rlfm7g;cursorTimestamp=2023-09-19T13:13:52.634502194Z;startTime=2023-09-19T11:01:00.000Z;endTime=2023-09-19T14:13:00.000Z?project=klara-nonprod</a></p></td>
<td></td>
<td><p>NOT_READY_TO_DELIVER</p></td>
</tr>
<tr>
<td></td>
<td><p>@POST <code>app.epost.ch/luz_mylife_epost_adapter/api/notification/register-device</code></p>
<hr />
<p>last 7 days:</p>
<p>&gt;10s: 4 log entries, but 2 of them were error 500</p></td>
<td></td>
<td><p><a href="https://console.cloud.google.com/traces/list?referrer=search&amp;authuser=0&amp;hl=vi&amp;project=klara-nonprod&amp;pageState=(%22traceIntervalPicker%22:(%22groupValue%22:%22P7D%22,%22customValue%22:null),%22traceFilter%22:(%22chips%22:%22%255B%257B_22k_22_3A_22SpanName_22_2C_22t_22_3A10_2C_22v_22_3A_22_5C_22Recv.%252Fluz_mylife_epost_adapter%252Fapi%252Fnotification%252Fregister-device_5C_22_22_2C_22i_22_3A_22span_22%257D%255D%22))&amp;start=1695106270498&amp;end=1695107939403&amp;tid=8c05d4ed1be625c8072f8ee264db33c8&amp;spanId=ece1ac6a98220327" class="external-link" rel="nofollow">https://console.cloud.google.com/traces/list?referrer=search&amp;authuser=0&amp;hl=vi&amp;project=klara-nonprod&amp;pageState=("traceIntervalPicker":("groupValue":"P7D","customValue":null),"traceFilter":("chips":"%255B%257B_22k_22_3A_22SpanName_22_2C_22t_22_3A10_2C_22v_22_3A_22_5C_22Recv.%252Fluz_mylife_epost_adapter%252Fapi%252Fnotification%252Fregister-device_5C_22_22_2C_22i_22_3A_22span_22%257D%255D"))&amp;start=1695106270498&amp;end=1695107939403&amp;tid=8c05d4ed1be625c8072f8ee264db33c8&amp;spanId=ece1ac6a98220327</a></p></td>
<td><p>most of the time spend for call @POST /luzsec/api/refreshtokens/refresh</p></td>
<td></td>
<td><p>NOT_READY_TO_DELIVER</p></td>
</tr>
</tbody>
</table>

</div>
