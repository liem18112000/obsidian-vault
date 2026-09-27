---
ai_hash: 7a734bab63475424
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 20
depth: 2.81
entities: []
relevance: 0.724
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47494365263/Get+tenant+token+from+public+api
space: FUT
status: reference
tags:
- confluence
- programming
- space/fut
title: Get tenant token from public api
topic: programming
type: source
updated: 2023-09-25
---

# Get tenant token from public api

> [!info] Imported from Confluence
> Space **FUT** · updated 2023-09-25 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47494365263/Get+tenant+token+from+public+api)
> Relevance 0.724 · topic `programming`

![[47494365263-image-20230920-020245.png]]



<a href="https://console.cloud.google.com/logs/query;query=--%20resource.type:%2528http_load_balancer%2529%20AND%20jsonPayload.enforcedSecurityPolicy.name:%2528prod-security-policy%2529%0A--%20httpRequest.latency%20%3C%20500ms%0A--%20httpRequest.latency%20%3E%20100s%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A%2528httpRequest.requestUrl:%20%22https:%2F%2Fapi.klara.ch%2Fcore%2Flatest%2Ftoken%22%20OR%20textPayload%20%3D~%20%22%2Fcore%2Flatest%2Ftoken%22%2529%0A--%20httpRequest.requestUrl:%2528%22api.klara.ch%22%20OR%20%22api.epost.ch%22%2529%20--%20public%20api;pinnedLogId=2023-09-13T09:27:12.934404411Z%2Fpvy6na297ahhfp9z;lfeCustomFields=;cursorTimestamp=2023-09-13T09:25:53.293977578Z;startTime=2023-09-13T09:24:00.000Z;endTime=2023-09-13T10:27:00.000Z?project=klara-prod" class="external-link" rel="nofollow">Query</a> to get this request in LB and related requests in Kong and public-api-adapter  
Note: It can only based on status code and time. No other ways to map request and it consequences


![[47494365263-image-20230920-020540.png]]



<a href="https://console.cloud.google.com/logs/query;query=--%20resource.type:%2528http_load_balancer%2529%20AND%20jsonPayload.enforcedSecurityPolicy.name:%2528prod-security-policy%2529%0A--%20httpRequest.latency%20%3C%20500ms%0A--%20httpRequest.latency%20%3E%20100s%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A%2528textPayload%3D~%22%2Fpublic-api-tokens%22%20AND%20textPayload%3D~%22time-consuming%3D%5Cd%7B5,%7D%22%2529%0A%0A--%20httpRequest.requestUrl:%2528%22api.klara.ch%22%20OR%20%22api.epost.ch%22%2529%20--%20public%20api;pinnedLogId=2023-09-13T09:27:12.934404411Z%2Fpvy6na297ahhfp9z;lfeCustomFields=;cursorTimestamp=2023-09-13T09:27:12.933739969Z;startTime=2023-09-13T09:24:00.000Z;endTime=2023-09-13T10:27:00.000Z?project=klara-prod" class="external-link" rel="nofollow">Query</a> to get request to jwt-service


![[47494365263-image-20230920-022001.png]]



<a href="https://console.cloud.google.com/logs/query;query=--%20resource.type:%2528http_load_balancer%2529%20AND%20jsonPayload.enforcedSecurityPolicy.name:%2528prod-security-policy%2529%0A--%20httpRequest.latency%20%3C%20500ms%0A--%20httpRequest.latency%20%3E%20100s%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20%2528textPayload%3D~%22%2Fpublic-api-tokens%22%20AND%20textPayload%3D~%22time-consuming%3D%5Cd%7B4,%7D%22%20AND%20textPayload%3D~%22status-code%3D400%22%2529%0A%2528textPayload%3D~%22Could%20not%20get%20token%20from%20keycloak%22%20OR%20textPayload%3D~%22Code%20is%20broken%22%2529%0A%0A--%20httpRequest.requestUrl:%2528%22api.klara.ch%22%20OR%20%22api.epost.ch%22%2529%20--%20public%20api;pinnedLogId=2023-09-13T09:27:12.934404411Z%2Fpvy6na297ahhfp9z;lfeCustomFields=;cursorTimestamp=2023-09-13T09:27:12.932886387Z;startTime=2023-09-13T09:24:00.000Z;endTime=2023-09-13T10:27:00.000Z?project=klara-prod" class="external-link" rel="nofollow">Error log</a> in jwt-service


![[47494365263-image-20230920-023503.png]]

![[47494365263-image-20230920-023556.png]]



## → **Analyze why cannot connect to keycloak**

Keycloak high load during the error time

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>During error</strong></p></th>
<th><p><strong>Last 1 day</strong></p></th>
<th></th>
</tr>
&#10;<tr>
<td>

![[47494365263-image-20230920-080917.png]]


<p><a href="https://console.cloud.google.com/logs/query;query=--%20resource.type:%2528http_load_balancer%2529%20AND%20jsonPayload.enforcedSecurityPolicy.name:%2528prod-security-policy%2529%0A--%20httpRequest.latency%20%3C%20500ms%0A--%20httpRequest.latency%20%3E%2050s%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20%2528textPayload%3D~%22%2Fpublic-api-tokens%22%20AND%20textPayload%3D~%22time-consuming%3D%5Cd%7B4,%7D%22%20AND%20textPayload%3D~%22status-code%3D400%22%2529%0A--%20%2528textPayload%3D~%22Could%20not%20get%20token%20from%20keycloak%22%20OR%20textPayload%3D~%22Code%20is%20broken%22%2529%0A--%20textPayload%3D~%22%2Fauth%2Frealms%2F%22%0A-textPayload%3D~%22Warning:%20Nashorn%20engine%20is%20planned%20to%20be%20removed%20from%20a%20future%20JDK%20release%22%0A--%20httpRequest.requestUrl:%2528%22api.klara.ch%22%20OR%20%22api.epost.ch%22%2529%20--%20public%20api%0A--%20%2528resource.labels.container_name%3D~%22keycloak%22%2529%0A--%20-httpRequest.status%3D200%0Alabels.%22k8s-pod%2Fapp%22%3D%22login-nginx-ingress%22;pinnedLogId=2023-09-13T09:27:12.934404411Z%2Fpvy6na297ahhfp9z;lfeCustomFields=;cursorTimestamp=2023-09-13T09:27:08.970434548Z;startTime=2023-09-12T10:27:00.000Z;endTime=2023-09-13T10:27:00.000Z?project=klara-prod" class="external-link" rel="nofollow">Log</a></p></td>
<td>

![[47494365263-image-20230920-080844.png]]


<p><a href="https://console.cloud.google.com/logs/query;query=--%20resource.type:%2528http_load_balancer%2529%20AND%20jsonPayload.enforcedSecurityPolicy.name:%2528prod-security-policy%2529%0A--%20httpRequest.latency%20%3C%20500ms%0A--%20httpRequest.latency%20%3E%2050s%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20%2528textPayload%3D~%22%2Fpublic-api-tokens%22%20AND%20textPayload%3D~%22time-consuming%3D%5Cd%7B4,%7D%22%20AND%20textPayload%3D~%22status-code%3D400%22%2529%0A--%20%2528textPayload%3D~%22Could%20not%20get%20token%20from%20keycloak%22%20OR%20textPayload%3D~%22Code%20is%20broken%22%2529%0A--%20textPayload%3D~%22%2Fauth%2Frealms%2F%22%0A-textPayload%3D~%22Warning:%20Nashorn%20engine%20is%20planned%20to%20be%20removed%20from%20a%20future%20JDK%20release%22%0A--%20httpRequest.requestUrl:%2528%22api.klara.ch%22%20OR%20%22api.epost.ch%22%2529%20--%20public%20api%0A--%20%2528resource.labels.container_name%3D~%22keycloak%22%2529%0Alabels.%22k8s-pod%2Fapp%22%3D%22login-nginx-ingress%22%0A--%20-httpRequest.status%3D200;pinnedLogId=2023-09-13T09:27:12.934404411Z%2Fpvy6na297ahhfp9z;lfeCustomFields=;cursorTimestamp=2023-09-20T08:04:28.207551994Z;duration=P1D?project=klara-prod" class="external-link" rel="nofollow">Log</a></p></td>
<td><p>Not high load</p></td>
</tr>
<tr>
<td>

![[47494365263-image-20230920-083006.png]]

</td>
<td></td>
<td><p><a href="https://console.cloud.google.com/monitoring/dashboards/builder/1f28b10f-66cd-49a8-8425-4d39393fb2a3;startTime=2023-09-13T06:42:03.560Z;endTime=2023-09-13T13:13:37.415Z?project=klara-prod" class="external-link" rel="nofollow">CPU usage</a> of keycloak very small</p></td>
</tr>
<tr>
<td></td>
<td></td>
<td><p>other?</p></td>
</tr>
</tbody>
</table>

</div>


![[47494365263-image-20230921-083701.png]]



→ Use MP Rest client, use custom config timeout (connect: 200ms, read timeout: 3s)

Some other attempts:

Analyze log of <a href="https://console.cloud.google.com/logs/query;query=--%20resource.type:%2528http_load_balancer%2529%20AND%20jsonPayload.enforcedSecurityPolicy.name:%2528prod-security-policy%2529%0A--%20httpRequest.latency%20%3C%20500ms%0A--%20httpRequest.latency%20%3E%2050s%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20%2528textPayload%3D~%22%2Fpublic-api-tokens%22%20AND%20textPayload%3D~%22time-consuming%3D%5Cd%7B4,%7D%22%20AND%20textPayload%3D~%22status-code%3D400%22%2529%0A--%20%2528textPayload%3D~%22Could%20not%20get%20token%20from%20keycloak%22%20OR%20textPayload%3D~%22Code%20is%20broken%22%2529%0A--%20textPayload%3D~%22%2Fauth%2Frealms%2F%22%0A-textPayload%3D~%22Warning:%20Nashorn%20engine%20is%20planned%20to%20be%20removed%20from%20a%20future%20JDK%20release%22%0A--%20httpRequest.requestUrl:%2528%22api.klara.ch%22%20OR%20%22api.epost.ch%22%2529%20--%20public%20api%0A--%20%2528resource.labels.container_name%3D~%22keycloak%22%2529%0A--%20-httpRequest.status%3D200%0Alabels.%22k8s-pod%2Fapp%22%3D%22login-nginx-ingress%22;pinnedLogId=2023-09-13T09:27:12.934404411Z%2Fpvy6na297ahhfp9z;lfeCustomFields=;cursorTimestamp=2023-09-13T09:26:20.441853468Z;startTime=2023-09-12T10:27:00.000Z;endTime=2023-09-13T10:27:00.000Z?project=klara-prod" class="external-link" rel="nofollow">login-nginx-ingress or keycloak</a> during the error time → nothing found

**→ Still not find root cause of the issue connection timeout.**

## Analyze time-consuming for requests took 1s-2s

Note: Using dev

**Sample 1**: <a href="https://console.cloud.google.com/traces/list?project=klara-nonprod&amp;pageState=(%22traceFilter%22:(%22chips%22:%22%255B%257B_22k_22_3A_22RootSpan_22_2C_22t_22_3A10_2C_22v_22_3A_22_5C_22Recv.%252Fcore%252Flatest%252Ftoken_5C_22_22_2C_22i_22_3A_22root_22%257D%255D%22),%22traceIntervalPicker%22:(%22groupValue%22:%22PT12H%22,%22customValue%22:null))&amp;start=1694517864981&amp;end=1695122664981&amp;tid=9df7a94d12ffe2d49b3dc4a4b72973e2&amp;spanId=00f72a0ff2de24b1" class="external-link" rel="nofollow">9df7a94d12ffe2d49b3dc4a4b72973e2</a>

Call to keycloak: 88.2%


![[47494365263-image-20230920-090828.png]]



**WHY: Request to jwt-service → LB again: business requirement**

Call to webclient/ivy to get roles: 4.6%


![[47494365263-image-20230920-091022.png]]



Call to luz-tenant to get all tenants: 1.1%


![[47494365263-image-20230920-091138.png]]



<a href="https://console.cloud.google.com/logs/query;query=--%20resource.type:%2528http_load_balancer%2529%20AND%20jsonPayload.enforcedSecurityPolicy.name:%2528prod-security-policy%2529%0A--%20httpRequest.latency%20%3C%20500ms%0A--%20httpRequest.latency%20%3E%20100s%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20%2528httpRequest.requestUrl:%20%22auth%2Frealms%2Fklara%2Fprotocol%2Fopenid-connect%2Ftoken%22%20OR%20textPayload%20%3D~%20%22openid-connect%2Ftoken%22%2529%0A%2528httpRequest.requestUrl:%20%22https:%2F%2Fapi-dev.klara.tech%2Fcore%2Flatest%2Ftoken%22%20OR%20textPayload%20%3D~%20%22%2Fcore%2Flatest%2Ftoken%22%2529%0A--%20httpRequest.requestUrl:%2528%22api.klara.ch%22%20OR%20%22api.epost.ch%22%2529%20--%20public%20api%0A--%20resource.labels.namespace_name%3D%22dev%22%0A--%20labels.%22k8s-pod%2Fapp%22%3D%22login-nginx-ingress%22;pinnedLogId=2023-09-20T01:35:03.886867Z%2Fdvsy9mfb25990;lfeCustomFields=;cursorTimestamp=2023-09-20T01:35:10.808170742Z;startTime=2023-09-20T01:34:50.231Z;endTime=2023-09-20T01:40:50.231Z?project=klara-nonprod" class="external-link" rel="nofollow">Logs</a> for LB, KONG (proxy) and public-api-adapter

**Sample 2**: <a href="https://console.cloud.google.com/traces/list?project=klara-nonprod&amp;pageState=(%22traceFilter%22:(%22chips%22:%22%255B%257B_22k_22_3A_22RootSpan_22_2C_22t_22_3A10_2C_22v_22_3A_22_5C_22Recv.%252Fcore%252Flatest%252Ftoken_5C_22_22_2C_22i_22_3A_22root_22%257D%255D%22),%22traceIntervalPicker%22:(%22groupValue%22:%22PT12H%22,%22customValue%22:null))&amp;start=1694517864981&amp;end=1695122664981&amp;tid=b092cd3c337a6477e102b39bd6e04f06&amp;spanId=8cae393084bfddce" class="external-link" rel="nofollow">b092cd3c337a6477e102b39bd6e04f06</a>

Time consuming for calling to keycloak: 60%

Time consuming for get roles and tenants: same same


![[47494365263-image-20230920-091958.png]]



why calling ivy api to get role?

**=\> Conclusion: On DEV**

Time mostly used in keycloak.

### time-consuming before call hitting to keycloak: check release notes to see performance improve.

#### <u>On prod</u>

<div>

<table>
<colgroup>
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>LB: api…, proxy and luz-public-api-adapter</strong></p></th>
<th><p><strong>jwt</strong></p></th>
<th><p><strong>webclient</strong></p></th>
<th><p><strong>tenant-service</strong></p></th>
<th><p><strong>LB: login….</strong></p></th>
<th><p><strong>Conclusion</strong></p></th>
<th><p><strong>Problems</strong></p></th>
<th><p><strong>additional info</strong></p></th>
</tr>
&#10;<tr>
<td>

![[47494365263-image-20230922-100954.png]]


<p><a href="https://console.cloud.google.com/logs/query;query=--%20resource.type:%2528http_load_balancer%2529%20AND%20jsonPayload.enforcedSecurityPolicy.name:%2528prod-security-policy%2529%0A--%20httpRequest.latency%20%3C%20500ms%0A--%20httpRequest.latency%20%3E%20100s%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A%2528httpRequest.requestUrl:%20%22https:%2F%2Fapi.klara.ch%2Fcore%2Flatest%2Ftoken%22%20OR%20textPayload%20%3D~%20%22%2Fcore%2Flatest%2Ftoken%22%2529%0A--%20httpRequest.requestUrl:%2528%22api.klara.ch%22%20OR%20%22api.epost.ch%22%2529%20--%20public%20api%0A--%20resource.type%3D%22http_load_balancer%22;pinnedLogId=2023-09-22T07:25:20.933210Z%2Fivb75xf55twap;lfeCustomFields=;cursorTimestamp=2023-09-22T07:25:43.518262Z;startTime=2023-09-22T06:47:00.000Z;endTime=2023-09-22T07:44:00.000Z?project=klara-prod" class="external-link" rel="nofollow">Log</a></p></td>
<td>

![[47494365263-image-20230922-104443.png]]


<p><a href="https://console.cloud.google.com/logs/query;query=--%20resource.type:%2528http_load_balancer%2529%20AND%20jsonPayload.enforcedSecurityPolicy.name:%2528prod-security-policy%2529%0A--%20httpRequest.latency%20%3C%20500ms%0A--%20httpRequest.latency%20%3E%20100s%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A%2528textPayload%3D~%22%2Fpublic-api-tokens%22%2529%0A-textPayload%3D~%22com.axonivy.sec.rest.RESTRequestFilter%22%0A--%20httpRequest.requestUrl:%2528%22api.klara.ch%22%20OR%20%22api.epost.ch%22%2529%20--%20public%20api;pinnedLogId=2023-09-22T07:25:23.055446775Z%2F50mbb133e5a3ut77;lfeCustomFields=;cursorTimestamp=2023-09-22T07:27:05.892200774Z;aroundTime=2023-09-22T07:25:41.856Z;duration=PT5M?project=klara-prod" class="external-link" rel="nofollow">Log</a></p></td>
<td></td>
<td>

![[47494365263-image-20230922-104406.png]]


<p><a href="https://console.cloud.google.com/logs/query;query=--%20resource.type:%2528http_load_balancer%2529%20AND%20jsonPayload.enforcedSecurityPolicy.name:%2528prod-security-policy%2529%0A--%20httpRequest.latency%20%3C%20500ms%0A--%20httpRequest.latency%20%3E%20100s%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A%2528textPayload%3D~%22%2Fall-tenants%22%2529%0A-textPayload%3D~%22ch.axonivy.sec.filter.RestRequestFilter%22%0A--%20httpRequest.requestUrl:%2528%22api.klara.ch%22%20OR%20%22api.epost.ch%22%2529%20--%20public%20api;pinnedLogId=2023-09-22T07:25:23.054054951Z%2Fim8t3i64w8gx9tio;lfeCustomFields=;cursorTimestamp=2023-09-22T07:25:45.218392897Z;aroundTime=2023-09-22T07:25:41.856Z;duration=PT5M?project=klara-prod" class="external-link" rel="nofollow">Log</a></p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="d04b67a0-a2df-4e80-a345-e47982de6fac" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>(textPayload=~&quot;/all-tenants&quot; AND textPayload=~&quot;time-consuming=\d{4,}&quot;)
-textPayload=~&quot;ch.axonivy.sec.filter.RestRequestFilter&quot;
-- httpRequest.requestUrl:(&quot;api.klara.ch&quot; OR &quot;api.epost.ch&quot;) -- public api
labels.&quot;k8s-pod/app&quot;=&quot;luztenant-service&quot;
-- all: 10,938 
-- &gt;= 1s: 3,244 </code></pre>
</div>
</div>
<p><a href="https://console.cloud.google.com/logs/query;query=--%20resource.type:%2528http_load_balancer%2529%20AND%20jsonPayload.enforcedSecurityPolicy.name:%2528prod-security-policy%2529%0A--%20httpRequest.latency%20%3C%20500ms%0A--%20httpRequest.latency%20%3E%20100s%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A%2528textPayload%3D~%22%2Fall-tenants%22%20AND%20textPayload%3D~%22time-consuming%3D%5Cd%7B4,%7D%22%2529%0A-textPayload%3D~%22ch.axonivy.sec.filter.RestRequestFilter%22%0A--%20httpRequest.requestUrl:%2528%22api.klara.ch%22%20OR%20%22api.epost.ch%22%2529%20--%20public%20api%0Alabels.%22k8s-pod%2Fapp%22%3D%22luztenant-service%22%0A--%20all:%2010,938%20%0A--%20%3E%3D%201s:%20;pinnedLogId=2023-09-22T07:25:23.054054951Z%2Fim8t3i64w8gx9tio;lfeCustomFields=;cursorTimestamp=2023-09-21T19:12:57.543814273Z;startTime=2023-09-21T02:23:45.411Z;endTime=2023-09-22T02:23:45.411Z?project=klara-prod" class="external-link" rel="nofollow">Log</a></p></td>
<td>

![[47494365263-image-20230922-105324.png]]


<p><a href="https://console.cloud.google.com/logs/query;query=--%20resource.type:%2528http_load_balancer%2529%20AND%20jsonPayload.enforcedSecurityPolicy.name:%2528prod-security-policy%2529%0A--%20httpRequest.latency%20%3C%20500ms%0A--%20httpRequest.latency%20%3E%20100s%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A%2528httpRequest.requestUrl:%20%22https:%2F%2Flogin.klara.ch%2Fauth%2Frealms%2Fklara%2Fprotocol%2Fopenid-connect%2Ftoken%22%20OR%20textPayload%20%3D~%20%22auth%2Frealms%2Fklara%2Fprotocol%2Fopenid-connect%2Ftoken%22%2529%0A--%20httpRequest.requestUrl:%2528%22api.klara.ch%22%20OR%20%22api.epost.ch%22%2529%20--%20public%20api%0A-httpRequest.requestUrl:%20%22https:%2F%2Flogin.klara.ch%2Fauth%2Frealms%2Fklara%2Fprotocol%2Fopenid-connect%2Ftoken%2Fintrospect%22%0Aresource.type%3D%22http_load_balancer%22;pinnedLogId=2023-09-22T07:25:20.968125Z%2Fwgvj6cf1tifp9;lfeCustomFields=;cursorTimestamp=2023-09-22T07:25:41.466153Z;startTime=2023-09-22T06:47:50.548Z;endTime=2023-09-22T07:44:50.549Z?project=klara-prod" class="external-link" rel="nofollow">Log</a></p>
<p>very small amount of time</p></td>
<td><p>Time mostly used for getting tenants</p></td>
<td>

![[47494365263-image-20230922-111329.png]]

</td>
<td>

![[47494365263-image-20230925-020248.png]]


<ul>
<li><p>indexes</p>
<ul>
<li><p>tenant_id</p></li>
<li><p>username, tenant id</p></li>
</ul></li>
</ul></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

</div>

# Other additional information

## Statistic for generating access/tenant token via public-api in last 5 days

**Sample date**: Sep 19, 2023, ~5PM

dvdat@AAVN-LAPTOP107:~/workspace/analyze_log\$ ./statistic_analyze.sh  
Enter the input file name: log.csv  
Enter the list of ranges, separated by whitespace: 0-500 501-1000 1001-2000 2001-5000 5001-10000 10001-20000 20001-50000 50001-100000 100001-200000  
Number of entries within the range 0-500: 11  
Number of entries within the range 501-1000: 2368  
Number of entries within the range 1001-2000: 3964  
Number of entries within the range 2001-5000: 82  
Number of entries within the range 5001-10000: 18  
Number of entries within the range 10001-20000: 1  
Number of entries within the range 50001-100000: 1  
Number of entries within the range 100001-200000: 1  
dvdat@AAVN-LAPTOP107:~/workspace/analyze_log\$

%% ai-graph-start %%

**Related notes:**
- [[Tenant token issue]]
- [[Test Keycloak - Company Identity Mapper (script mapper)]]
- [[Understanding Keycloak Authorization Code flow]]
- [[Optimus ePost myLife app(luz_mylife_epost_adapter) - API Response Performance Analysis]]
- [[EPC API - Load Test]]

%% ai-graph-end %%