---
ai_hash: 3d278cfcb7365e00
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 89
depth: 2.96
entities: []
relevance: 0.798
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48519381275/EPC+API+-+Load+Test
space: FUT
status: reference
tags:
- confluence
- programming
- space/fut
title: EPC API - Load Test
topic: programming
type: source
updated: 2025-06-10
---

# EPC API - Load Test

> [!info] Imported from Confluence
> Space **FUT** · updated 2025-06-10 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48519381275/EPC+API+-+Load+Test)
> Relevance 0.798 · topic `programming`

# Why we have this test?

When we ran the test with the SmartSend workflow, we noticed that many requests were slow. (The test used 3 pods of EPC API)


![[48519381275-image-20250603-142643.png]]



So we conducted an analysis here: <a href="https://console.cloud.google.com/logs/analytics;queriedResources=%7B%22resources%22:%5B%22projects%2Fklara-performance%2Flocations%2Feurope-west6%2Fbuckets%2FDefault-ZH%2Fviews%2F_AllLogs%22%5D%7D;queryHandle=%7B%22A_a%22:%7B%22nativeJobId%22:%22Cgj2P-60qfSXyxIgam9iXzNpUXo4U0FuXzVKN3lKU1pRaV91UnluUmJjNUgaDGV1cm9wZS13ZXN0NkDWq4iRpRA%22,%22viewResourceNames%22:%5B%22projects%2Fklara-performance%2Flocations%2Feurope-west6%2Fbuckets%2FDefault-ZH%2Fviews%2F_AllLogs%22%5D,%22queryType%22:%22LOGS_ANALYTICS_QUERY_TYPE_SOURCE%22%7D,%22query%22:%22WITH%5Cn%20%20logs%20AS%20%2528%5Cn%20%20%20%20SELECT%5Cn%20%20%20%20%20%20JSON_VALUE%2528json_payload,%20&#39;$.message&#39;%2529%20AS%20message,%5Cn%20%20%20%20%20%20CAST%2528REGEXP_EXTRACT%2528JSON_VALUE%2528json_payload,%20&#39;$.message&#39;%2529,%20r&#39;completed%20in%20%2528%5C%5Cd%2B%2529ms&#39;%2529%20AS%20INT64%2529%20as%20duration_ms,%5Cn%20%20%20%20%20%20REGEXP_EXTRACT%2528JSON_VALUE%2528json_payload,%20&#39;$.message&#39;%2529,%20r&#39;%5C%5C%5BDB_DEBUG%5C%5C%5D%20%2528%5B%5E%20%5D%2BRepository%20%5B%5E%20%5D%2B%2528%3F:%5C%5Cs%2Bmethod%2529%3F%2529&#39;%2529%20AS%20operation%5Cn%20%20%20%20FROM%5Cn%20%20%20%20%20%20%60klara-performance.europe-west6.Default-ZH._AllLogs%60%5Cn%20%20%20%20WHERE%5Cn%20%20%20%20%20%20NORMALIZE_AND_CASEFOLD%2528resource.type,%20NFKC%2529%20%3D%20%5C%22k8s_container%5C%22%5Cn%20%20%20%20%20%20AND%20NORMALIZE_AND_CASEFOLD%2528SAFE.STRING%2528resource.labels%5B%5C%22project_id%5C%22%5D%2529,%20NFKC%2529%20%3D%20%5C%22klara-performance%5C%22%5Cn%20%20%20%20%20%20AND%20NORMALIZE_AND_CASEFOLD%2528SAFE.STRING%2528resource.labels%5B%5C%22location%5C%22%5D%2529,%20NFKC%2529%20%3D%20%5C%22europe-west6-a%5C%22%5Cn%20%20%20%20%20%20AND%20NORMALIZE_AND_CASEFOLD%2528SAFE.STRING%2528resource.labels%5B%5C%22cluster_name%5C%22%5D%2529,%20NFKC%2529%20%3D%20%5C%22klara-performance%5C%22%5Cn%20%20%20%20%20%20AND%20NORMALIZE_AND_CASEFOLD%2528SAFE.STRING%2528resource.labels%5B%5C%22namespace_name%5C%22%5D%2529,%20NFKC%2529%20%3D%20%5C%22performance%5C%22%5Cn%20%20%20%20%20%20AND%20NORMALIZE_AND_CASEFOLD%2528SAFE.STRING%2528labels%5B%5C%22k8s-pod%2Fapp%5C%22%5D%2529,%20NFKC%2529%20%3D%20%5C%22luz-epc-api-poc%5C%22%5Cn%20%20%20%20%20%20AND%20severity_number%20%3E%3D%200%5Cn%20%20%20%20%20%20AND%20JSON_VALUE%2528json_payload,%20&#39;$.message&#39;%2529%20IS%20NOT%20NULL%5Cn%20%20%20%20%20%20AND%20REGEXP_CONTAINS%2528JSON_VALUE%2528json_payload,%20&#39;$.message&#39;%2529,%20r&#39;%5C%5C%5BDB_DEBUG%5C%5C%5D&#39;%2529%5Cn%20%20%20%20%20%20AND%20REGEXP_CONTAINS%2528JSON_VALUE%2528json_payload,%20&#39;$.message&#39;%2529,%20r&#39;completed%20in%20%5C%5Cd%2Bms&#39;%2529%5Cn%20%20%2529,%5Cn%20%20stats_with_arrays%20AS%20%2528%5Cn%20%20%20%20SELECT%5Cn%20%20%20%20%20%20operation,%5Cn%20%20%20%20%20%20MIN%2528duration_ms%2529%20as%20min_duration_ms,%5Cn%20%20%20%20%20%20ROUND%2528AVG%2528duration_ms%2529,%202%2529%20as%20avg_duration_ms,%5Cn%20%20%20%20%20%20MAX%2528duration_ms%2529%20as%20max_duration_ms,%5Cn%20%20%20%20%20%20SUM%2528duration_ms%2529%20as%20total_duration_ms,%5Cn%20%20%20%20%20%20COUNT%2528*%2529%20as%20call_count,%5Cn%20%20%20%20%20%20ARRAY_AGG%2528duration_ms%20ORDER%20BY%20duration_ms%2529%20as%20sorted_durations,%5Cn%20%20%20%20%20%20ARRAY_AGG%2528message%20ORDER%20BY%20duration_ms%20DESC%20LIMIT%201%2529%5BOFFSET%25280%2529%5D%20as%20slowest_execution_log%5Cn%20%20%20%20FROM%5Cn%20%20%20%20%20%20logs%5Cn%20%20%20%20WHERE%5Cn%20%20%20%20%20%20operation%20IS%20NOT%20NULL%5Cn%20%20%20%20GROUP%20BY%5Cn%20%20%20%20%20%20operation%5Cn%20%20%2529%5CnSELECT%5Cn%20%20operation,%5Cn%20%20min_duration_ms,%5Cn%20%20avg_duration_ms,%5Cn%20%20CASE%5Cn%20%20%20%20WHEN%20MOD%2528ARRAY_LENGTH%2528sorted_durations%2529,%202%2529%20%3D%201%5Cn%20%20%20%20THEN%20sorted_durations%5BOFFSET%2528DIV%2528ARRAY_LENGTH%2528sorted_durations%2529%20-%201,%202%2529%2529%5D%5Cn%20%20%20%20ELSE%20%2528%5Cn%20%20%20%20%20%20sorted_durations%5BOFFSET%2528DIV%2528ARRAY_LENGTH%2528sorted_durations%2529,%202%2529%20-%201%2529%5D%20%2B%5Cn%20%20%20%20%20%20sorted_durations%5BOFFSET%2528DIV%2528ARRAY_LENGTH%2528sorted_durations%2529,%202%2529%2529%5D%5Cn%20%20%20%20%2529%20%2F%202.0%5Cn%20%20END%20as%20median_duration_ms,%5Cn%20%20max_duration_ms,%5Cn%20%20total_duration_ms,%5Cn%20%20call_count,%5Cn%20%20slowest_execution_log%5CnFROM%5Cn%20%20stats_with_arrays%5CnORDER%20BY%5Cn%20%20max_duration_ms%20DESC%22%7D;upperTab=query;lowerTab=query_results;vizLayoutType=TABLE;queryLanguage=SQL;startTime=2025-05-30T01:02:30.517Z;endTime=2025-05-30T07:02:30.518Z;useReservedSlots=false;fromExplorer=region?inv=1&amp;invt=Abyv8A&amp;project=klara-performance&amp;chartConfig=%7B%22xyChart%22:%7B%22constantLines%22:%5B%5D,%22dataSets%22:%5B%7B%22breakdowns%22:%5B%5D,%22dimensions%22:%5B%7B%22column%22:%22operation%22,%22columnType%22:%22STRING%22,%22maxBinCount%22:5,%22sortColumn%22:%22operation%22,%22sortOrder%22:%22SORT_ORDER_ASCENDING%22%7D%5D,%22measures%22:%5B%7B%22aggregationFunction%22:%7B%22parameters%22:%5B%5D,%22type%22:%22count%22%7D,%22column%22:%22%22%7D%5D,%22opsAnalyticsQuery%22:%7B%22queryExecutionRules%22:%7B%22useReservedSlots%22:false%7D,%22queryHandle%22:%22Cgj2P-60qfSXyxIgam9iXzNpUXo4U0FuXzVKN3lKU1pRaV91UnluUmJjNUgaDGV1cm9wZS13ZXN0NkDWq4iRpRA%22,%22sql%22:%22WITH%5Cn%20%20logs%20AS%20(%5Cn%20%20%20%20SELECT%5Cn%20%20%20%20%20%20JSON_VALUE(json_payload,%20%27$.message%27)%20AS%20message,%5Cn%20%20%20%20%20%20CAST(REGEXP_EXTRACT(JSON_VALUE(json_payload,%20%27$.message%27),%20r%27completed%20in%20(%5C%5Cd%2B)ms%27)%20AS%20INT64)%20as%20duration_ms,%5Cn%20%20%20%20%20%20REGEXP_EXTRACT(JSON_VALUE(json_payload,%20%27$.message%27),%20r%27%5C%5C%5BDB_DEBUG%5C%5C%5D%20(%5B%5E%20%5D%2BRepository%20%5B%5E%20%5D%2B(%3F:%5C%5Cs%2Bmethod)%3F)%27)%20AS%20operation%5Cn%20%20%20%20FROM%5Cn%20%20%20%20%20%20%60klara-performance.europe-west6.Default-ZH._AllLogs%60%5Cn%20%20%20%20WHERE%5Cn%20%20%20%20%20%20NORMALIZE_AND_CASEFOLD(resource.type,%20NFKC)%20%3D%20%5C%22k8s_container%5C%22%5Cn%20%20%20%20%20%20AND%20NORMALIZE_AND_CASEFOLD(SAFE.STRING(resource.labels%5B%5C%22project_id%5C%22%5D),%20NFKC)%20%3D%20%5C%22klara-performance%5C%22%5Cn%20%20%20%20%20%20AND%20NORMALIZE_AND_CASEFOLD(SAFE.STRING(resource.labels%5B%5C%22location%5C%22%5D),%20NFKC)%20%3D%20%5C%22europe-west6-a%5C%22%5Cn%20%20%20%20%20%20AND%20NORMALIZE_AND_CASEFOLD(SAFE.STRING(resource.labels%5B%5C%22cluster_name%5C%22%5D),%20NFKC)%20%3D%20%5C%22klara-performance%5C%22%5Cn%20%20%20%20%20%20AND%20NORMALIZE_AND_CASEFOLD(SAFE.STRING(resource.labels%5B%5C%22namespace_name%5C%22%5D),%20NFKC)%20%3D%20%5C%22performance%5C%22%5Cn%20%20%20%20%20%20AND%20NORMALIZE_AND_CASEFOLD(SAFE.STRING(labels%5B%5C%22k8s-pod%2Fapp%5C%22%5D),%20NFKC)%20%3D%20%5C%22luz-epc-api-poc%5C%22%5Cn%20%20%20%20%20%20AND%20severity_number%20%3E%3D%200%5Cn%20%20%20%20%20%20AND%20JSON_VALUE(json_payload,%20%27$.message%27)%20IS%20NOT%20NULL%5Cn%20%20%20%20%20%20AND%20REGEXP_CONTAINS(JSON_VALUE(json_payload,%20%27$.message%27),%20r%27%5C%5C%5BDB_DEBUG%5C%5C%5D%27)%5Cn%20%20%20%20%20%20AND%20REGEXP_CONTAINS(JSON_VALUE(json_payload,%20%27$.message%27),%20r%27completed%20in%20%5C%5Cd%2Bms%27)%5Cn%20%20),%5Cn%20%20stats_with_arrays%20AS%20(%5Cn%20%20%20%20SELECT%5Cn%20%20%20%20%20%20operation,%5Cn%20%20%20%20%20%20MIN(duration_ms)%20as%20min_duration_ms,%5Cn%20%20%20%20%20%20ROUND(AVG(duration_ms),%202)%20as%20avg_duration_ms,%5Cn%20%20%20%20%20%20MAX(duration_ms)%20as%20max_duration_ms,%5Cn%20%20%20%20%20%20SUM(duration_ms)%20as%20total_duration_ms,%5Cn%20%20%20%20%20%20COUNT(*)%20as%20call_count,%5Cn%20%20%20%20%20%20ARRAY_AGG(duration_ms%20ORDER%20BY%20duration_ms)%20as%20sorted_durations,%5Cn%20%20%20%20%20%20ARRAY_AGG(message%20ORDER%20BY%20duration_ms%20DESC%20LIMIT%201)%5BOFFSET(0)%5D%20as%20slowest_execution_log%5Cn%20%20%20%20FROM%5Cn%20%20%20%20%20%20logs%5Cn%20%20%20%20WHERE%5Cn%20%20%20%20%20%20operation%20IS%20NOT%20NULL%5Cn%20%20%20%20GROUP%20BY%5Cn%20%20%20%20%20%20operation%5Cn%20%20)%5CnSELECT%5Cn%20%20operation,%5Cn%20%20min_duration_ms,%5Cn%20%20avg_duration_ms,%5Cn%20%20CASE%5Cn%20%20%20%20WHEN%20MOD(ARRAY_LENGTH(sorted_durations),%202)%20%3D%201%5Cn%20%20%20%20THEN%20sorted_durations%5BOFFSET(DIV(ARRAY_LENGTH(sorted_durations)%20-%201,%202))%5D%5Cn%20%20%20%20ELSE%20(%5Cn%20%20%20%20%20%20sorted_durations%5BOFFSET(DIV(ARRAY_LENGTH(sorted_durations),%202)%20-%201)%5D%20%2B%5Cn%20%20%20%20%20%20sorted_durations%5BOFFSET(DIV(ARRAY_LENGTH(sorted_durations),%202))%5D%5Cn%20%20%20%20)%20%2F%202.0%5Cn%20%20END%20as%20median_duration_ms,%5Cn%20%20max_duration_ms,%5Cn%20%20total_duration_ms,%5Cn%20%20call_count,%5Cn%20%20slowest_execution_log%5CnFROM%5Cn%20%20stats_with_arrays%5CnORDER%20BY%5Cn%20%20max_duration_ms%20DESC%22%7D,%22plotType%22:%22STACKED_BAR%22,%22pointConnectionMethod%22:%22GAP_DETECTION%22,%22sortOrderParameters%22:%5B%5D,%22targetAxis%22:%22Y1%22%7D%5D,%22options%22:%7B%22mode%22:%22COLOR%22%7D,%22y1Axis%22:%7B%22label%22:%22%22,%22scale%22:%22LINEAR%22%7D%7D%7D" class="external-link" rel="nofollow"><strong><u>Link to Log Analyze</u></strong></a>

- We can see that most of the time is spent on read queries.


![[48519381275-image-20250603-141517.png]]



We suspected that the MongoDB connection had reached its limit, which caused the queries to slow down, so we started investigating the MongoDB configuration in our NestJS setup.

# Research threading in Nestjs

### EPC API

EPC API is using Mongoose to connect to MongoDB, and we are currently using the default configuration. By default, `maxPoolSize` is set to 100.

<a href="https://mongoosejs.com/docs/connections.html" class="external-link" data-card-appearance="inline" rel="nofollow">https://mongoosejs.com/docs/connections.html</a>


![[48519381275-image-20250530-070728.png]]



There are 3 instances of the EPC API running in production, so the maximum number of simultaneous database connections can reach up to 300.


![[48519381275-image-20250602-065235.png]]



### MongoDB

Base on <a href="https://www.mongodb.com/docs/atlas/reference/free-shared-limitations/#:~:text=Atlas%20throttles%20the%20network%20speed,data%20transfers%20on%20that%20connection." class="external-link" rel="nofollow">document of MongoDB</a>, maximum connection can handle by M0 Free clusters is 500 connections.


![[48519381275-image-20250602-050154.png]]



**Assumption**: MongoDB's connection limit of 500 is still higher than the 300 maximum connections from EPC API, so the issue may not be caused by hitting the MongoDB connection limit. Therefore, we performed additional tests to identify the root cause.

# Test Result

### Scenario:

- We created an API that receives 100 message IDs, queries MongoDB to find the corresponding data using `findByIds` in `MessageReadRepository`, and returns the number of records found.

- There are two modes: real and fake database connection.

<div id="expander-1587141478" class="expand-container conf-macro output-block" hasbody="true" macro-id="c9caf8e8-b34c-4a01-9163-cf214a7fe210" macro-name="expand">

<div id="expander-control-1587141478" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Real/Fake Database Connection</span>

</div>

<div id="expander-content-1587141478" class="expand-content expand-hidden">

We have a flag that, when set to false, returns the data without connecting to the real database.


![[48519381275-image-20250603-145044.png]]



</div>

</div>

### Configuration of the test:

<div id="expander-1232548018" class="expand-container conf-macro output-block" hasbody="true" macro-id="2f3de3fa-567f-43ab-be5f-4f2fa2ec3a48" macro-name="expand">

<div id="expander-control-1232548018" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">GKE config</span>

</div>

<div id="expander-content-1232548018" class="expand-content expand-hidden">

Here are the configuration that use in these test:

**luz-epc-api-poc:**

- CPU: 4

- Memory: 3 Gi


![[48519381275-image-20250602-065609.png]]



</div>

</div>

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
<th><p><strong>N.o pods</strong></p></th>
<th><p><strong>Concurrent users</strong></p></th>
<th><p><strong>DB connection?</strong></p></th>
<th><p><strong>RPS</strong></p></th>
<th><p><strong>Median Latency (ms)</strong></p></th>
<th><p><strong>90%ile Latency (ms)</strong></p></th>
<th><p><strong>Key observation</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p>100</p></td>
<td><p>Yes (Real)</p></td>
<td><p>41.1</p></td>
<td><p>2200</p></td>
<td><p>3100</p></td>
<td><p>Baseline with DB</p>
<div id="expander-370913285" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="e6eeaabf-2266-4394-ba3c-6923492cdc3b" data-macro-name="expand">
<div id="expander-control-370913285" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Details</span>
</div>
<div id="expander-content-370913285" class="expand-content expand-hidden">

![[48519381275-image-20250602-081744.png]]

![[48519381275-image-20250602-081754.png]]

![[48519381275-image-20250602-081806.png]]

![[48519381275-image-20250602-081842.png]]


</div>
</div></td>
</tr>
<tr>
<td><p>1</p></td>
<td><p>100</p></td>
<td><p>No (Mocked)</p></td>
<td><p>185.8</p></td>
<td><p>510</p></td>
<td><p>610</p></td>
<td><p><strong>~4.5x RPS increase &amp; latency drop without DB.</strong> Clear DB bottleneck.</p>
<div id="expander-1226166778" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="27406eea-b6c8-4be2-8730-71f4d066fa86" data-macro-name="expand">
<div id="expander-control-1226166778" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Details</span>
</div>
<div id="expander-content-1226166778" class="expand-content expand-hidden">

![[48519381275-image-20250602-080828.png]]

![[48519381275-image-20250602-080749.png]]

![[48519381275-image-20250602-080807.png]]

![[48519381275-image-20250602-080936.png]]


</div>
</div></td>
</tr>
<tr>
<td><p>1</p></td>
<td><p>300</p></td>
<td><p>Yes (Real)</p></td>
<td><p>39.7</p></td>
<td><p>4000</p></td>
<td><p>7600</p></td>
<td><p>Increased users overwhelm single pod with DB; RPS stagnates, latency worsens.</p>
<div id="expander-2031710387" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="7f78ddaa-7cd9-454c-8c4f-b77f919bdea0" data-macro-name="expand">
<div id="expander-control-2031710387" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Details</span>
</div>
<div id="expander-content-2031710387" class="expand-content expand-hidden">

![[48519381275-image-20250602-083126.png]]

![[48519381275-image-20250602-083955.png]]

![[48519381275-image-20250602-084054.png]]


</div>
</div></td>
</tr>
<tr>
<td><p>1</p></td>
<td><p>300</p></td>
<td><p>No (Mocked)</p></td>
<td><p>175.6</p></td>
<td><p>1500</p></td>
<td><p>1800</p></td>
<td><p><strong>~4.4x RPS increase &amp; latency drop without DB.</strong> DB bottleneck persists.</p>
<div id="expander-1494421714" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="7c2ab38e-7871-4153-b84e-c4749ddb1264" data-macro-name="expand">
<div id="expander-control-1494421714" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Details</span>
</div>
<div id="expander-content-1494421714" class="expand-content expand-hidden">

![[48519381275-image-20250602-083101.png]]

![[48519381275-image-20250602-082935.png]]

![[48519381275-image-20250602-083042.png]]


</div>
</div></td>
</tr>
<tr>
<td><p>3</p></td>
<td><p>300</p></td>
<td><p>Yes (Real)</p></td>
<td><p>148.8</p></td>
<td><p>1500</p></td>
<td><p>2600</p></td>
<td><p>Scaling pods helps (3.75x RPS vs 1 pod/300 users/DB). DB still a major factor.</p>
<div id="expander-1979141858" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="601f24d7-d55d-43ca-8b87-df4029f29180" data-macro-name="expand">
<div id="expander-control-1979141858" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Details</span>
</div>
<div id="expander-content-1979141858" class="expand-content expand-hidden">

![[48519381275-image-20250602-085703.png]]

![[48519381275-image-20250602-090905.png]]

![[48519381275-image-20250602-090844.png]]

![[48519381275-image-20250602-091032.png]]


</div>
</div></td>
</tr>
<tr>
<td><p>3</p></td>
<td><p>300</p></td>
<td><p>No (Mocked)</p></td>
<td><p>405.6</p></td>
<td><p>460</p></td>
<td><p>630</p></td>
<td><p><strong>~2.7x RPS increase &amp; latency drop without DB.</strong> App itself scales well.</p>
<div id="expander-692304282" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="c7408527-266d-4320-9ec0-44fbeeb84070" data-macro-name="expand">
<div id="expander-control-692304282" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Details</span>
</div>
<div id="expander-content-692304282" class="expand-content expand-hidden">

![[48519381275-image-20250602-084504.png]]

![[48519381275-image-20250602-085450.png]]

![[48519381275-image-20250602-085503.png]]

![[48519381275-image-20250602-085614.png]]


</div>
</div></td>
</tr>
</tbody>
</table>

</div>

### **Max Pool Size** Test Result

Scenarios:

- We called the `findByIds` API with **the same** list of 100 message IDs in each request. The IDs did **not change** between calls.

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
<th><p><strong>N.o pods</strong></p></th>
<th><p><strong>Concurrent users</strong></p></th>
<th><p><strong>DB connection?</strong></p></th>
<th><p><strong>maxPoolSize</strong></p>
<p><strong>per pod</strong></p></th>
<th><p><strong>RPS</strong></p></th>
<th><p><strong>Median Latency (ms)</strong></p></th>
<th><p><strong>90%ile Latency (ms)</strong></p></th>
<th><p><strong>Key observation</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p>50</p></td>
<td><p>Yes (Real)</p></td>
<td><p>50</p></td>
<td><p>176.8</p></td>
<td><p>270</p></td>
<td><p>380</p></td>
<td><p>Baseline with happy case.</p>
<div id="expander-270463836" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="55dab0b8-584b-4b48-9a30-b2203cb1c2ed" data-macro-name="expand">
<div id="expander-control-270463836" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Details</span>
</div>
<div id="expander-content-270463836" class="expand-content expand-hidden">

![[48519381275-image-20250603-071528.png]]

![[48519381275-image-20250603-072801.png]]

![[48519381275-image-20250603-072749.png]]

![[48519381275-image-20250603-073005.png]]


</div>
</div></td>
</tr>
<tr>
<td><p>1</p></td>
<td><p>50</p></td>
<td><p>Yes (Real)</p></td>
<td><p>100</p></td>
<td><p>222.9</p></td>
<td><p>220</p></td>
<td><p>270</p></td>
<td><p>After increasing the <code>maxPoolSize</code>, both the RPS and median response time improved.</p>
<div id="expander-1649490209" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="175d6723-22fc-4418-a1b3-c340b130f273" data-macro-name="expand">
<div id="expander-control-1649490209" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Details</span>
</div>
<div id="expander-content-1649490209" class="expand-content expand-hidden">

![[48519381275-image-20250603-082134.png]]



![[48519381275-image-20250603-080815.png]]

![[48519381275-image-20250603-080806.png]]

![[48519381275-image-20250603-080900.png]]


</div>
</div></td>
</tr>
<tr>
<td><p>1</p></td>
<td><p>50</p></td>
<td><p>Yes (Real)</p></td>
<td><p>25</p></td>
<td><p>215</p></td>
<td><p>220</p></td>
<td><p>260</p></td>
<td><p>After decreasing the <code>maxPoolSize</code>, there was no reduction in either RPS or median response time.</p>
<div id="expander-1090902601" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="9701bc89-9c51-49f1-bcb9-d40f646fb21b" data-macro-name="expand">
<div id="expander-control-1090902601" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Details</span>
</div>
<div id="expander-content-1090902601" class="expand-content expand-hidden">

![[48519381275-image-20250603-082125.png]]

![[48519381275-image-20250603-083058.png]]

![[48519381275-image-20250603-083118.png]]


</div>
</div></td>
</tr>
<tr>
<td><p>1</p></td>
<td><p>50</p></td>
<td><p>Yes (Real)</p></td>
<td><p>10</p></td>
<td><p>217</p></td>
<td><p>230</p></td>
<td><p>250</p></td>
<td><p>After decreasing more the <code>maxPoolSize</code>, there was no reduction in either RPS or median response time.</p>
<div id="expander-1958538172" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="8fd59d87-6300-4752-9cd9-7bd2b4adcf92" data-macro-name="expand">
<div id="expander-control-1958538172" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Details</span>
</div>
<div id="expander-content-1958538172" class="expand-content expand-hidden">

![[48519381275-image-20250603-090853.png]]

![[48519381275-image-20250603-090834.png]]

![[48519381275-image-20250603-090925.png]]

![[48519381275-image-20250603-091014.png]]


</div>
</div></td>
</tr>
<tr>
<td><p>1</p></td>
<td><p>50</p></td>
<td><p>Yes (Real)</p></td>
<td><p>1</p></td>
<td><p>218.3</p></td>
<td><p>220</p></td>
<td><p>260</p></td>
<td><p>After decreasing more the <code>maxPoolSize</code>, there was no reduction in either RPS or median response time.</p>
<div id="expander-277124771" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="33ef6908-1dc8-40bc-a371-736cd1f49a3f" data-macro-name="expand">
<div id="expander-control-277124771" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Details</span>
</div>
<div id="expander-content-277124771" class="expand-content expand-hidden">

![[48519381275-image-20250603-093948.png]]

![[48519381275-image-20250603-094434.png]]

![[48519381275-image-20250603-094526.png]]


</div>
</div></td>
</tr>
<tr>
<td><p>1</p></td>
<td><p>100</p></td>
<td><p>Yes (Real)</p></td>
<td><p>200</p></td>
<td><p>216</p></td>
<td><p>450</p></td>
<td><p>530</p></td>
<td><p>TODO: Retest</p>
<div id="expander-1999432101" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="ae46bf6d-df44-475e-b6c0-c092c7af4bbf" data-macro-name="expand">
<div id="expander-control-1999432101" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Details</span>
</div>
<div id="expander-content-1999432101" class="expand-content expand-hidden">

![[48519381275-image-20250603-085333.png]]



![[48519381275-image-20250603-085310.png]]

![[48519381275-image-20250603-085352.png]]

![[48519381275-image-20250603-090104.png]]


</div>
</div></td>
</tr>
</tbody>
</table>

</div>

### **Max Pool Size** Test Result

This is to double-check whether the `maxPoolSize` configuration is working as expected.

The conclusion is that `maxPoolSize` is working as expected.

Scenarios:

- We called the `findByIds` API with a list of 100 **random** message IDs in each request. The IDs **changed** between calls.

<div>

<table>
<colgroup>
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>N.o pods</strong></p></th>
<th><p><strong>Concurrent users</strong></p></th>
<th><p><strong>DB connection?</strong></p></th>
<th><p><strong>maxPoolSize</strong></p>
<p><strong>per pod</strong></p></th>
<th><p><strong>RPS</strong></p></th>
<th><p><strong>Median Latency (ms)</strong></p></th>
<th><p><strong>90%ile Latency (ms)</strong></p></th>
<th><p><strong>Number of Connection</strong></p>
<p><strong>in MongoDB</strong></p></th>
<th><p><strong>Key observation</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p>50</p></td>
<td><p>Yes (Real)</p></td>
<td><p>50</p></td>
<td><p>59.6</p></td>
<td><p>530</p></td>
<td><p>710</p></td>
<td><p>~82 → 112</p></td>
<td><p>Baseline with happy case.</p>
<div id="expander-1395171262" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="c87db814-5318-46cc-bb68-4d5aecc3e94a" data-macro-name="expand">
<div id="expander-control-1395171262" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Details</span>
</div>
<div id="expander-content-1395171262" class="expand-content expand-hidden">

![[48519381275-image-20250609-035812.png]]

![[48519381275-image-20250609-040149.png]]



![[48519381275-image-20250609-035914.png]]

![[48519381275-image-20250609-040024.png]]


<p>After scale down the EPC API, the connection reduced to 82.</p>

![[48519381275-image-20250609-040439.png]]

![[48519381275-image-20250609-040541.png]]


</div>
</div></td>
</tr>
<tr>
<td><p>1</p></td>
<td><p>50</p></td>
<td><p>Yes (Real)</p></td>
<td><p>10</p></td>
<td><p>58.7</p></td>
<td><p>570</p></td>
<td><p>680</p></td>
<td><p>85-&gt; 94</p></td>
<td><p>After decreasing more the <code>maxPoolSize</code>, there was no reduction in either RPS or median response time.</p>
<div id="expander-642295912" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="662384a4-9279-4112-ae18-be10c1e0aea6" data-macro-name="expand">
<div id="expander-control-642295912" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Details</span>
</div>
<div id="expander-content-642295912" class="expand-content expand-hidden">
<p>Before deploying EPC API:</p>

![[48519381275-image-20250609-041703.png]]


<p>After deploying but before running test:</p>

![[48519381275-image-20250609-042157.png]]


<p>After test:</p>

![[48519381275-image-20250609-043317.png]]

![[48519381275-image-20250609-043327.png]]

![[48519381275-image-20250609-043452.png]]

![[48519381275-image-20250609-043543.png]]


<p>After scale down EPC API:</p>

![[48519381275-image-20250609-043858.png]]

![[48519381275-image-20250609-043930.png]]


</div>
</div></td>
</tr>
<tr>
<td><p>1</p></td>
<td><p>50</p></td>
<td><p>Yes (Real)</p></td>
<td><p>1</p></td>
<td><p>65.4</p></td>
<td><p>450</p></td>
<td><p>510</p></td>
<td><p>No changed</p></td>
<td><p>After decreasing more the <code>maxPoolSize</code>, there was no reduction in either RPS or median response time.</p>
<div id="expander-635116260" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="dd9a3f61-ae92-4bb0-9b69-cd98c4b6bb7d" data-macro-name="expand">
<div id="expander-control-635116260" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Details</span>
</div>
<div id="expander-content-635116260" class="expand-content expand-hidden">

![[48519381275-image-20250609-034033.png]]

![[48519381275-image-20250609-034053.png]]

![[48519381275-image-20250609-034140.png]]

![[48519381275-image-20250609-034231.png]]


</div>
</div></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[One API end to end testing]]
- [[Regular Load Test Performance Test of OneAPI]]
- [[Public API client performance analysis]]
- [[AI-0000 Low Analyze job throughput]]
- [[Optimus ePost myLife app(luz_mylife_epost_adapter) - API Response Performance Analysis]]

%% ai-graph-end %%