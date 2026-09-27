---
ai_hash: 0d5da19fc296c66b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 28
depth: 2.4
entities: []
relevance: 0.701
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47042134101/luz_cache+performance+test+with+security
space: LUZ
status: reference
tags:
- confluence
- security
- space/luz
title: luz_cache performance test with security
topic: security
type: source
updated: 2022-01-25
---

# luz_cache performance test with security

> [!info] Imported from Confluence
> Space **LUZ** · updated 2022-01-25 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47042134101/luz_cache+performance+test+with+security)
> Relevance 0.701 · topic `security`

#### Concurrency level 2 / 500000 requests (GET request)

##### redis-cache


![[47042134101-perf_redis-cache_cpu_load.png]]




![[47042134101-perf_redis-cache_mem_usage.png]]

![[47042134101-perf_redis-cache_calls.png]]

![[47042134101-perf_redis-cache_client_connections.png]]



##### luz-cache


![[47042134101-luz-cache_–_Details_zum_Deployment_–_Kubernetes_Engine_–_klara-nonprod_–_Google_.png]]



<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e4bdaa27-0759-430f-81b1-56c10d650674" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
root@toolbox:/# ab -k -c 2 -n 500000 -H 'Content-Type: application/json' -H 'Authorization: Bearer eyJhbGciOiJSUzUxMiJ9.eyJzdWIiOiJhZG1pbiIsImNvbXBhbnktdGVuYW50Ijp7ImNvbXBhbnlJbmZvSWQiOjQyODMsImNvbXBhbnlJZCI6MSwiY29tcGFueU5hbWUiOiJPS0sgU01BUlRGT1JNIiwic3RhdHVzIjpudWxsLCJzdGF0dXNFeHBpcnlEYXRlIjpudWxsLCJpZCI6NzQ5Miwicm9sZXMiOlsiY29tcGFueV9hZG1pbmlzdHJhdG9yIiwiZW1wbG95ZWUiLCJleHRlcm5hbF91c2VyIl0sInRlbmFudElkIjoiNzViMDBjNDMtMzQ5MC00MWRlLWExMjMtOGZhOThjM2I1NDlmIiwidXNlcm5hbWUiOiIiLCJuYW1lIjpudWxsLCJ0eXBlIjoiQ09NUEFOWSIsImNyZWF0ZURhdGUiOm51bGwsInVwZGF0ZURhdGUiOm51bGwsImV4cGlyZURhdGUiOm51bGwsImV4cGlyZVRpbWUiOm51bGx9LCJpc3MiOiJjb20uYXhvbml2eSIsInRlbmFudElkIjoiNzViMDBjNDMtMzQ5MC00MWRlLWExMjMtOGZhOThjM2I1NDlmIiwidXNlcl9yb2xlcyI6WyJTeXN0ZW1BZG1pbmlzdHJhdG9yIiwiQ1JPTl9KT0IiLCJFdmVyeWJvZHkiLCJhbGxfdGVuYW50c19hY2Nlc3MiLCJBZG1pbmlzdHJhdG9yIiwiTmV3c19BZG1pbiJdLCJwZXJzb24tdGVuYW50Ijp7ImlkIjowLCJyb2xlcyI6W10sInRlbmFudElkIjoiYTFmN2U3YmEtMDZlYy00NzRkLWIyZmQtODlhYWYyMzVjYTM3IiwidXNlcm5hbWUiOiJhZG1pbiIsIm5hbWUiOm51bGwsInR5cGUiOiJQRVJTT04iLCJjcmVhdGVEYXRlIjoiV2VkIE5vdiAzMCAxMTowNzo1OSBDRVQgMjAxNiIsInVwZGF0ZURhdGUiOm51bGwsImV4cGlyZURhdGUiOm51bGwsImV4cGlyZVRpbWUiOm51bGx9LCJleHAiOjE2NDMwNjEyMjQsImlhdCI6MTY0MzAxODAyNH0.PMu7kjO-Q19hwsy6GhUgGtIqSAf49rpwSbPFe7uAdqAHfD8kmO7QFOBxPflnXy8gCDYy8AFh9cLTatzs_J0BqcPo6B57penSiSZPqBylVO_ziqoiDhNT9CslR8bQ2fRQLdOnyHl7axFT1roBgii9WotE5HfYTCoVn6vLrSh_jwfHwD_k7G70NTMaRs_z-LN3BPTxFxuXAqYrl690-uSw66-qy2bCm9OKfVia7wrUq2WrsPlTyyLGV4CEjo_Qpsqn7gKiNImxAuZANrLJawjZk0c0Bwunfe-VqqD7Kg2G1p-XJFBphJbe5rOEGLRZSyoKcXvdcDeIHUcdC1RzdQV-Bw' 'http://luz-cache:8080/75b00c43-3490-41de-a123-8fa98c3b549f/caches/dev_luz-eletter_75b00c43-3490-41de-a123-8fa98c3b549f_a'
This is ApacheBench, Version 2.3 <$Revision: 1807734 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking luz-cache (be patient)
Completed 50000 requests
Completed 100000 requests
Completed 150000 requests
Completed 200000 requests
Completed 250000 requests
Completed 300000 requests
Completed 350000 requests
Completed 400000 requests
Completed 450000 requests
Completed 500000 requests
Finished 500000 requests


Server Software:
Server Hostname:        luz-cache
Server Port:            8080

Document Path:          /75b00c43-3490-41de-a123-8fa98c3b549f/caches/dev_luz-eletter_75b00c43-3490-41de-a123-8fa98c3b549f_a
Document Length:        94 bytes

Concurrency Level:      2
Time taken for tests:   461.454 seconds
Complete requests:      500000
Failed requests:        0
Keep-Alive requests:    500000
Total transferred:      94500000 bytes
HTML transferred:       47000000 bytes
Requests per second:    1083.53 [#/sec] (mean)
Time per request:       1.846 [ms] (mean)
Time per request:       0.923 [ms] (mean, across all concurrent requests)
Transfer rate:          199.99 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.0      0       0
Processing:     1    2   0.6      2      36
Waiting:        0    2   0.6      2      36
Total:          1    2   0.6      2      36

Percentage of the requests served within a certain time (ms)
  50%      2
  66%      2
  75%      2
  80%      2
  90%      2
  95%      2
  98%      3
  99%      4
 100%     36 (longest request)
```

</div>

</div>

#### Increasing concurrency until the longest request hits \> 50ms

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
<th><p><strong>Concurrent/Total</strong></p></th>
<th><p><strong>Total time (s)</strong></p></th>
<th><p><strong>Time per request (ms)</strong></p></th>
<th><p><strong>Longest (ms)</strong></p></th>
<th><p><strong>Status (pass/total)</strong></p></th>
</tr>
&#10;<tr>
<td colspan="5"><h3 id="luz_cacheperformancetestwithsecurity-Testrun(GCPtoolbox)">Test run (GCP toolbox)</h3>
<h4 id="luz_cacheperformancetestwithsecurity-Prerequisites">Prerequisites</h4>
<ul>
<li></li>
</ul>
<h4 id="luz_cacheperformancetestwithsecurity-luz-cache.1">luz-cache</h4>

![[47042134101-perf_luz-cache_testrun_overview.png]]


<h4 id="luz_cacheperformancetestwithsecurity-Command">Command</h4>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="9aee4422-6885-4bfd-abd9-a72fa7434bc3" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code># setup up value
curl --location --request POST &#39;http://luz-cache:8080/75b00c43-3490-41de-a123-8fa98c3b549f/caches&#39; \
--header &#39;Authorization: Bearer eyJhbGciOiJSUzUxMiJ9.eyJzdWIiOiJhZG1pbiIsImNvbXBhbnktdGVuYW50Ijp7ImNvbXBhbnlJbmZvSWQiOjQyODMsImNvbXBhbnlJZCI6MSwiY29tcGFueU5hbWUiOiJPS0sgU01BUlRGT1JNIiwic3RhdHVzIjpudWxsLCJzdGF0dXNFeHBpcnlEYXRlIjpudWxsLCJpZCI6NzQ5Miwicm9sZXMiOlsiY29tcGFueV9hZG1pbmlzdHJhdG9yIiwiZW1wbG95ZWUiLCJleHRlcm5hbF91c2VyIl0sInRlbmFudElkIjoiNzViMDBjNDMtMzQ5MC00MWRlLWExMjMtOGZhOThjM2I1NDlmIiwidXNlcm5hbWUiOiIiLCJuYW1lIjpudWxsLCJ0eXBlIjoiQ09NUEFOWSIsImNyZWF0ZURhdGUiOm51bGwsInVwZGF0ZURhdGUiOm51bGwsImV4cGlyZURhdGUiOm51bGwsImV4cGlyZVRpbWUiOm51bGx9LCJpc3MiOiJjb20uYXhvbml2eSIsInRlbmFudElkIjoiNzViMDBjNDMtMzQ5MC00MWRlLWExMjMtOGZhOThjM2I1NDlmIiwidXNlcl9yb2xlcyI6WyJTeXN0ZW1BZG1pbmlzdHJhdG9yIiwiQ1JPTl9KT0IiLCJFdmVyeWJvZHkiLCJhbGxfdGVuYW50c19hY2Nlc3MiLCJBZG1pbmlzdHJhdG9yIiwiTmV3c19BZG1pbiJdLCJwZXJzb24tdGVuYW50Ijp7ImlkIjowLCJyb2xlcyI6W10sInRlbmFudElkIjoiYTFmN2U3YmEtMDZlYy00NzRkLWIyZmQtODlhYWYyMzVjYTM3IiwidXNlcm5hbWUiOiJhZG1pbiIsIm5hbWUiOm51bGwsInR5cGUiOiJQRVJTT04iLCJjcmVhdGVEYXRlIjoiV2VkIE5vdiAzMCAxMTowNzo1OSBDRVQgMjAxNiIsInVwZGF0ZURhdGUiOm51bGwsImV4cGlyZURhdGUiOm51bGwsImV4cGlyZVRpbWUiOm51bGx9LCJleHAiOjE2NDMwNjEyMjQsImlhdCI6MTY0MzAxODAyNH0.PMu7kjO-Q19hwsy6GhUgGtIqSAf49rpwSbPFe7uAdqAHfD8kmO7QFOBxPflnXy8gCDYy8AFh9cLTatzs_J0BqcPo6B57penSiSZPqBylVO_ziqoiDhNT9CslR8bQ2fRQLdOnyHl7axFT1roBgii9WotE5HfYTCoVn6vLrSh_jwfHwD_k7G70NTMaRs_z-LN3BPTxFxuXAqYrl690-uSw66-qy2bCm9OKfVia7wrUq2WrsPlTyyLGV4CEjo_Qpsqn7gKiNImxAuZANrLJawjZk0c0Bwunfe-VqqD7Kg2G1p-XJFBphJbe5rOEGLRZSyoKcXvdcDeIHUcdC1RzdQV-Bw&#39; \
--header &#39;Content-Type: application/json&#39; \
--data-raw &#39;{
    &quot;key&quot;: &quot;dev_luz-eletter_75b00c43-3490-41de-a123-8fa98c3b549f_a&quot;,
    &quot;value&quot;: &quot;this is the value 1&quot;
}&#39;
&#10; 
# run test sample
ab -k -c 1 -n 5000 -H &#39;Content-Type: application/json&#39; -H \
&#39;Authorization: Bearer eyJhbGciOiJSUzUxMiJ9.eyJzdWIiOiJhZG1pbiIsImNvbXBhbnktdGVuYW50Ijp7ImNvbXBhbnlJbmZvSWQiOjQyODMsImNvbXBhbnlJZCI6MSwiY29tcGFueU5hbWUiOiJPS0sgU01BUlRGT1JNIiwic3RhdHVzIjpudWxsLCJzdGF0dXNFeHBpcnlEYXRlIjpudWxsLCJpZCI6NzQ5Miwicm9sZXMiOlsiY29tcGFueV9hZG1pbmlzdHJhdG9yIiwiZW1wbG95ZWUiLCJleHRlcm5hbF91c2VyIl0sInRlbmFudElkIjoiNzViMDBjNDMtMzQ5MC00MWRlLWExMjMtOGZhOThjM2I1NDlmIiwidXNlcm5hbWUiOiIiLCJuYW1lIjpudWxsLCJ0eXBlIjoiQ09NUEFOWSIsImNyZWF0ZURhdGUiOm51bGwsInVwZGF0ZURhdGUiOm51bGwsImV4cGlyZURhdGUiOm51bGwsImV4cGlyZVRpbWUiOm51bGx9LCJpc3MiOiJjb20uYXhvbml2eSIsInRlbmFudElkIjoiNzViMDBjNDMtMzQ5MC00MWRlLWExMjMtOGZhOThjM2I1NDlmIiwidXNlcl9yb2xlcyI6WyJTeXN0ZW1BZG1pbmlzdHJhdG9yIiwiQ1JPTl9KT0IiLCJFdmVyeWJvZHkiLCJhbGxfdGVuYW50c19hY2Nlc3MiLCJBZG1pbmlzdHJhdG9yIiwiTmV3c19BZG1pbiJdLCJwZXJzb24tdGVuYW50Ijp7ImlkIjowLCJyb2xlcyI6W10sInRlbmFudElkIjoiYTFmN2U3YmEtMDZlYy00NzRkLWIyZmQtODlhYWYyMzVjYTM3IiwidXNlcm5hbWUiOiJhZG1pbiIsIm5hbWUiOm51bGwsInR5cGUiOiJQRVJTT04iLCJjcmVhdGVEYXRlIjoiV2VkIE5vdiAzMCAxMTowNzo1OSBDRVQgMjAxNiIsInVwZGF0ZURhdGUiOm51bGwsImV4cGlyZURhdGUiOm51bGwsImV4cGlyZVRpbWUiOm51bGx9LCJleHAiOjE2NDMwNjEyMjQsImlhdCI6MTY0MzAxODAyNH0.PMu7kjO-Q19hwsy6GhUgGtIqSAf49rpwSbPFe7uAdqAHfD8kmO7QFOBxPflnXy8gCDYy8AFh9cLTatzs_J0BqcPo6B57penSiSZPqBylVO_ziqoiDhNT9CslR8bQ2fRQLdOnyHl7axFT1roBgii9WotE5HfYTCoVn6vLrSh_jwfHwD_k7G70NTMaRs_z-LN3BPTxFxuXAqYrl690-uSw66-qy2bCm9OKfVia7wrUq2WrsPlTyyLGV4CEjo_Qpsqn7gKiNImxAuZANrLJawjZk0c0Bwunfe-VqqD7Kg2G1p-XJFBphJbe5rOEGLRZSyoKcXvdcDeIHUcdC1RzdQV-Bw&#39; &#39;http://luz-cache:8080/75b00c43-3490-41de-a123-8fa98c3b549f/caches/dev_luz-eletter_75b00c43-3490-41de-a123-8fa98c3b549f_a&#39;</code></pre>
</div>
</div>
<p>Test output logs:</p>
<p><span class="confluence-embedded-file-wrapper conf-macro output-inline" data-hasbody="false" data-macro-id="4aabb55f-137e-4532-ae94-b809b459b818" data-macro-name="view-file"><a href="../_attachments/47042134101-perf_luz-cache.txt" class="confluence-embedded-file" data-nice-type="Text File" data-file-src="/wiki/download/attachments/47042134101/perf_luz-cache.txt?version=1&amp;modificationDate=1643030285040&amp;cacheVersion=1&amp;api=v2" data-mime-type="text/plain" data-has-thumbnail="true">

![[47042134101-perf_luz-cache.txt]]

</a></span></p></td>
</tr>
<tr>
<td><p>1/5000</p></td>
<td><p>9.052</p></td>
<td><p>1.810</p></td>
<td><p>50% 2<br />
66% 2<br />
75% 2<br />
80% 2<br />
90% 2<br />
95% 2<br />
98% 3<br />
99% 3<br />
100% 17 (longest request)</p></td>
<td><p>5000/5000</p></td>
</tr>
<tr>
<td><p>2/5000</p></td>
<td><p>4.603</p></td>
<td><p>1.841</p></td>
<td><p>50% 2<br />
66% 2<br />
75% 2<br />
80% 2<br />
90% 2<br />
95% 2<br />
98% 3<br />
99% 4<br />
100% 16 (longest request)</p></td>
<td><p>5000/5000</p></td>
</tr>
<tr>
<td><p>3/5000</p></td>
<td><p>4.516</p></td>
<td><p>2.710</p></td>
<td><p>50% 2<br />
66% 2<br />
75% 2<br />
80% 2<br />
90% 2<br />
95% 3<br />
98% 6<br />
<span>99% 29</span><br />
<span>100% 222 (longest request)</span></p></td>
<td><p>5000/5000</p></td>
</tr>
<tr>
<td><p>4/5000</p></td>
<td><p>4.508</p></td>
<td><p>3.606</p></td>
<td><p>50% 2<br />
66% 2<br />
75% 2<br />
80% 2<br />
90% 3<br />
95% 4<br />
<span>98% 31</span><br />
<span>99% 45</span><br />
<span>100% 237 (longest request)</span></p></td>
<td><p>5000/5000</p></td>
</tr>
<tr>
<td><p>5/5000</p></td>
<td><p>4.347</p></td>
<td><p>4.347</p></td>
<td><p>50% 2<br />
66% 3<br />
75% 3<br />
80% 3<br />
90% 4<br />
95% 6<br />
<span>98% 36</span><br />
<span>99% 46</span><br />
<span>100% 249 (longest request)</span></p></td>
<td><p>5000/5000</p></td>
</tr>
<tr>
<td><p>6/5000</p></td>
<td><p>4.463</p></td>
<td><p>5.355</p></td>
<td><p>50% 3<br />
66% 3<br />
75% 3<br />
80% 4<br />
90% 5<br />
95% 9<br />
<span>98% 46</span><br />
<span>99% 54</span><br />
<span>100% 251 (longest request)</span></p></td>
<td><p>5000/5000</p></td>
</tr>
<tr>
<td><p>7/5000</p></td>
<td><p>4.236</p></td>
<td><p>5.930</p></td>
<td><p>50% 3<br />
66% 3<br />
75% 3<br />
80% 3<br />
90% 5<br />
95% 17<br />
<span>98% 51</span><br />
<span>99% 56</span><br />
<span>100% 375 (longest request)</span></p></td>
<td><p>5000/5000</p></td>
</tr>
<tr>
<td><p>8/5000</p></td>
<td><p>4.332</p></td>
<td><p>6.931</p></td>
<td><p>50% 3<br />
66% 3<br />
75% 3<br />
80% 3<br />
90% 5<br />
<span>95% 46</span><br />
<span>98% 56</span><br />
<span>99% 76</span><br />
<span>100% 259 (longest request)</span></p></td>
<td><p>5000/5000</p></td>
</tr>
<tr>
<td><p>9/5000</p></td>
<td><p>4.282</p></td>
<td><p>7.708</p></td>
<td><p>50% 3<br />
66% 3<br />
75% 4<br />
80% 4<br />
90% 7<br />
<span>95% 48</span><br />
<span>98% 56</span><br />
<span>99% 73</span><br />
<span>100% 351 (longest request)</span></p></td>
<td><p>5000/5000</p></td>
</tr>
<tr>
<td><p>10/5000</p></td>
<td><p>4.306</p></td>
<td><p>8.612</p></td>
<td><p>50% 3<br />
66% 4<br />
75% 4<br />
80% 5<br />
90% 9<br />
<span>95% 52</span><br />
<span>98% 59</span><br />
<span>99% 101</span><br />
<span>100% 363 (longest request)</span></p></td>
<td><p>5000/5000</p></td>
</tr>
</tbody>
</table>

</div>

#### Concurrency level 2 / 500000 requests (POST request)

##### redis-cache


![[47042134101-perf_redis-cache_post_cpu_load.png]]

![[47042134101-perf_redis-cache_post_mem_usage.png]]

![[47042134101-perf_redis-cache_post_calls.png]]

![[47042134101-perf_redis-cache_post_client_connections.png]]



<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a39af818-d0b7-4938-aeb3-e264c4eac2f4" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
root@toolbox:/tmp# ab -k -c 2 -n 500000 -p payload_value.txt -T 'application/json' -H 'Authorization: Bearer eyJhbGciOiJSUzUxMiJ9.eyJzdWIiOiJhZG1pbiIsImNvbXBhbnktdGVuYW50Ijp7ImNvbXBhbnlJbmZvSWQiOjQyODMsImNvbXBhbnlJZCI6MSwiY29tcGFueU5hbWUiOiJPS0sgU01BUlRGT1JNIiwic3RhdHVzIjpudWxsLCJzdGF0dXNFeHBpcnlEYXRlIjpudWxsLCJpZCI6NzQ5Miwicm9sZXMiOlsiY29tcGFueV9hZG1pbmlzdHJhdG9yIiwiZW1wbG95ZWUiLCJleHRlcm5hbF91c2VyIl0sInRlbmFudElkIjoiNzViMDBjNDMtMzQ5MC00MWRlLWExMjMtOGZhOThjM2I1NDlmIiwidXNlcm5hbWUiOiIiLCJuYW1lIjpudWxsLCJ0eXBlIjoiQ09NUEFOWSIsImNyZWF0ZURhdGUiOm51bGwsInVwZGF0ZURhdGUiOm51bGwsImV4cGlyZURhdGUiOm51bGwsImV4cGlyZVRpbWUiOm51bGx9LCJpc3MiOiJjb20uYXhvbml2eSIsInRlbmFudElkIjoiNzViMDBjNDMtMzQ5MC00MWRlLWExMjMtOGZhOThjM2I1NDlmIiwidXNlcl9yb2xlcyI6WyJTeXN0ZW1BZG1pbmlzdHJhdG9yIiwiQ1JPTl9KT0IiLCJFdmVyeWJvZHkiLCJhbGxfdGVuYW50c19hY2Nlc3MiLCJBZG1pbmlzdHJhdG9yIiwiTmV3c19BZG1pbiJdLCJwZXJzb24tdGVuYW50Ijp7ImlkIjowLCJyb2xlcyI6W10sInRlbmFudElkIjoiYTFmN2U3YmEtMDZlYy00NzRkLWIyZmQtODlhYWYyMzVjYTM3IiwidXNlcm5hbWUiOiJhZG1pbiIsIm5hbWUiOm51bGwsInR5cGUiOiJQRVJTT04iLCJjcmVhdGVEYXRlIjoiV2VkIE5vdiAzMCAxMTowNzo1OSBDRVQgMjAxNiIsInVwZGF0ZURhdGUiOm51bGwsImV4cGlyZURhdGUiOm51bGwsImV4cGlyZVRpbWUiOm51bGx9LCJleHAiOjE2NDMwNjEyMjQsImlhdCI6MTY0MzAxODAyNH0.PMu7kjO-Q19hwsy6GhUgGtIqSAf49rpwSbPFe7uAdqAHfD8kmO7QFOBxPflnXy8gCDYy8AFh9cLTatzs_J0BqcPo6B57penSiSZPqBylVO_ziqoiDhNT9CslR8bQ2fRQLdOnyHl7axFT1roBgii9WotE5HfYTCoVn6vLrSh_jwfHwD_k7G70NTMaRs_z-LN3BPTxFxuXAqYrl690-uSw66-qy2bCm9OKfVia7wrUq2WrsPlTyyLGV4CEjo_Qpsqn7gKiNImxAuZANrLJawjZk0c0Bwunfe-VqqD7Kg2G1p-XJFBphJbe5rOEGLRZSyoKcXvdcDeIHUcdC1RzdQV-Bw' 'http://luz-cache:8080/75b00c43-3490-41de-a123-8fa98c3b549f/caches/'
This is ApacheBench, Version 2.3 <$Revision: 1807734 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking luz-cache (be patient)
Completed 50000 requests
Completed 100000 requests
Completed 150000 requests
Completed 200000 requests
Completed 250000 requests
Completed 300000 requests
Completed 350000 requests
Completed 400000 requests
Completed 450000 requests
Completed 500000 requests
Finished 500000 requests


Server Software:
Server Hostname:        luz-cache
Server Port:            8080

Document Path:          /75b00c43-3490-41de-a123-8fa98c3b549f/caches/
Document Length:        0 bytes

Concurrency Level:      2
Time taken for tests:   403.169 seconds
Complete requests:      500000
Failed requests:        0
Keep-Alive requests:    500000
Total transferred:      25500000 bytes
Total body sent:        1614500000
HTML transferred:       0 bytes
Requests per second:    1240.17 [#/sec] (mean)
Time per request:       1.613 [ms] (mean)
Time per request:       0.806 [ms] (mean, across all concurrent requests)
Transfer rate:          61.77 [Kbytes/sec] received
                        3910.67 kb/s sent
                        3972.43 kb/s total

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.0      0       0
Processing:     1    2   0.9      1      53
Waiting:        0    2   0.9      1      53
Total:          1    2   0.9      1      53
WARNING: The median and mean for the processing time are not within a normal deviation
        These results are probably not that reliable.
WARNING: The median and mean for the waiting time are not within a normal deviation
        These results are probably not that reliable.
WARNING: The median and mean for the total time are not within a normal deviation
        These results are probably not that reliable.

Percentage of the requests served within a certain time (ms)
  50%      1
  66%      2
  75%      2
  80%      2
  90%      2
  95%      2
  98%      4
  99%      7
 100%     53 (longest request)
```

</div>

</div>

### Conclusion

Comparing to GET calls (2 concurrent / 500000 calls) without security the latency time (mean) has been increased a bit (1.647 to 1.846 ms). The CPU (0.2%) load and memory (~117MB) usage remain low. The time per request (mean) is around 1083.53 calls / sec (without sec 1214.28).

The performance of the POST calls (2 concurrent / 500000 calls) looks similar to the GET calls or even a bit better. The number of client connections raises to 10 clients comparing to GET calls with 2 clients. And the CPU load raises to 9% comparing to GET calls with 6.3%.

%% ai-graph-start %%

**Related notes:**
- [[Redis NoSQL distributed cache - Performance test]]
- [[Benchmark of luz-database (performance env)]]
- [[Luz Audit System - Performance Optimization Proposal]]
- [[Create Document API – Performance Testing Report]]
- [[luz-vault - How to run Vault Benchmark]]

%% ai-graph-end %%