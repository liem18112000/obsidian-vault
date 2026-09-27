---
title: "Load test webclient-nginx-ingress"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/47101706960/Load+test+webclient-nginx-ingress
space: "HACKA"
topic: infra
relevance: 0.741
depth: 2.65
updated: 2022-05-20
attachments: 24
tags:
  - confluence
  - infra
  - space/hacka
---

# Load test webclient-nginx-ingress

> [!info] Imported from Confluence
> Space **HACKA** · updated 2022-05-20 · [open original](https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/47101706960/Load+test+webclient-nginx-ingress)
> Relevance 0.741 · topic `infra`

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="7a42f842c0e0d063f9fdf9d70efd814a" macro-name="toc">

</div>

# 1. Problem

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47101706960_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-75479" macro-id="558506f2-cd13-4c49-bc87-a84130430f0e" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-75479" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-75479</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

Today we had to restart it due to the new Chrome version but it was never able to fully configure itself due to a lot of failed own domain updates such as 

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="76777c8b-4a6d-4774-8075-f343a6ae49f4" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
I0330 10:57:21.963613       1 event.go:255] Event(v1.ObjectReference{Kind:"Ingress", Namespace:"prod", Name:"od-be-gin-shop-ch-ingress", UID:"1e946ba5-8a21-402b-ab5c-8c7e32528fc3", APIVersion:"extensions/v1beta1", ResourceVersion:"303548298", FieldPath:""}): type: 'Warning' reason: 'UpdatedWithError' Configuration was updated due to updated secret prod/od-be-gin-shop-ch-tls, but not applied: Error when reloading NGINX when updating Secret: nginx reload failed: Command /usr/sbin/nginx -s reload stdout: ""
stderr: "nginx: [alert] kill(20, 1) failed (3: No such process)\n"
finished with error: exit status 1 
```

</div>

</div>

At the end the increase from 1.5 -\> 4GB made it possible to restart nginx – as you can see from the memory usage diagram, after the inital usage of approx 3GB it went down to 1GB. 

The problem is what if we have «thousands» of own domains – do we need to reserve xx GB of memory just to get ngnix started up ?

# 2. Investigation using JMeter tool

We try to simulate the case that Nginx is restarting while we have alot of users requests to webclient-nginx-ingress at the same time. We measure the resources (CPU/Memory) of webclient-nginx-ingress under an expected load.

In this load test, we use the JMeter<sup>1</sup> tool to simulate the load to webclient-nginx-ingress (Figure 1).


![[47101706960-image-20220429-072925.png]]



## 2.1. Environment and configuration

These following configrations are applied for all test cases.

- Test Scenario: The memory/cpu consumption of webclient-nginx-ingress

- Description: This test simulates alot of concurrent requests to webclient-nginx-ingress. Measure the memory/cpu consumption of webclient-nginx-ingress.

- Precondition: Have 100 Own domain ingress/tls

- Tool: JMeter

- Environment: GCP Dev-vn

- The webclient-nginx-ingress is in normal load

- Number of Users accessing (Request sampler): 1000

- Number of time to execute testing for one user (Loop time): 10

- Ramp-Up Period (The delay time between starting users): 100ms

- Testing round: 3

- Resources under test:

  - https://dev-vn.klara.tech

  - https://dev-vn.klara.tech/klara-maintenance.html

We execute the test in 3 rounds.

For one round we have 10000 requests to nginx = Request sampler \* Loop time = 1000 \* 10

## 2.1. Load test webclient-nginx-ingress without own domain ingresses

This is the first test of load testing nginx. In this scenario, we assume that we don’t have any own domain ingresses.

In the Figure 2, we can see that the CPU and memory consumption are low. It’s just 0.1 for CPU used and 100 MB for Memory used.


![[47101706960-ảnh-20220504-043546.png]]



Table 1. Summary testing for webclient-nginx-ingress without own domain ingresses

<div>

|             |          |          |                       |             |
|-------------|----------|----------|-----------------------|-------------|
| No Requests | Min (ms) | Max (ms) | Number of Request/sec | Error ( % ) |
| 30000       | 25       | 1942     | 23.1                  | 0           |

</div>

## 2.2. Load test webclient-nginx-ingress with own domain ingresses which don’t have TLS secret

This is the second test of load testing nginx. In this scenario, we assume that we have 100 own domain ingresses which don’t have TLS secret.

### 2.2.1. Adding own domain ingress

First of all, we add 100 own domain ingresses and see whether the load of webclient-nginx-ingress is increased or not.

Look at the Figure 3. we see that the load of webclient-nginx-ingress is a little bit increased while adding the own domain ingresses.


![[47101706960-ảnh-20220504-082629.png]]



### 2.2.2 Multiple requests without restarting webclient-nginx-ingress

In this case, we continuously to send multiple requests but we don’t restart the webclient-nginx-ingress at that time.


![[47101706960-ảnh-20220504-102632.png]]



Table 2. Summary testing for webclient-nginx-ingress without restarting the nginx

<div>

|             |          |          |                       |             |
|-------------|----------|----------|-----------------------|-------------|
| No Requests | Min (ms) | Max (ms) | Number of Request/sec | Error ( % ) |
| 30000       | 0        | 21000    | 93.4                  | 1.68        |

</div>

### 2.2.3 Multiple requests while restarting webclient-nginx-ingress

In this case, we restart the webclient-nginx-ingress while it’s received multiple requests. For each round, we restart the nginx once when it’s received alot of requests. Basically, we will restart it in 3 times.


![[47101706960-ảnh-20220504-105413.png]]



Table 3. Summary testing for sending multiple requests to nginx while restarting it

<div>

|             |          |          |                       |             |
|-------------|----------|----------|-----------------------|-------------|
| No Requests | Min (ms) | Max (ms) | Number of Request/sec | Error ( % ) |
| 30000       | 0        | 22262    | 59.8                  | 22.6        |

</div>

## 2.3. Load test webclient-nginx-ingress with own domain ingresses which have the TLS secret

This is the thrid test of load testing nginx. In this scenario, we assume that we have 100 own domain ingresses which have the TLS secret.

### 2.3.1 Multiple requests without restarting webclient-nginx-ingress

In this case, we continuously to send multiple requests but we don’t restart the webclient-nginx-ingress at that time.


![[47101706960-ảnh-20220504-113910.png]]



Table 4. Summary testing for sending multiple requests to nginx without restarting nginx

<div>

|             |          |          |                       |             |
|-------------|----------|----------|-----------------------|-------------|
| No Requests | Min (ms) | Max (ms) | Number of Request/sec | Error ( % ) |
| 30000       | 22       | 2300     | 61.1                  | 0           |

</div>

### 2.3.2 Multiple requests while restarting webclient-nginx-ingress


![[47101706960-ảnh-20220504-112807.png]]



Table 5. Summary testing for sending multiple requests to nginx while restarting it

<div>

|             |          |          |                       |             |
|-------------|----------|----------|-----------------------|-------------|
| No Requests | Min (ms) | Max (ms) | Number of Request/sec | Error ( % ) |
| 30000       | 0        | 15022    | 49.5                  | 46.68       |

</div>

### 2.3.3 TLS Secrets are updated

In this case, we assume that the TLS Secrets are updated.  
After that, we perform the test and restart the nginx for each round.


![[47101706960-ảnh-20220505-044856.png]]



## 2.3. Conclusion

We can see when we send multiple concurrent request to nginx without restarting it. The CPU and memory is not consumed much, just arround 0.1 (Figure 2, 4) for CPU, 80-200MB for memory (Figure 2).

In the case that we restart nginx when it’s received requests. The CPU is used most in the last test case 2.3.3, arround 1.76 (Figure 8 ). The memory is increased to ~500MB (Figure 5, 7, 8 ).

The memory is increased when we add 100 own domain ingresses (Figure 3), arround 200MB.

While restarting the nginx. The request is failed, the error percentage in Table 3, 5 is increased.

To summary, the nginx is always restarted successfully at most 500MB of memory for 100 own domain ingresses.

# 3. Investigation using Loadium tool

As we may know that in the section 2. We tried to use a local tool (JMeter) to gerneate the load for webclient-nginx-ingress. Even though we have created 1000 threads to simulate 1000 users who access the dev-vn.klara.tech at the same time, but the result is not so good, the number of requests was just arrount 57 req/s. One possible reason is that the local machine or the network isn’t strong enough to do the load test.

So that we try another solution that we use a cloud based load testing tool Loadium<sup>2</sup>. Which runs up to 250 concurrent users per account (Free subscription) in `Asia Pacific - Singapore` region<sup>3</sup>


![[47101706960-loadium load test.png]]



## 3.1. Environment and configuration

These following configrations are applied for all test cases.

- Test Scenario: The memory/cpu consumption of webclient-nginx-ingress

- Description: This test simulates alot of concurrent requests to webclient-nginx-ingress. Measure the memory/cpu consumption of webclient-nginx-ingress.

- Precondition: Have 100 Own domain ingress/tls

- Tool: Loadium

- Environment: GCP Dev-vn

- The webclient-nginx-ingress is in normal load

- Number of Users accessing (Request sampler): 250

- Test duration: ~10min or API call limit (100.000)

- Ramp-Up Period (The delay time between starting users): 0

- Location: Asia Pacific (Singapore)

- Resources under test:

  - https://dev-vn.klara.tech

For the limitation of free subscription that only allows 250 concurrent users. So that we created 4 free accounts. The total concurrent users of 4 accounts = 4 \* 250 = 1000 (Concurrent users)

## 3.2. Load test webclient-nginx-ingress with limit of 4 GB Memory

This is the first test for webclient-nginx-ingress, we keep the current memory as 4GB

### 3.2.1 Multiple requests or Normal load while restarting webclient-nginx-ingress

Repeat load test as section 2.3.1, 2.3.2.


![[47101706960-ảnh-20220518-112257.png]]



In Figure 9, we can see the A point that it consumes alot memory (3.31GB at most) compare with B point which doesn’t consume too much memory just arround 0.3GB.

Table 6. Summary the CPU/Memory usage in 2 cases

<div>

<table>
<tbody>
<tr>
<th></th>
<th><p><strong>Max CPU usage (seconds)</strong></p></th>
<th><p><strong>Max Memory usage (GB)</strong></p></th>
<th><p><strong>Min CPU usage (seconds)</strong></p></th>
<th><p><strong>Min Memory usage (GB)</strong></p></th>
</tr>
&#10;<tr>
<td><p>Case A: Nginx has alot of requests ~200 req/s</p></td>
<td><p>2.91</p></td>
<td><p>3.31</p></td>
<td rowspan="2"><p>0.01</p></td>
<td rowspan="2"><p>0.3</p></td>
</tr>
<tr>
<td><p>Case B: Nginx is in normal load - No load testing</p></td>
<td><p>1.73</p></td>
<td><p>0.27~0.3</p></td>
</tr>
</tbody>
</table>

</div>

### 3.2.2 Request/Respone stats from Loadium for one account


![[47101706960-image-20220519-110222.png]]



We got some errors (12% in Figure 10) while restarting the nginx. Some kind of errors are:

- 404

- Non HTTP response message: dev-vn.klara.tech:443 failed to respond

- Non HTTP response message: Remote host closed connection during handshake

- 500 Internal Server Error

## 3.3. Load test webclient-nginx-ingress with limit of 1.5 GB Memory

In this case, we reduce the memory from 4GB to 1.5GB

### 3.3.1 Multiple requests or Normal load while restarting webclient-nginx-ingress


![[47101706960-image-20220519-104104.png]]



In Figure 10, this time we still see the nginx consumes alot memory (~1.48GB almost full memory that we allow). For the normal load, it’s the same as before, it’s not consume much memory.

<div>

<table>
<tbody>
<tr>
<th></th>
<th><p><strong>Max CPU usage (seconds)</strong></p></th>
<th><p><strong>Max Memory usage (GB)</strong></p></th>
<th><p><strong>Min CPU usage (seconds)</strong></p></th>
<th><p><strong>Min Memory usage (GB)</strong></p></th>
</tr>
&#10;<tr>
<td><p>Case A: Nginx has alot of requests ~200 req/s</p></td>
<td><p>2.91</p></td>
<td><p>3.31</p></td>
<td rowspan="2"><p>0.01</p></td>
<td rowspan="2"><p>0.3</p></td>
</tr>
<tr>
<td><p>Case B: Nginx is in normal load - No load testing</p></td>
<td><p>1.73</p></td>
<td><p>0.27~0.3</p></td>
</tr>
</tbody>
</table>

</div>

### 3.3.2 Request/Respone stats from Loadium for one account


![[47101706960-image-20220519-113211.png]]



After we reduced the memory, the percentage error is increased very high, from 12% (Figure 10) to 80% (Figure 12). Its mean we cannot reach to the webclient-nginx-ingress this time. If we look at Figure 12, it needs arround 3GB of memory for restarting and keep serving requests, but now, we reduce the memory, and it cannot serve as much as previously.

After it restarted, we can access to klara normally.

## 3.4. Load test webclient-nginx-ingress (limit 1.5GB memory) without restarting it

While we reduce the memory to 1.5GB, we also execute an extra load test without restarting it.

### 3.4.1 Multiple requests without restarting webclient-nginx-ingress

We execute load test without restarting it. The memory is increase a bit from ~300MB to arround 500MB, nothing more. But after we finished the load test, the webclient-nginx-ingress does not release the memory usage, keep it as 500MB


![[47101706960-image-20220520-041643.png]]



It only releases the memory when we redeploy. Now the memory is back to 300MB. In the Figure 14, it keeps the memory as high as when we load test until the next deployment.


![[47101706960-image-20220520-042915.png]]



### 3.4.2 Request/Respone stats from Loadium for one account


![[47101706960-image-20220520-081613.png]]



In the figure 15, the percentage error is the most lowest compare to two previous test cases (3.2 and 3.3), one reason is that we don’t restarting the webclient-nginx-ingress.

## 3.5. Conclusion

By using the cloud load testing tool, now the webclient-nginx-ingress memory is increased during the test.

When the memory limit is huge (4GB memory), the webclien-nginx-ingress could restart and serve more request than the lower memory (1.5GB memory).

It does consume memory when we have alot of requests even though we don’t restart it (section 3.4)

The most percentage error is when the nginx has a limitation of memory (in this test is 1.5GB memory).

# Reference

1.  <a href="https://www.guru99.com/jmeter-performance-testing.html" class="external-link" rel="nofollow">https://www.guru99.com/jmeter-performance-testing.html</a>

2.  <a href="https://loadium.com/load-testing-tools" class="external-link" data-card-appearance="inline" rel="nofollow">https://loadium.com/load-testing-tools</a>

3.  <a href="https://wiki.loadium.com/test-settings/what-is-geolocation" class="external-link" data-card-appearance="inline" rel="nofollow">https://wiki.loadium.com/test-settings/what-is-geolocation</a>
