---
title: "Create Document API – Performance Testing Report"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48709795997/Create+Document+API+Performance+Testing+Report
space: "TK"
topic: programming
relevance: 0.738
depth: 2.65
updated: 2025-10-15
attachments: 17
tags:
  - confluence
  - programming
  - space/tk
---

# Create Document API – Performance Testing Report

> [!info] Imported from Confluence
> Space **TK** · updated 2025-10-15 · [open original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48709795997/Create+Document+API+Performance+Testing+Report)
> Relevance 0.738 · topic `programming`

# 1. Test Case Description

## Objective

Evaluate the Create Document API for performance, stability, and error handling under heavy load, with a target throughput of 140 requests per second (RPS)

# Test Results

## **Environment setup**

### Resource Requests & Limits

- **Requests** (minimum guaranteed resources per pod):

  - CPU: **100m** (0.1 vCPU)

  - Memory: **10Gi**

- **Limits** (maximum allowed per pod):

  - CPU: **15** (15 vCPUs)

  - Memory: **10Gi**

### Deployment Details

- **Replicas defined:** `10`

**Test case:**

1.  Test with create document full process including virus scan, auditlog, enrichment process

<div>

<table>
<colgroup>
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Case</strong></p></th>
<th><p><strong>Target RPS</strong></p></th>
<th><p><strong>Max VUs</strong></p></th>
<th><p><strong>Duration (m)</strong></p></th>
<th><p><strong>Requests Sent</strong></p></th>
<th><p><strong>Achieved RPS</strong></p></th>
<th><p><strong>Timeout</strong></p></th>
<th><p><strong>Errors</strong></p></th>
<th><p><strong>Median time (ms)</strong></p></th>
<th><p><strong>luz-docs pods scaling</strong></p></th>
<th><p><strong>luz-antivirus scaling</strong></p></th>
<th><p><strong>k6 result</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p>30</p></td>
<td><p>500</p></td>
<td><p>10 m</p></td>
<td><p>17562</p></td>
<td><p>29.22889/s</p></td>
<td><p>30s</p></td>
<td><p>1337 (7.61%)<br />
Mostly exceed timeout 30s</p></td>
<td><p>1.06s</p></td>
<td><p>10 pods</p></td>
<td><p>From 4 to 14/20 pods</p></td>
<td>

![[48709795997-image-20251001-102126.png]]

</td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>40</p></td>
<td><p>500</p></td>
<td><p>10m</p></td>
<td><p>20934</p></td>
<td><p>33.327444/s</p></td>
<td><p>30s</p></td>
<td><p>4998 (23.87%)<br />
Mostly exceed timeout 30s</p></td>
<td><p>7.63s</p></td>
<td><p>10 pods</p></td>
<td><p>From 4 to 16/20 pods</p></td>
<td>

![[48709795997-image-20251001-110639.png]]

</td>
</tr>
<tr>
<td><p><span style="background-color: rgb(211,241,167);">3</span></p></td>
<td><p><span style="background-color: rgb(211,241,167);">50</span></p></td>
<td><p><span style="background-color: rgb(211,241,167);">500</span></p></td>
<td><p><span style="background-color: rgb(211,241,167);">10m</span></p></td>
<td><p><span style="background-color: rgb(211,241,167);">29788</span></p></td>
<td><p><span style="background-color: rgb(211,241,167);">49.447234/s</span></p></td>
<td><p><span style="background-color: rgb(211,241,167);">30s</span></p></td>
<td><p><span style="background-color: rgb(211,241,167);">135 (0.45%)</span></p></td>
<td><p><span style="background-color: rgb(211,241,167);">1.08s</span></p></td>
<td><p><span style="background-color: rgb(211,241,167);">10 pods</span></p></td>
<td><p><span style="background-color: rgb(211,241,167);">Always 20/20 pods</span></p></td>
<td>

![[48709795997-image-20251001-112122.png]]

</td>
</tr>
<tr>
<td><p>4</p></td>
<td><p>60</p></td>
<td><p>500</p></td>
<td><p>10m</p></td>
<td><p>21888</p></td>
<td><p>34.742675/s</p></td>
<td><p>30s</p></td>
<td><p>5871 (26.82%)</p>
<p>Mostly exceed timeout 30s</p></td>
<td><p>9.38s</p></td>
<td><p>10 pods</p></td>
<td><p>From 4 to 17/20 pods</p></td>
<td>

![[48709795997-image-20251001-104756.png]]

</td>
</tr>
<tr>
<td><p><span style="background-color: rgb(211,241,167);">5</span></p></td>
<td><p><span style="background-color: rgb(211,241,167);">60</span></p></td>
<td><p><span style="background-color: rgb(211,241,167);">500</span></p></td>
<td><p><span style="background-color: rgb(211,241,167);">10m</span></p></td>
<td><p><span style="background-color: rgb(211,241,167);">34517</span></p></td>
<td><p><span style="background-color: rgb(211,241,167);">56.233842/s</span></p></td>
<td><p><span style="background-color: rgb(211,241,167);">30s</span></p></td>
<td><p><span style="background-color: rgb(211,241,167);">153 (0.44%)</span></p></td>
<td><p><span style="background-color: rgb(211,241,167);">5.44s</span></p></td>
<td><p><span style="background-color: rgb(211,241,167);">10 pods</span></p></td>
<td><p><span style="background-color: rgb(211,241,167);">Always 20/20 pods</span></p></td>
<td>

![[48709795997-image-20251002-080421.png]]

</td>
</tr>
<tr>
<td><p>6</p></td>
<td><p>140</p></td>
<td><p>500</p></td>
<td><p>10m</p></td>
<td><p>39656</p></td>
<td><p><span style="background-color: rgb(254,222,200);">63.351613/s</span></p></td>
<td><p>30s</p></td>
<td><p>1228 (3.09%)</p></td>
<td><p>7.38s</p></td>
<td><p>10 pods</p></td>
<td><p>Always 20/20 pods</p></td>
<td>

![[48709795997-image-20251002-104123.png]]

</td>
</tr>
</tbody>
</table>

</div>

**Special case:**

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Category</strong></p></th>
<th><p><strong>Details</strong></p></th>
</tr>
&#10;<tr>
<td><p><strong>Target RPS</strong></p></td>
<td><p>140</p></td>
</tr>
<tr>
<td><p><strong>Duration</strong></p></td>
<td><p>10 minutes</p></td>
</tr>
<tr>
<td><p><strong>Max VUs</strong></p></td>
<td><p>500</p></td>
</tr>
<tr>
<td><p><strong>Pods Configuration</strong></p></td>
<td><ul>
<li><p><strong><span style="background-color: rgb(211,241,167);">luz-docs:</span></strong><span style="background-color: rgb(211,241,167);"> 10 pods</span></p></li>
<li><p><strong><span style="background-color: rgb(211,241,167);">luz-antivirus:</span></strong><span style="background-color: rgb(211,241,167);"> 40 pods</span></p></li>
<li><p><strong><span style="background-color: rgb(211,241,167);">luz-message-broker:</span></strong><span style="background-color: rgb(211,241,167);"> 20 pods</span></p></li>
<li><p><strong><span style="background-color: rgb(211,241,167);">luz-audit:</span></strong><span style="background-color: rgb(211,241,167);"> 10 pods</span></p></li>
</ul></td>
</tr>
<tr>
<td><p><strong>Achieved RPS</strong></p></td>
<td><p><span style="background-color: rgb(211,241,167);">121</span></p></td>
</tr>
<tr>
<td><p><strong>Total Requests</strong></p></td>
<td><p>74,174</p></td>
</tr>
<tr>
<td><p><strong>Errors</strong></p></td>
<td><p>0</p></td>
</tr>
<tr>
<td><p><strong>K6 result</strong></p></td>
<td>

![[48709795997-image-20251015-063729.png]]

</td>
</tr>
</tbody>
</table>

</div>

**Key Insights:**

- System is **stable at ~50 RPS**, with very low errors and good latency, provided antivirus is at 20/20.

- **Antivirus dependency:** Timeouts occur when antivirus pods are still scaling. Pre-scaling to full (20/20) improves stability drastically.

- **Luz-message-broker dependency:** When reaching high RPS, **luz-message-broker** needs to scale up; otherwise, the audit log creation process may fail, resulting in **503 errors**.

- **Luz-docs asynchronous processes** such as *first-request jobs* and *enrichers* impact overall performance when handling high RPS.

- **Bottleneck:** Luz-antivirus, Luz-message-broker

- **140 RPS target not achievable** under current configuration (limited to ~63 RPS).  

2.  Test with create document with auditlog, enricher but skip scan virus

<div>

<table>
<colgroup>
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 9%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Case</strong></p></th>
<th><p><strong>Target RPS</strong></p></th>
<th><p><strong>Max VUs</strong></p></th>
<th><p><strong>Duration (m)</strong></p></th>
<th><p><strong>Requests Sent</strong></p></th>
<th><p><strong>Achieved RPS</strong></p></th>
<th><p><strong>Timeout</strong></p></th>
<th><p><strong>Errors</strong></p></th>
<th><p><strong>Median time (ms)</strong></p></th>
<th><p><strong>luz-docs pods scaling</strong></p></th>
<th><p><strong>k6 result</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p>60</p></td>
<td><p>500</p></td>
<td><p>10 m</p></td>
<td><p>35982</p></td>
<td><p>59.945441/s</p></td>
<td><p>30s</p></td>
<td><p>153 (0.42%)<br />
Mostly failed when start up k6, request is not ready</p></td>
<td><p>159.92ms</p></td>
<td><p>10 pods</p></td>
<td>

![[48709795997-image-20251002-063144.png]]

</td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>80</p></td>
<td><p>500</p></td>
<td><p>10m</p></td>
<td><p>47926</p></td>
<td><p>79.832729/s</p></td>
<td><p>30s</p></td>
<td><p>205 (0.42%)<br />
Mostly failed when start up k6, request is not ready</p></td>
<td><p>201.01ms</p></td>
<td><p>10 pods</p></td>
<td>

![[48709795997-image-20251002-065106.png]]

</td>
</tr>
<tr>
<td><p>3</p></td>
<td><p>100</p></td>
<td><p>500</p></td>
<td><p>10m</p></td>
<td><p>59939</p></td>
<td><p>99.834344/s</p></td>
<td><p>30s</p></td>
<td><p>256 (0.42%)<br />
Mostly failed when start up k6, request is not ready</p></td>
<td><p>170.54ms</p></td>
<td><p>10 pods</p></td>
<td>

![[48709795997-image-20251002-070528.png]]

</td>
</tr>
<tr>
<td><p>4</p></td>
<td><p>140</p></td>
<td><p>500</p></td>
<td><p>10m</p></td>
<td><p>83821</p></td>
<td><p>139.50837/s</p></td>
<td><p>30s</p></td>
<td><p>495 (0.59%)<br />
Mostly failed when start up k6, request is not ready</p></td>
<td><p>259.74ms</p></td>
<td><p>10 pods</p></td>
<td>

![[48709795997-image-20251002-072034.png]]

</td>
</tr>
<tr>
<td><p>5</p></td>
<td><p>200</p></td>
<td><p>500</p></td>
<td><p>10m</p></td>
<td><p>118754</p></td>
<td><p>197.452841/s</p></td>
<td><p>30s</p></td>
<td><p>742 (0.62%)<br />
Mostly failed when start up k6, request is not ready</p></td>
<td><p>740.19ms</p></td>
<td><p>10 pods</p></td>
<td>

![[48709795997-image-20251002-085458.png]]

</td>
</tr>
<tr>
<td><p>6</p></td>
<td><p>400</p></td>
<td><p>500</p></td>
<td><p>10</p></td>
<td><p>150614</p></td>
<td><p>248.176393/s</p></td>
<td><p>30s</p></td>
<td><p>2<br />
<br />
<em>This run has setup k6 better with healthcheck call first. So there is no failed when startup</em></p></td>
<td><p>1.78s</p></td>
<td><p>10 pods</p></td>
<td>

![[48709795997-image-20251002-092018.png]]

</td>
</tr>
<tr>
<td><p>7</p></td>
<td><p>400</p></td>
<td><p>1000</p></td>
<td><p>10</p></td>
<td><p>155773</p></td>
<td><p>254.320398/s</p></td>
<td><p>30s</p></td>
<td><p>310 (0.19%)</p></td>
<td><p>2.5s</p></td>
<td><p>10 pods</p></td>
<td>

![[48709795997-image-20251002-101405.png]]

</td>
</tr>
</tbody>
</table>

</div>

**Conclusion:** Skipping antivirus removes a major performance bottleneck, enabling achievement of 250 RPS.
