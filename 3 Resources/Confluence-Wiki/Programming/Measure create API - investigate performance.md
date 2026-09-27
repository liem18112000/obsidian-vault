---
ai_hash: 263d11e4bb9e910c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 26
depth: 2.65
entities: []
relevance: 0.738
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47119893374/Measure+create+API+-+investigate+performance
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: Measure create API - investigate performance
topic: programming
type: source
updated: 2022-06-01
---

# Measure create API - investigate performance

> [!info] Imported from Confluence
> Space **LUZ** · updated 2022-06-01 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47119893374/Measure+create+API+-+investigate+performance)
> Relevance 0.738 · topic `programming`

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="20c96826-458a-4ca3-87fc-b71869049c22" macro-name="view-file"><a href="../_attachments/47119893374-invoice.pdf" class="confluence-embedded-file" data-nice-type="PDF Document" data-file-src="/wiki/download/attachments/47119893374/invoice.pdf?version=1&amp;modificationDate=1653972377668&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/pdf" data-has-thumbnail="true">

![[47119893374-invoice.pdf]]

</a></span><span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="badc0c6b-0a9c-492f-a5de-beb31489d459" macro-name="view-file"><a href="../_attachments/47119893374-metadata_N.json" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47119893374/metadata_N.json?version=3&amp;modificationDate=1654082517750&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/json" data-has-thumbnail="true">

![[47119893374-metadata_N.json]]

</a></span>

Test create api with above reference file and metadata on Dev

<div>

<table style="width:100%;">
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
<th></th>
<th><p><strong>Process Audit Data</strong></p></th>
<th><p><strong>Scanning Antivirus</strong></p></th>
<th><p><strong>Add technical fields</strong></p></th>
<th><p><strong>Create metadata in JsonStore</strong></p></th>
<th><p><strong>Call Vault to get DEK and EDEK</strong></p></th>
<th><p><strong>Encrypt and Upload files to GCS (EDEK, Reference)</strong></p></th>
<th><p><strong>Trigger create audit log (Async)</strong></p></th>
<th><p><strong>Total</strong></p></th>
<th><p><strong>Image</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p>1ms</p></td>
<td><p><span>78ms</span></p></td>
<td><p>4ms</p></td>
<td><p><span>52ms</span></p></td>
<td><p>29ms</p></td>
<td><p><span>468ms</span></p>
<p><span>(Encryption took 106ms)</span></p></td>
<td><p>10ms</p></td>
<td><p>646ms</p></td>
<td>

![[47119893374-image-20220531-104135.png]]

</td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>0ms</p></td>
<td><p><span>88ms</span></p></td>
<td><p>4ms</p></td>
<td><p><span>58ms</span></p></td>
<td><p>29ms</p></td>
<td><p><span>333ms</span></p>
<p><span>(Encryption took 147ms)</span></p></td>
<td><p>12ms</p></td>
<td><p>529ms</p></td>
<td>

![[47119893374-image-20220531-104942.png]]

</td>
</tr>
<tr>
<td><p>3</p></td>
<td><p>0ms</p></td>
<td><p><span>80ms</span></p></td>
<td><p>5ms</p></td>
<td><p><span>70ms</span></p></td>
<td><p>29ms</p></td>
<td><p><span>339ms</span></p>
<p><span>(Encryption took 91ms)</span></p></td>
<td><p>10ms</p></td>
<td><p>537ms</p></td>
<td>

![[47119893374-image-20220531-110535.png]]

</td>
</tr>
<tr>
<td><p>4</p></td>
<td><p>1ms</p></td>
<td><p><span>87ms</span></p></td>
<td><p>4ms</p></td>
<td><p><span>48ms</span></p></td>
<td><p>25ms</p></td>
<td><p><span>406ms</span></p>
<p><span>(Encryption took 133ms)</span></p></td>
<td><p>13ms</p></td>
<td><p>588ms</p></td>
<td>

![[47119893374-image-20220531-110721.png]]

</td>
</tr>
<tr>
<td><p>5</p></td>
<td><p>1ms</p></td>
<td><p><span>98ms</span></p></td>
<td><p>5ms</p></td>
<td><p><span>51ms</span></p></td>
<td><p>39ms</p></td>
<td><p><span>274ms</span></p>
<p><span>(Encryption took 114ms)</span></p></td>
<td><p>22ms</p></td>
<td><p>493ms</p></td>
<td>

![[47119893374-image-20220531-110908.png]]

</td>
</tr>
<tr>
<td><p>6</p></td>
<td><p>0ms</p></td>
<td><p><span>94ms</span></p></td>
<td><p>4ms</p></td>
<td><p><span>58ms</span></p></td>
<td><p>24ms</p></td>
<td><p><span>415ms</span></p>
<p><span>(Encryption took 127ms)</span></p></td>
<td><p>12ms</p></td>
<td><p>611ms</p></td>
<td>

![[47119893374-image-20220531-111116.png]]

</td>
</tr>
<tr>
<td><p>7</p></td>
<td><p>0ms</p></td>
<td><p><span>76ms</span></p></td>
<td><p>4ms</p></td>
<td><p><span>58ms</span></p></td>
<td><p>26ms</p></td>
<td><p><span>260ns</span></p>
<p><span>(Encryption took 91ms)</span></p></td>
<td><p>11ms</p></td>
<td><p>439ms</p></td>
<td>

![[47119893374-image-20220531-111328.png]]

</td>
</tr>
<tr>
<td><p>8</p></td>
<td><p>1ms</p></td>
<td><p><span>96ms</span></p></td>
<td><p>5ms</p></td>
<td><p><span>65ms</span></p></td>
<td><p>27ms</p></td>
<td><p><span>294ms</span></p>
<p><span>(Encryption took 104ms)</span></p></td>
<td><p>13ms</p></td>
<td><p>504ms</p></td>
<td>

![[47119893374-image-20220531-111506.png]]

</td>
</tr>
<tr>
<td><p>9</p></td>
<td><p>0ms</p></td>
<td><p><span>82ms</span></p></td>
<td><p>5ms</p></td>
<td><p><span>79ms</span></p></td>
<td><p>29ms</p></td>
<td><p><span>403ms</span></p>
<p><span>(Encryption took 114ms)</span></p></td>
<td><p>13ms</p></td>
<td><p>615ms</p></td>
<td>

![[47119893374-image-20220531-111704.png]]

</td>
</tr>
<tr>
<td><p>10</p></td>
<td><p>0ms</p></td>
<td><p><span>83ms</span></p></td>
<td><p>5ms</p></td>
<td><p><span>58ms</span></p></td>
<td><p>28ms</p></td>
<td><p><span>330ms</span></p>
<p><span>(Encryption took 103ms)</span></p></td>
<td><p>11ms</p></td>
<td><p>521ms</p></td>
<td>

![[47119893374-image-20220531-111833.png]]

</td>
</tr>
</tbody>
</table>

</div>

Test create API with the file 10MB

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="829f96a0-06a4-473e-b774-feaec2f7c28a" macro-name="view-file"><a href="../_attachments/47119893374-metadata_N.json" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47119893374/metadata_N.json?version=3&amp;modificationDate=1654082517750&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/json" data-has-thumbnail="true">

![[47119893374-metadata_N.json]]

</a></span><span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="98e0d612-44e6-4db4-a7e6-a1736d14f648" macro-name="view-file"><a href="../_attachments/47119893374-Test10MB.pdf" class="confluence-embedded-file" data-nice-type="PDF Document" data-file-src="/wiki/download/attachments/47119893374/Test10MB.pdf?version=1&amp;modificationDate=1654080092930&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/pdf" data-has-thumbnail="true">

![[47119893374-Test10MB.pdf]]

</a></span>

<div>

<table style="width:100%;">
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
<th></th>
<th><p><strong>Process Audit Data</strong></p></th>
<th><p><strong>Scanning Antivirus</strong></p></th>
<th><p><strong>Add technical fields</strong></p></th>
<th><p><strong>Create metadata in JsonStore</strong></p></th>
<th><p><strong>Call Vault to get DEK and EDEK</strong></p></th>
<th><p><strong>Encrypt and Upload files to GCS (EDEK, Reference)</strong></p></th>
<th><p><strong>Trigger create audit log (Async)</strong></p></th>
<th><p><strong>Total</strong></p></th>
<th><p><strong>Image</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p>0ms</p></td>
<td><p><span>3253ms</span></p></td>
<td><p>7ms</p></td>
<td><p>65ms</p></td>
<td><p>34ms</p></td>
<td><p><span>495ms</span></p>
<p><span>(Encryption took 182ms)</span></p></td>
<td><p>14ms</p></td>
<td><p><strong>3905ms</strong></p></td>
<td>

![[47119893374-image-20220601-102026.png]]

</td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>0ms</p></td>
<td><p><span>3175ms</span></p></td>
<td><p>5ms</p></td>
<td><p>73ms</p></td>
<td><p>47ms</p></td>
<td><p><span>435ms</span></p>
<p><span>(Encryption took 176ms)</span></p></td>
<td><p>18ms</p></td>
<td><p><strong>3796ms</strong></p></td>
<td>

![[47119893374-image-20220601-102455.png]]

</td>
</tr>
<tr>
<td><p>3</p></td>
<td><p>1ms</p></td>
<td><p><span>3130ms</span></p></td>
<td><p>7ms</p></td>
<td><p>84ms</p></td>
<td><p>31ms</p></td>
<td><p><span>518ms</span></p>
<p><span>(Encryption took 165ms)</span></p></td>
<td><p>16ms</p></td>
<td><p><strong>3822ms</strong></p></td>
<td>

![[47119893374-image-20220601-103023.png]]

</td>
</tr>
<tr>
<td><p>4</p></td>
<td><p>0ms</p></td>
<td><p><span>3130ms</span></p></td>
<td><p>7ms</p></td>
<td><p>73ms</p></td>
<td><p>30ms</p></td>
<td><p><span>624ms</span></p>
<p><span>(Encryption took 217ms)</span></p></td>
<td><p>16ms</p></td>
<td><p><strong>3917ms</strong></p></td>
<td>

![[47119893374-image-20220601-103410.png]]

</td>
</tr>
<tr>
<td><p>5</p></td>
<td><p>1ms</p></td>
<td><p><span>3147ms</span></p></td>
<td><p>5ms</p></td>
<td><p>70ms</p></td>
<td><p>31ms</p></td>
<td><p><span>503ms</span></p>
<p><span>(Encryption took 155ms)</span></p></td>
<td><p>18ms</p></td>
<td><p><strong>3818ms</strong></p></td>
<td>

![[47119893374-image-20220601-103722.png]]

</td>
</tr>
</tbody>
</table>

</div>

Test created API with the file 100MB

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="59715167-b047-4ad7-9127-e6bb6beb5d55" macro-name="view-file"><a href="../_attachments/47119893374-metadata_N.json" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47119893374/metadata_N.json?version=3&amp;modificationDate=1654082517750&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/json" data-has-thumbnail="true">

![[47119893374-metadata_N.json]]

</a></span><span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="59b9b71b-40a7-4dfc-b217-1a25009ce392" macro-name="view-file"><a href="../_attachments/47119893374-file_100mb.pdf" class="confluence-embedded-file" data-nice-type="PDF Document" data-file-src="/wiki/download/attachments/47119893374/file_100mb.pdf?version=1&amp;modificationDate=1654082504274&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/pdf" data-has-thumbnail="true">

![[47119893374-file_100mb.pdf]]

</a></span>

<div>

<table style="width:100%;">
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
<th></th>
<th><p><strong>Process Audit Data</strong></p></th>
<th><p><strong>Scanning Antivirus</strong></p></th>
<th><p><strong>Add technical fields</strong></p></th>
<th><p><strong>Create metadata in JsonStore</strong></p></th>
<th><p><strong>Call Vault to get DEK and EDEK</strong></p></th>
<th><p><strong>Encrypt and Upload files to GCS (EDEK, Reference)</strong></p></th>
<th><p><strong>Trigger create audit log (Async)</strong></p></th>
<th><p><strong>Total</strong></p></th>
<th><p><strong>Image</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p>0ms</p></td>
<td><p><span>24494ms</span></p></td>
<td><p>6ms</p></td>
<td><p>77ms</p></td>
<td><p>31ms</p></td>
<td><p><span>2245ms</span></p>
<p><span>(Encryption took 610ms)</span></p></td>
<td><p>13ms</p></td>
<td><p><strong>27179ms</strong></p></td>
<td>

![[47119893374-image-20220601-105406.png]]

</td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>0ms</p></td>
<td><p><span>24039ms</span></p></td>
<td><p>5ms</p></td>
<td><p>80ms</p></td>
<td><p>33ms</p></td>
<td><p><span>2252ms</span></p>
<p><span>(Encryption took 621ms)</span></p></td>
<td><p>16ms</p></td>
<td><p><strong>26708ms</strong></p></td>
<td>

![[47119893374-image-20220601-110105.png]]

</td>
</tr>
<tr>
<td><p>3</p></td>
<td><p>0ms</p></td>
<td><p><span>23984ms</span></p></td>
<td><p>5ms</p></td>
<td><p>83ms</p></td>
<td><p>31ms</p></td>
<td><p><span>1940ms</span></p>
<p><span>(Encryption took 588ms)</span></p></td>
<td><p>14ms</p></td>
<td><p><strong>26385ms</strong></p></td>
<td>

![[47119893374-image-20220601-110635.png]]

</td>
</tr>
<tr>
<td><p>4</p></td>
<td><p>1ms</p></td>
<td><p><span>23912ms</span></p></td>
<td><p>7ms</p></td>
<td><p>81ms</p></td>
<td><p>33ms</p></td>
<td><p><span>1871ms</span></p>
<p><span>(Encryption took 541ms)</span></p></td>
<td><p>15ms</p></td>
<td><p><strong>26255ms</strong></p></td>
<td>

![[47119893374-image-20220601-111403.png]]

</td>
</tr>
<tr>
<td><p>5</p></td>
<td><p>1ms</p></td>
<td><p><span>23979ms</span></p></td>
<td><p>7ms</p></td>
<td><p>92ms</p></td>
<td><p>32ms</p></td>
<td><p><span>1990ms</span></p>
<p><span>(Encryption took 594ms)</span></p></td>
<td><p>13ms</p></td>
<td><p><strong>26419ms</strong></p></td>
<td>

![[47119893374-image-20220601-112023.png]]

</td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Create Document API – Performance Testing Report]]
- [[Measure API luz-docs]]
- [[eArchive – Reproduce performance issue and understand the issue on DEV]]
- [[Timing Benchmark Results Document ZIP Imports]]
- [[EPC API - Load Test]]

%% ai-graph-end %%