---
title: "Redis NoSQL distributed cache -  Performance test"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47007137884/Redis+NoSQL+distributed+cache+-+Performance+test
space: "LUZ"
topic: infra
relevance: 0.842
depth: 3
updated: 2021-12-10
attachments: 3
tags:
  - confluence
  - infra
  - space/luz
---

# Redis NoSQL distributed cache -  Performance test

> [!info] Imported from Confluence
> Space **LUZ** · updated 2021-12-10 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47007137884/Redis+NoSQL+distributed+cache+-+Performance+test)
> Relevance 0.842 · topic `infra`

This page shows the performance test results run by Apache Benchmark Tool (AB). The client which has been use to connect to the Redis server is Jakarta NoSQL.

## Implementation details

- luz_redis host has to be additionally set to redis configuration otherwiese localhost will be taken as the default host configuration

- Use `BucketManagerFactory` instead of `BucketManager` to work with JedisPool. `BucketManager` won’t use any JedisPool

## Open issue to solve

- Set the request timeout to 120 seconds on redis distibuted cache server (docu: <a href="https://redis.io/topics/clients#client-timeouts" class="external-link" data-card-appearance="inline" rel="nofollow">https://redis.io/topics/clients#client-timeouts</a> ). The default configuration timeout is 0 (disabled, no timeout set)

## Test runs

Perform GET requests to redis server (`http://localhost:8086/luz_cache_example/api/caches/1`).

- Apache Benchmark Tool output description: <a href="https://httpd.apache.org/docs/2.4/programs/ab.html" class="external-link" data-card-appearance="inline" rel="nofollow">https://httpd.apache.org/docs/2.4/programs/ab.html</a>

- JedisPool config: <a href="https://programmer.help/blogs/jedis-connection-pool-configuration.html" class="external-link" data-card-appearance="inline" rel="nofollow">https://programmer.help/blogs/jedis-connection-pool-configuration.html</a> / <a href="https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f" class="external-link" data-card-appearance="inline" rel="nofollow">https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f</a>


![[47007137884-Instance_details_–_Memorystore_–_klara-nonprod_–_Google_Cloud_Platform.png]]



<div>

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th></th>
<th><p><strong>Tests</strong></p></th>
<th><p><strong>Test execution</strong></p></th>
<th><p><strong>Test result</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p>1000 Req. / 100 Threads</p></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="80bc3240-72d1-4e59-993b-75204e496cff" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: bash; gutter: false; theme: Confluence" data-theme="Confluence"><code>~ ab -n 1000 -c 100 http://localhost:8086/luz_cache_example/api/caches/1
This is ApacheBench, Version 2.3 &lt;$Revision: 1879490 $&gt;
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/
&#10;Benchmarking localhost (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Completed 600 requests
Completed 700 requests
Completed 800 requests
Completed 900 requests
Completed 1000 requests
Finished 1000 requests
&#10;
Server Software:
Server Hostname:        localhost
Server Port:            8086
&#10;Document Path:          /luz_cache_example/api/caches/1
Document Length:        1 bytes
&#10;Concurrency Level:      100
Time taken for tests:   6.000 seconds
Complete requests:      1000
Failed requests:        0
Total transferred:      127000 bytes
HTML transferred:       1000 bytes
Requests per second:    166.67 [#/sec] (mean)
Time per request:       599.995 [ms] (mean)
Time per request:       6.000 [ms] (mean, across all concurrent requests)
Transfer rate:          20.67 [Kbytes/sec] received
&#10;Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    1   1.5      0       8
Processing:   521  576  54.6    553     773
Waiting:       18   68  51.6     48     270
Total:        521  577  55.2    553     776
&#10;Percentage of the requests served within a certain time (ms)
  50%    553
  66%    578
  75%    608
  80%    637
  90%    655
  95%    672
  98%    742
  99%    758
 100%    776 (longest request)</code></pre>
</div>
</div></td>
<td><p>All requests successfully completed and all clients connections has been disposed.</p></td>
</tr>
<tr>
<td>2</td>
<td><p>5000 Req. / 100 Threads</p></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="983c1afd-7487-4ff0-bc37-e95c924448f4" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: bash; gutter: false; theme: Confluence" data-theme="Confluence"><code> ~ ab -n 5000 -c 100 http://localhost:8086/luz_cache_example/api/caches/1
This is ApacheBench, Version 2.3 &lt;$Revision: 1879490 $&gt;
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/
&#10;Benchmarking localhost (be patient)
Completed 500 requests
Completed 1000 requests
Completed 1500 requests
Completed 2000 requests
Completed 2500 requests
Completed 3000 requests
Completed 3500 requests
Completed 4000 requests
Completed 4500 requests
Completed 5000 requests
Finished 5000 requests
&#10;
Server Software:
Server Hostname:        localhost
Server Port:            8086
&#10;Document Path:          /luz_cache_example/api/caches/1
Document Length:        1 bytes
&#10;Concurrency Level:      100
Time taken for tests:   30.251 seconds
Complete requests:      5000
Failed requests:        0
Total transferred:      635000 bytes
HTML transferred:       5000 bytes
Requests per second:    165.28 [#/sec] (mean)
Time per request:       605.019 [ms] (mean)
Time per request:       6.050 [ms] (mean, across all concurrent requests)
Transfer rate:          20.50 [Kbytes/sec] received
&#10;Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.8      0       9
Processing:   521  595 121.4    557    1475
Waiting:       19   91 119.9     52     971
Total:        521  595 121.5    557    1475
&#10;Percentage of the requests served within a certain time (ms)
  50%    557
  66%    581
  75%    600
  80%    616
  90%    685
  95%    774
  98%   1030
  99%   1272
 100%   1475 (longest request)</code></pre>
</div>
</div></td>
<td></td>
</tr>
<tr>
<td>3</td>
<td><p>10000 Req. / 100 Threads</p></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="08fc924e-701c-4f9d-8dce-6fe7b305f725" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: bash; gutter: false; theme: Confluence" data-theme="Confluence"><code>~ ab -n 10000 -c 100 http://localhost:8086/luz_cache_example/api/caches/1
This is ApacheBench, Version 2.3 &lt;$Revision: 1879490 $&gt;
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/
&#10;Benchmarking localhost (be patient)
Completed 1000 requests
Completed 2000 requests
Completed 3000 requests
Completed 4000 requests
Completed 5000 requests
Completed 6000 requests
Completed 7000 requests
Completed 8000 requests
Completed 9000 requests
Completed 10000 requests
Finished 10000 requests
&#10;
Server Software:
Server Hostname:        localhost
Server Port:            8086
&#10;Document Path:          /luz_cache_example/api/caches/1
Document Length:        1 bytes
&#10;Concurrency Level:      100
Time taken for tests:   54.733 seconds
Complete requests:      10000
Failed requests:        0
Total transferred:      1270000 bytes
HTML transferred:       10000 bytes
Requests per second:    182.70 [#/sec] (mean)
Time per request:       547.331 [ms] (mean)
Time per request:       5.473 [ms] (mean, across all concurrent requests)
Transfer rate:          22.66 [Kbytes/sec] received
&#10;Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.7      0      34
Processing:   519  544  27.1    535     810
Waiting:       18   41  26.3     33     302
Total:        519  544  27.3    535     810
&#10;Percentage of the requests served within a certain time (ms)
  50%    535
  66%    542
  75%    548
  80%    552
  90%    571
  95%    591
  98%    621
  99%    662
 100%    810 (longest request)</code></pre>
</div>
</div></td>
<td><p>All requests successfully completed and all clients connections has been disposed.</p></td>
</tr>
<tr>
<td>4</td>
<td><p>100000 Req. / 100 Threads (MAX_TOTAL=200)</p></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="ba6eee74-f5b4-4651-b883-fba1c80ee621" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: bash; gutter: false; theme: Confluence" data-theme="Confluence"><code> ~ ab -n 100000 -c 100 http://localhost:8086/luz_cache_example/api/caches/1
This is ApacheBench, Version 2.3 &lt;$Revision: 1879490 $&gt;
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/
&#10;Benchmarking localhost (be patient)
Completed 10000 requests
Completed 20000 requests
Completed 30000 requests
Completed 40000 requests
Completed 50000 requests
Completed 60000 requests
Completed 70000 requests
Completed 80000 requests
Completed 90000 requests
Completed 100000 requests
Finished 100000 requests
&#10;
Server Software:
Server Hostname:        localhost
Server Port:            8086
&#10;Document Path:          /luz_cache_example/api/caches/1
Document Length:        1 bytes
&#10;Concurrency Level:      100
Time taken for tests:   548.362 seconds
Complete requests:      100000
Failed requests:        0
Total transferred:      12700000 bytes
HTML transferred:       100000 bytes
Requests per second:    182.36 [#/sec] (mean)
Time per request:       548.362 [ms] (mean)
Time per request:       5.484 [ms] (mean, across all concurrent requests)
Transfer rate:          22.62 [Kbytes/sec] received
&#10;Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0  34.8      0   11009
Processing:   510  548  40.5    536    1515
Waiting:       10   45  39.0     34     965
Total:        519  548  53.4    537   11538
&#10;Percentage of the requests served within a certain time (ms)
  50%    537
  66%    543
  75%    550
  80%    556
  90%    578
  95%    607
  98%    650
  99%    686
 100%  11538 (longest request)</code></pre>
</div>
</div></td>
<td><p>All requests successfully completed and all clients connections has been disposed.</p></td>
</tr>
<tr>
<td>5</td>
<td><p>100000 Req. / 100 Threads (MAX_TOTAL=1000)</p></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="a0dfa3c0-fb47-4035-9770-b6e6eb28ab6c" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: bash; gutter: false; theme: Confluence" data-theme="Confluence"><code>~ ab -n 100000 -c 100 http://localhost:8086/luz_cache_example/api/caches/1
This is ApacheBench, Version 2.3 &lt;$Revision: 1879490 $&gt;
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/
&#10;Benchmarking localhost (be patient)
Completed 10000 requests
Completed 20000 requests
Completed 30000 requests
Completed 40000 requests
Completed 50000 requests
Completed 60000 requests
Completed 70000 requests
Completed 80000 requests
Completed 90000 requests
Completed 100000 requests
Finished 100000 requests
&#10;
Server Software:
Server Hostname:        localhost
Server Port:            8086
&#10;Document Path:          /luz_cache_example/api/caches/1
Document Length:        18 bytes
&#10;Concurrency Level:      100
Time taken for tests:   550.091 seconds
Complete requests:      100000
Failed requests:        0
Total transferred:      14500000 bytes
HTML transferred:       1800000 bytes
Requests per second:    181.79 [#/sec] (mean)
Time per request:       550.091 [ms] (mean)
Time per request:       5.501 [ms] (mean, across all concurrent requests)
Transfer rate:          25.74 [Kbytes/sec] received
&#10;Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.7      0      49
Processing:   511  549  40.7    538    1312
Waiting:       10   46  39.4     36     802
Total:        519  550  40.8    538    1312
&#10;Percentage of the requests served within a certain time (ms)
  50%    538
  66%    545
  75%    552
  80%    558
  90%    581
  95%    610
  98%    642
  99%    688
 100%   1312 (longest request)</code></pre>
</div>
</div></td>
<td><p>All requests successfully completed and all clients connections has been disposed.</p>
<p>Since the client call has been limited to 100 threads it won’t take any effects to set MAX_TOTAL from 200 to 1000. But the overall connection time is lower:</p>
<p>MAX_TOTAL 200:</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="8a6f0ed7-5f5e-465a-8705-cd213ab539bb" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: bash; gutter: false; theme: Confluence" data-theme="Confluence"><code>Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0  34.8      0   11009
Processing:   510  548  40.5    536    1515
Waiting:       10   45  39.0     34     965
Total:        519  548  53.4    537   11538</code></pre>
</div>
</div>
<p>MAX_TOTAL 1000:</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="8923e8a8-9c2a-475b-97ba-fc091471a518" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: bash; gutter: false; theme: Confluence" data-theme="Confluence"><code>Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.7      0      49
Processing:   511  549  40.7    538    1312
Waiting:       10   46  39.4     36     802
Total:        519  550  40.8    538    1312</code></pre>
</div>
</div></td>
</tr>
<tr>
<td>6</td>
<td><p>100000 Req. / 200 Threads (MAX_TOTAL=1000)</p></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="819a5e4a-edaa-49a8-a9bd-31a2f024b750" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>~ ab -n 100000 -c 200 http://localhost:8086/luz_cache_example/api/caches/1
This is ApacheBench, Version 2.3 &lt;$Revision: 1879490 $&gt;
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/
&#10;Benchmarking localhost (be patient)
Completed 10000 requests
Completed 20000 requests
Completed 30000 requests
Completed 40000 requests
Completed 50000 requests
Completed 60000 requests
Completed 70000 requests
Completed 80000 requests
Completed 90000 requests
Completed 100000 requests
Finished 100000 requests
&#10;
Server Software:
Server Hostname:        localhost
Server Port:            8086
&#10;Document Path:          /luz_cache_example/api/caches/1
Document Length:        0 bytes
&#10;Concurrency Level:      200
Time taken for tests:   305.412 seconds
Complete requests:      100000
Failed requests:        13277
   (Connect: 0, Receive: 0, Length: 13277, Exceptions: 0)
Total transferred:      7236451 bytes
HTML transferred:       13277 bytes
Requests per second:    327.43 [#/sec] (mean)
Time per request:       610.823 [ms] (mean)
Time per request:       3.054 [ms] (mean, across all concurrent requests)
Transfer rate:          23.14 [Kbytes/sec] received
&#10;Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    1   2.3      0     219
Processing:   517  610  76.4    591    1143
Waiting:       14  107  80.7     82     601
Total:        520  610  76.5    592    1145
&#10;Percentage of the requests served within a certain time (ms)
  50%    592
  66%    621
  75%    641
  80%    654
  90%    696
  95%    750
  98%    845
  99%    920
 100%   1145 (longest request)</code></pre>
</div>
</div></td>
<td><p>All requests successfully completed and all clients connections has been disposed.</p></td>
</tr>
</tbody>
</table>

</div>

## Links

- <a href="https://github.com/googleapis/java-redis" class="external-link" data-card-appearance="inline" rel="nofollow">https://github.com/googleapis/java-redis</a>

- Documentation Memorystore for redis: <a href="https://cloud.google.com/memorystore/docs/redis" class="external-link" data-card-appearance="inline" rel="nofollow">https://cloud.google.com/memorystore/docs/redis</a>

- Client library for Redis API <a href="https://cloud.google.com/memorystore/docs/redis/libraries" class="external-link" data-card-appearance="inline" rel="nofollow">https://cloud.google.com/memorystore/docs/redis/libraries</a>

- Redis API <a href="https://cloud.google.com/memorystore/docs/redis/reference/rest" class="external-link" data-card-appearance="inline" rel="nofollow">https://cloud.google.com/memorystore/docs/redis/reference/rest</a>

- Client libraries: <a href="https://cloud.google.com/appengine/docs/standard/java/using-memorystore" class="external-link" data-card-appearance="inline" rel="nofollow">https://cloud.google.com/appengine/docs/standard/java/using-memorystore</a>

- Client code snippet: <a href="https://cloud.google.com/appengine/docs/standard/java/using-memorystore#setup_redis_db" class="external-link" data-card-appearance="inline" rel="nofollow">https://cloud.google.com/appengine/docs/standard/java/using-memorystore#setup_redis_db</a>


![[47007137884-image-20211208-093134.png]]



Keyword: Reactive


![[47007137884-image-20211210-161940.png]]
