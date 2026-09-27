---
title: "Performance Test: 100% Thumbnail Cloud Run"
created: 2026-04-10
updated: 2026-04-13
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49314463780/Performance+Test+100+Thumbnail+Cloud+Run
confluence_id: "49314463780"
confluence_path: "Team Kepler > Developer note"
tags: [confluence, performance, cloud-run, thumbnail]
---

# Performance Test: 100% Thumbnail Cloud Run

*Confluence source · Team Kepler › Developer note · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49314463780/Performance+Test+100+Thumbnail+Cloud+Run) · updated 2026-04-13*

![[image-20260410-074859.png]]![[image-20260410-074706.png]]

Log link: [https://cloudlogging.app.goo.gl/5PhUkEtw7jnwBTFNA](https://cloudlogging.app.goo.gl/5PhUkEtw7jnwBTFNA)

![[image-20260413-104725.png]]

[Metric Link](https://console.cloud.google.com/monitoring/metrics-explorer;endTime=2026-04-13T01:46:35.213Z;startTime=2026-04-13T01:33:35.213Z?pageState=%7B%22xyChart%22:%7B%22constantLines%22:%5B%5D,%22dataSets%22:%5B%7B%22legendTemplate%22:%22$%7Bmetric.labels.state%7D%22,%22plotType%22:%22LINE%22,%22pointConnectionMethod%22:%22GAP_DETECTION%22,%22targetAxis%22:%22Y1%22,%22timeSeriesFilter%22:%7B%22aggregations%22:%5B%7B%22alignmentPeriod%22:%2230s%22,%22crossSeriesReducer%22:%22REDUCE_SUM%22,%22groupByFields%22:%5B%22resource.label.%5C%22service_name%5C%22%22,%22metric.label.%5C%22state%5C%22%22%5D,%22perSeriesAligner%22:%22ALIGN_MAX%22%7D%5D,%22apiSource%22:%22DEFAULT_CLOUD%22,%22crossSeriesReducer%22:%22REDUCE_SUM%22,%22filter%22:%22metric.type%3D%5C%22run.googleapis.com%2Fcontainer%2Finstance_count%5C%22%20resource.type%3D%5C%22cloud_run_revision%5C%22%22,%22groupByFields%22:%5B%22resource.label.%5C%22service_name%5C%22%22,%22metric.label.%5C%22state%5C%22%22%5D,%22minAlignmentPeriod%22:%2230s%22,%22perSeriesAligner%22:%22ALIGN_MAX%22%7D%7D%5D,%22options%22:%7B%22mode%22:%22COLOR%22%7D,%22y1Axis%22:%7B%22label%22:%22%22,%22scale%22:%22LINEAR%22%7D%7D%7D&project=klara-performance)

![[image-20260410-074621.png]]

### Scaling history (from `gcloud monitoring`)

|                |             |                  |         |          |         |
|----------------|-------------|------------------|---------|----------|---------|
| Minute (GMT+7) | UTC         | Active instances | 2xx/min | p95 (ms) | CPU max |
| 08:34          | 01:34       | 72               | —       | —        | 3%      |
| 08:36          | 01:36       | 39               | (ramp)  | 5266     | 69%     |
| 08:37          | 01:37       | 84               | —       | 691      | 100%    |
| 08:38          | 01:38       | **120 (max)**    | —       | 465      | 100%    |
| 08:39–08:43    | 01:39–01:43 | **120 (cap)**    | ≈14.4K  | 424–453  | 66–73%  |
| 08:44          | 01:44       | 117              | —       | 424      | 61%     |
| 08:45          | 01:45       | 62               | drain   | —        | 4%      |

### Key observations

- **Hit** `maxScale=120` **for 6 consecutive minutes.** The autoscaler clamped at the configured ceiling.

- **Cold-start hump:** p95 spiked to 5.3 s in the first minute (ramp 39 → 120 instances), then settled to ~430 ms.

- **CPU saturated at 100%** during the ramp; once at 120 instances it dropped to 66–73% — comfortable headroom *at this RPS only because the cap was sufficient*.

- **No errors** because 240 RPS × ~430 ms ≈ 103 in-flight reqs, well under 120 × 8 (containerConcurrency) = 960 capacity.

- Cold-start latency observed: ~10.5 s per new instance.

### Verdict

240 RPS is a **safe operating point** for the current Cloud Run config. The service hit the maxScale ceiling but had concurrency headroom; raising RPS without raising `maxScale` would tip into queueing.

![[image-20260410-074859.png]]![[image-20260410-085121.png]]

Log Link: [https://cloudlogging.app.goo.gl/rfPKJ4kAKR2oLoQD7](https://cloudlogging.app.goo.gl/rfPKJ4kAKR2oLoQD7)

![[image-20260413-105147.png]]

[Metric Link](https://console.cloud.google.com/monitoring/metrics-explorer;endTime=2026-04-13T02:25:35.213Z;startTime=2026-04-13T02:10:35.213Z?pageState=%7B%22xyChart%22:%7B%22constantLines%22:%5B%5D,%22dataSets%22:%5B%7B%22legendTemplate%22:%22$%7Bmetric.labels.state%7D%22,%22plotType%22:%22LINE%22,%22pointConnectionMethod%22:%22GAP_DETECTION%22,%22targetAxis%22:%22Y1%22,%22timeSeriesFilter%22:%7B%22aggregations%22:%5B%7B%22alignmentPeriod%22:%2230s%22,%22crossSeriesReducer%22:%22REDUCE_SUM%22,%22groupByFields%22:%5B%22resource.label.%5C%22service_name%5C%22%22,%22metric.label.%5C%22state%5C%22%22%5D,%22perSeriesAligner%22:%22ALIGN_MAX%22%7D%5D,%22apiSource%22:%22DEFAULT_CLOUD%22,%22crossSeriesReducer%22:%22REDUCE_SUM%22,%22filter%22:%22metric.type%3D%5C%22run.googleapis.com%2Fcontainer%2Finstance_count%5C%22%20resource.type%3D%5C%22cloud_run_revision%5C%22%22,%22groupByFields%22:%5B%22resource.label.%5C%22service_name%5C%22%22,%22metric.label.%5C%22state%5C%22%22%5D,%22minAlignmentPeriod%22:%2230s%22,%22perSeriesAligner%22:%22ALIGN_MAX%22%7D%7D%5D,%22options%22:%7B%22mode%22:%22COLOR%22%7D,%22y1Axis%22:%7B%22label%22:%22%22,%22scale%22:%22LINEAR%22%7D%7D%7D&project=klara-performance)

![[image-20260410-085010.png]]

### Cloud Run window — best-fit

- UTC: `2026-04-12 01:54 → 02:09`

- **GMT+7:** `2026-04-12 08:54 → 09:09` (sustained 25 K req/min ≈ 417 RPS — matches achieved rate)

### Scaling history

|                |             |                  |               |          |         |
|----------------|-------------|------------------|---------------|----------|---------|
| Minute (GMT+7) | UTC         | Active instances | 2xx/min       | p95 (ms) | CPU max |
| 08:55          | 01:55       | 14 (start)       | 1.6K          | 5101     | 81%     |
| 08:56          | 01:56       | 55               | 7.5K          | 913      | 100%    |
| 08:57          | 01:57       | 105              | 13.5K         | 590      | 100%    |
| 08:58          | 01:58       | **120 (cap)**    | 15.9K         | 532      | 85%     |
| 09:01–09:08    | 02:01–02:08 | **120 (cap)**    | 23.9–26.5K    | 295–367  | 100%    |
| 09:09          | 02:09       | 120              | 19.4K (drain) | 295      | 100%    |
| 09:10          | 02:10       | 115              | —             | —        | 69%     |

### Key observations

- **Saturated at maxScale=120 for 11+ minutes**, with **CPU pinned at 100%** for 8 of those — clear capacity ceiling.

- The k6 PDF reports 5.17 % failures with avg latency 741 ms and p95 2.65 s — these symptoms (queue build-up, latency inflation) match a Cloud Run service running at its instance cap with CPU saturated.

- However, **no 5xx response codes were logged in this exact window.** The 5,175 failures are likely client-side: k6 timeouts, connection resets, or upstream overload before the request was logged by Cloud Run.

- Cold start ~10.5 s per new instance — visible as the 5.1 s p95 spike on the first minute.

### Verdict

At 480 RPS (~25 K req/min), the service is **at its hard ceiling**: 120 instances × 8 concurrency × ~330 ms processing ≈ 2,900 RPS theoretical, but CPU saturates well before — practical ceiling sits around the achieved 417 RPS. To clear the wall, raise `autoscaling.knative.dev/maxScale` and/or raise CPU per instance.

![[image-20260410-074859.png]]![[image-20260410-085845.png]]

Log Link: [https://cloudlogging.app.goo.gl/Gae9PJGwt1aqgUeS8](https://cloudlogging.app.goo.gl/Gae9PJGwt1aqgUeS8)

![[image-20260413-105359.png]]

[Metric Link](https://console.cloud.google.com/monitoring/metrics-explorer;endTime=2026-04-13T04:11:35.213Z;startTime=2026-04-13T04:00:35.213Z?pageState=%7B%22xyChart%22:%7B%22constantLines%22:%5B%5D,%22dataSets%22:%5B%7B%22legendTemplate%22:%22$%7Bmetric.labels.state%7D%22,%22plotType%22:%22LINE%22,%22pointConnectionMethod%22:%22GAP_DETECTION%22,%22targetAxis%22:%22Y1%22,%22timeSeriesFilter%22:%7B%22aggregations%22:%5B%7B%22alignmentPeriod%22:%2230s%22,%22crossSeriesReducer%22:%22REDUCE_SUM%22,%22groupByFields%22:%5B%22resource.label.%5C%22service_name%5C%22%22,%22metric.label.%5C%22state%5C%22%22%5D,%22perSeriesAligner%22:%22ALIGN_MAX%22%7D%5D,%22apiSource%22:%22DEFAULT_CLOUD%22,%22crossSeriesReducer%22:%22REDUCE_SUM%22,%22filter%22:%22metric.type%3D%5C%22run.googleapis.com%2Fcontainer%2Finstance_count%5C%22%20resource.type%3D%5C%22cloud_run_revision%5C%22%22,%22groupByFields%22:%5B%22resource.label.%5C%22service_name%5C%22%22,%22metric.label.%5C%22state%5C%22%22%5D,%22minAlignmentPeriod%22:%2230s%22,%22perSeriesAligner%22:%22ALIGN_MAX%22%7D%7D%5D,%22options%22:%7B%22mode%22:%22COLOR%22%7D,%22y1Axis%22:%7B%22label%22:%22%22,%22scale%22:%22LINEAR%22%7D%7D%7D&project=klara-performance)

![[image-20260410-090753.png]]

### Cloud Run window

- UTC: `2026-04-12 04:00 → 04:10`

- **GMT+7:** `2026-04-12 11:00 → 11:10` (~10 min, sustained 25–27 K req/min ≈ 420–450 RPS)

### Scaling history

|                |       |                  |         |             |          |      |
|----------------|-------|------------------|---------|-------------|----------|------|
| Minute (GMT+7) | UTC   | Active instances | 2xx/min | **5xx/min** | p95 (ms) | CPU  |
| 11:01          | 04:01 | 116              | 13.9K   | **320**     | 5286     | 92%  |
| 11:02          | 04:02 | **120 (cap)**    | 24.1K   | **901**     | 578      | 100% |
| 11:03          | 04:03 | **120 (cap)**    | 25.1K   | **1,131**   | 529      | 100% |
| 11:04          | 04:04 | **120 (cap)**    | 25.8K   | **1,147**   | 508      | 100% |
| 11:05          | 04:05 | **120 (cap)**    | 26.5K   | **1,122**   | 496      | 100% |
| 11:06          | 04:06 | **120 (cap)**    | 26.9K   | **1,008**   | 491      | 100% |
| 11:07          | 04:07 | **120 (cap)**    | 25.4K   | **1,817**   | 489      | 100% |
| 11:08          | 04:08 | **120 (cap)**    | 18.8K   | **3,211** ⚠ | 504      | 100% |
| 11:09          | 04:09 | **120 (cap)**    | 17.9K   | **3,573** ⚠ | 504      | 100% |
| 11:10          | 04:10 | **120 (cap)**    | 16.3K   | **3,104** ⚠ | 521      | 100% |

**Total 5xx logged: ~17,330** (k6 reports 11,947 — discrepancy likely from window boundary or test-run overlap.)

### Key observations

- **Hard saturation:** maxScale=120 + CPU=100% for the entire test. The autoscaler had nothing left to give.

- **Error rate compounds over time:** 5xx grows from 320/min to 3,573/min as in-flight requests pile up — classic overload pattern. 2xx throughput *drops* (26.9K → 16.3K) as the service spends cycles on failing requests.

- **p95 stays near 500 ms** even during failure storm — Cloud Run sheds excess load (returns 5xx fast) rather than queuing, which is why latency doesn't blow up.

- Cold start (~10.5 s) accounts for the 5.3 s spike on minute 1.
