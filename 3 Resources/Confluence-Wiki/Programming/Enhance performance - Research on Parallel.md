---
ai_hash: 650819438a555090
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 19
depth: 2.41
entities: []
relevance: 0.703
source: https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47189918209/Enhance+performance+-+Research+on+Parallel
space: TP2020
status: reference
tags:
- confluence
- programming
- space/tp2020
title: Enhance performance - Research on Parallel
topic: programming
type: source
updated: 2022-10-06
---

# Enhance performance - Research on Parallel

> [!info] Imported from Confluence
> Space **TP2020** · updated 2022-10-06 · [open original](https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47189918209/Enhance+performance+-+Research+on+Parallel)
> Relevance 0.703 · topic `programming`

# Current situation


![[47189918209-image-20220929-042859.png]]



Method `createLetterStorageModification` will be called for each letter

getDocumentById: Average 87ms each

patchDocumentById: Average 640ms each

=\> total: 727ms \* letters

There could be hundreds of letters. For example: 727ms \* 100letters = 73s

# The problem

The next letter has to wait for the previous letter to be finished before its turn. And the waiting time is long.

# Potential solutions

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
<th><p><strong>No.</strong></p></th>
<th><p><strong>Description</strong></p></th>
<th><p><strong>Improvement</strong></p></th>
<th><p><strong>Difficulty Level</strong></p></th>
<th><p><strong>Advantage</strong></p></th>
<th><p><strong>Disadvantage</strong></p></th>
<th><p><strong>Note</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p>Just 1 request for all letters</p></td>
<td><p>Unknown</p></td>
<td><p>Hard</p></td>
<td><p>No letter have to wait</p></td>
<td><p>Depends on Kepler’s API</p></td>
<td></td>
</tr>
<tr>
<td><p>2</p></td>
<td><ul>
<li><p>1 request to get all letters</p></li>
<li><p>keep the number of request to patch</p></li>
</ul></td>
<td><p>10%</p>
<p>(100 letters took ~65s)</p></td>
<td><p>Easy</p></td>
<td><ul>
<li><p>initial request to get all letters</p></li>
</ul></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>3</p></td>
<td><p>Call patch requests in parallel (since the letters are independent)</p>
<ul>
<li><p>1 request to get all letters</p></li>
<li><p>Async 2 threads</p>
<ul>
<li><p>Patch requests</p></li>
</ul></li>
</ul></td>
<td><p>55%</p>
<p>(100 letters took ~33s)</p></td>
<td><p>Normal</p></td>
<td><ul>
<li><p>initial request to get all letters</p></li>
<li><p>call patching APIs in parallel</p></li>
</ul></td>
<td><ul>
<li><p>Depends on Kepler’s API</p></li>
<li><p>Need more threads to call patching API in async</p></li>
</ul></td>
<td></td>
</tr>
<tr>
<td><p><span>4</span></p></td>
<td><ul>
<li><p><span>1 request to get all letters</span></p></li>
<li><p><span>Async</span></p>
<ul>
<li><p><span>send individual patch</span></p></li>
</ul></li>
</ul></td>
<td><p><span>65%</span></p>
<p><span>(100 letters took ~25s)</span></p></td>
<td><p><span>Normal</span></p></td>
<td><p><span>Simplest solution can achiveve without external support</span></p></td>
<td><p><span>Need to care about thread pool size</span></p>
<p><span>=&gt; TODO: monitor and measurement</span></p></td>
<td><p>Inputs from other teams:</p>
<ul>
<li><p>Arrow: <a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20530450090/Identity+matching+performance#Identitymatchingperformance-Thethreadpool">Identity matching performance</a></p></li>
<li><p>Future:</p>
<ul>
<li><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/blog/2021/09/25/46961821466/How+to+set+an+ideal+thread+pool+size">How to set an ideal thread pool size</a> *</p></li>
<li><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/blog/2021/09/25/46962115395/Concurrency+and+parallelism+in+Java">Concurrency and parallelism in Java</a></p></li>
</ul></li>
</ul>
<p>What to measure:</p>
<ul>
<li><p>How many concurrent requests can be sent by the sender (thread pool size)</p></li>
<li><p>how many concurrent requests the server side is capable of handling (worker thread, scalability, ...)</p></li>
<li><p>API's time-consuming</p></li>
</ul>
<p>Where to measure:</p>
<ul>
<li><p>luz-docs-view-controller:<br />
<a href="https://console.cloud.google.com/kubernetes/service/asia-southeast1-a/klara-dev-vn/dev-vn/luz-docs-view-controller/overview?project=klara-nonprod" class="external-link" rel="nofollow">https://console.cloud.google.com/kubernetes/service/asia-southeast1-a/klara-dev-vn/dev-vn/luz-docs-view-controller/overview?project=klara-nonprod</a></p></li>
<li><p>luz-docs<br />
<a href="https://console.cloud.google.com/kubernetes/service/asia-southeast1-a/klara-dev-vn/dev-vn/luz-docs/overview?project=klara-nonprod" class="external-link" rel="nofollow">https://console.cloud.google.com/kubernetes/service/asia-southeast1-a/klara-dev-vn/dev-vn/luz-docs/overview?project=klara-nonprod</a></p></li>
<li><p>luz-jsonstore<br />
<a href="https://console.cloud.google.com/kubernetes/service/asia-southeast1-a/klara-dev-vn/dev-vn/luz-jsonstore/overview?project=klara-nonprod" class="external-link" rel="nofollow">https://console.cloud.google.com/kubernetes/service/asia-southeast1-a/klara-dev-vn/dev-vn/luz-jsonstore/overview?project=klara-nonprod</a></p></li>
</ul></td>
</tr>
</tbody>
</table>

</div>

# Measurement

## Tool

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="9753c165-bb60-4461-b2ca-ce86e2608bd4" macro-name="view-file"><a href="../_attachments/47189918209-apache_benchmark_2022_10_06.zip" class="confluence-embedded-file" data-nice-type="Zip Archive" data-file-src="/wiki/download/attachments/47189918209/apache_benchmark_2022_10_06.zip?version=1&amp;modificationDate=1665055860804&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/zip" data-has-thumbnail="true">

![[47189918209-apache_benchmark_2022_10_06.zip]]

</a></span>

To use the benchmark:

- download binary Apache Benchmark <a href="https://www.apachelounge.com/download/#google_vignette" class="external-link" data-card-appearance="inline" rel="nofollow">https://www.apachelounge.com/download/#google_vignette</a>

- download the script above, edit the script content and the post body, then execute it

## BEFORE

### 50 letters, 100 requests


![[47189918209-image-20221005-123045.png]]



<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Note</strong></p></th>
<th></th>
</tr>
&#10;<tr>
<td><p>Module: luz-docs</p></td>
<td>

![[47189918209-image-20221005-123829.png]]

</td>
</tr>
<tr>
<td><p>Module: luz-jsonstore</p>
<p>CPU: high</p></td>
<td>

![[47189918209-image-20221005-123907.png]]

</td>
</tr>
</tbody>
</table>

</div>

## AFTER

### 50 letters, 100 requests


![[47189918209-image-20221005-101035.png]]



<div>

|  |  |
|----|----|
|  |  |
| Module: luz-docs | 

![[47189918209-image-20221005-101125.png]]

 |
| Module: luz-jsonstore | 

![[47189918209-image-20221005-101538.png]]

 |

</div>

## Calculation

<a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/blog/2021/09/25/46961821466/How+to+set+an+ideal+thread+pool+size#Just-give-me-the-formula!" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/blog/2021/09/25/46961821466/How+to+set+an+ideal+thread+pool+size#Just-give-me-the-formula!</a>

`Number of threads = Number of Available Cores * (1 + Wait time / Service time)`  
Case store 50 letters in the root folder

- Number of letters: 50.

- Destination: Root.

- Threads in `luz_docs_view_controller`: 1.

- Time-consuming in `luz_docs_view_controller`: 54s.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="35a692e3-9290-4212-b1a1-25353fe8d16b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Number of threads = Number of Available Cores * (1 + Wait time / Service time)
total = 54s
eachLetter = getletter + patchletter = 0.664 + 0.076 = 0.824
waitingTime = eachLetter * letterCount = 0.824 * 50 = 41.2
serviceTime = total - waitingTime = 12.8
=> Number of threads = 2 * (1 + 41.2 / 12.8) = ~8 (threads)
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Performance pain points]]
- [[Concurrency Design Patterns]]
- [[Research on bulk removal of access class]]
- [[eArchive performance — luz-epost-business-web calls the count API on every search]]
- [[Prompt Performance Code Review]]

%% ai-graph-end %%