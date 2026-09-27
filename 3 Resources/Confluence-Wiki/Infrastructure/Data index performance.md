---
title: "Data index performance"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48575152168/Data+index+performance
space: "FUT"
topic: infra
relevance: 0.724
depth: 2.56
updated: 2025-07-23
attachments: 25
tags:
  - confluence
  - infra
  - space/fut
---

# Data index performance

> [!info] Imported from Confluence
> Space **FUT** · updated 2025-07-23 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48575152168/Data+index+performance)
> Relevance 0.724 · topic `infra`

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="6ef68e09-1cd1-4c5b-92c6-8b9b436083c1" macro-name="toc">

</div>

# Design


![[48575152168-data-index.drawio (1).png]]



# Performance

<div>

<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>setup</strong></p></th>
<th><p><strong>duration</strong></p></th>
<th><p><strong>result (data-index deployment)</strong></p></th>
<th><p><strong>pubsub metric</strong></p></th>
<th><p><strong>mongodb metric</strong></p></th>
<th></th>
</tr>
&#10;<tr>
<td><ul>
<li><p>data index (5 pods)</p>
<ul>
<li><p>2 CPU 2GB RAM</p></li>
<li><p>pubsub consumer</p>
<ul>
<li><p>10 executor threads</p></li>
<li><p>message pool size 100</p></li>
<li><p>ack deadline 10s (extend max 30s)</p></li>
</ul></li>
</ul></li>
</ul>

![[48575152168-image-20250710-090010.png]]


<ul>
<li><p>serverless workflow (10 pods)</p>
<ul>
<li><p>2 CPU 2GB RAM</p></li>
</ul></li>
</ul>
<p><strong>Load</strong>: <a href="https://cloudlogging.app.goo.gl/B28BtkEPWmhp1EPaA" class="external-link" rel="nofollow">1.4 mil</a> events in total (for 103k workflow instances)</p></td>
<td><p>1h30m</p></td>
<td><ul>
<li><p>ack max 480 msgs/s</p></li>
<li><p>99% percentile 300ms <a href="https://cloudlogging.app.goo.gl/bTgxziVaf4JwoQHU9" class="external-link" rel="nofollow">log</a></p></li>
</ul></td>
<td><p><a href="https://console.cloud.google.com/monitoring/metrics-explorer;endTime=2025-07-10T05:37:56.654Z;startTime=2025-07-10T03:47:36.695Z?pageState=%7B%22xyChart%22:%7B%22constantLines%22:%5B%5D,%22dataSets%22:%5B%7B%22plotType%22:%22LINE%22,%22pointConnectionMethod%22:%22GAP_DETECTION%22,%22targetAxis%22:%22Y1%22,%22timeSeriesFilter%22:%7B%22aggregations%22:%5B%7B%22alignmentPeriod%22:%2260s%22,%22crossSeriesReducer%22:%22REDUCE_NONE%22,%22groupByFields%22:%5B%5D,%22perSeriesAligner%22:%22ALIGN_RATE%22%7D%5D,%22apiSource%22:%22DEFAULT_CLOUD%22,%22crossSeriesReducer%22:%22REDUCE_NONE%22,%22filter%22:%22metric.type%3D%5C%22pubsub.googleapis.com%2Fsubscription%2Fsent_message_count%5C%22%20resource.type%3D%5C%22pubsub_subscription%5C%22%20resource.label.%5C%22subscription_id%5C%22%3D%5C%22pubsub-subscription-performance-epost-workflow-events%5C%22%20metric.label.%5C%22delivery_type%5C%22%3D%5C%22streaming_pull%5C%22%22,%22groupByFields%22:%5B%5D,%22minAlignmentPeriod%22:%2260s%22,%22perSeriesAligner%22:%22ALIGN_RATE%22%7D%7D,%7B%22plotType%22:%22LINE%22,%22pointConnectionMethod%22:%22GAP_DETECTION%22,%22targetAxis%22:%22Y1%22,%22timeSeriesFilter%22:%7B%22aggregations%22:%5B%7B%22alignmentPeriod%22:%2260s%22,%22crossSeriesReducer%22:%22REDUCE_NONE%22,%22groupByFields%22:%5B%5D,%22perSeriesAligner%22:%22ALIGN_RATE%22%7D%5D,%22apiSource%22:%22DEFAULT_CLOUD%22,%22crossSeriesReducer%22:%22REDUCE_NONE%22,%22filter%22:%22metric.type%3D%5C%22pubsub.googleapis.com%2Fsubscription%2Fpull_ack_message_operation_count%5C%22%20resource.type%3D%5C%22pubsub_subscription%5C%22%20resource.label.%5C%22subscription_id%5C%22%3D%5C%22pubsub-subscription-performance-epost-workflow-events%5C%22%20metric.label.%5C%22response_code%5C%22%3D%5C%22success%5C%22%22,%22groupByFields%22:%5B%5D,%22minAlignmentPeriod%22:%2260s%22,%22perSeriesAligner%22:%22ALIGN_RATE%22%7D%7D,%7B%22plotType%22:%22LINE%22,%22pointConnectionMethod%22:%22GAP_DETECTION%22,%22targetAxis%22:%22Y2%22,%22timeSeriesFilter%22:%7B%22aggregations%22:%5B%7B%22alignmentPeriod%22:%2260s%22,%22crossSeriesReducer%22:%22REDUCE_PERCENTILE_95%22,%22groupByFields%22:%5B%5D,%22perSeriesAligner%22:%22ALIGN_DELTA%22%7D%5D,%22apiSource%22:%22DEFAULT_CLOUD%22,%22crossSeriesReducer%22:%22REDUCE_PERCENTILE_95%22,%22filter%22:%22metric.type%3D%5C%22pubsub.googleapis.com%2Fsubscription%2Fack_latencies%5C%22%20resource.type%3D%5C%22pubsub_subscription%5C%22%20resource.label.%5C%22subscription_id%5C%22%3D%5C%22pubsub-subscription-performance-epost-workflow-events%5C%22%22,%22groupByFields%22:%5B%5D,%22minAlignmentPeriod%22:%2260s%22,%22perSeriesAligner%22:%22ALIGN_DELTA%22%7D%7D,%7B%22plotType%22:%22LINE%22,%22pointConnectionMethod%22:%22GAP_DETECTION%22,%22targetAxis%22:%22Y1%22,%22timeSeriesFilter%22:%7B%22aggregations%22:%5B%7B%22alignmentPeriod%22:%2260s%22,%22crossSeriesReducer%22:%22REDUCE_NONE%22,%22groupByFields%22:%5B%5D,%22perSeriesAligner%22:%22ALIGN_RATE%22%7D%5D,%22apiSource%22:%22DEFAULT_CLOUD%22,%22crossSeriesReducer%22:%22REDUCE_NONE%22,%22filter%22:%22metric.type%3D%5C%22pubsub.googleapis.com%2Fsubscription%2Fpull_ack_request_count%5C%22%20resource.type%3D%5C%22pubsub_subscription%5C%22%20resource.label.%5C%22subscription_id%5C%22%3D%5C%22pubsub-subscription-performance-epost-workflow-events%5C%22%20resource.label.%5C%22project_id%5C%22%3D%5C%22klara-performance%5C%22%20metadata.system_labels.%5C%22name%5C%22%3D%5C%22pubsub-subscription-performance-epost-workflow-events%5C%22%22,%22groupByFields%22:%5B%5D,%22minAlignmentPeriod%22:%2260s%22,%22perSeriesAligner%22:%22ALIGN_RATE%22%7D%7D,%7B%22plotType%22:%22LINE%22,%22pointConnectionMethod%22:%22GAP_DETECTION%22,%22targetAxis%22:%22Y1%22,%22timeSeriesFilter%22:%7B%22aggregations%22:%5B%7B%22alignmentPeriod%22:%2260s%22,%22crossSeriesReducer%22:%22REDUCE_NONE%22,%22groupByFields%22:%5B%5D,%22perSeriesAligner%22:%22ALIGN_RATE%22%7D%5D,%22apiSource%22:%22DEFAULT_CLOUD%22,%22crossSeriesReducer%22:%22REDUCE_NONE%22,%22filter%22:%22metric.type%3D%5C%22pubsub.googleapis.com%2Fsubscription%2Fmod_ack_deadline_request_count%5C%22%20resource.type%3D%5C%22pubsub_subscription%5C%22%20resource.label.%5C%22subscription_id%5C%22%3D%5C%22pubsub-subscription-performance-epost-workflow-events%5C%22%20resource.label.%5C%22project_id%5C%22%3D%5C%22klara-performance%5C%22%20metadata.system_labels.%5C%22name%5C%22%3D%5C%22pubsub-subscription-performance-epost-workflow-events%5C%22%22,%22groupByFields%22:%5B%5D,%22minAlignmentPeriod%22:%2260s%22,%22perSeriesAligner%22:%22ALIGN_RATE%22%7D%7D%5D,%22options%22:%7B%22mode%22:%22COLOR%22%7D,%22y1Axis%22:%7B%22label%22:%22%22,%22scale%22:%22LINEAR%22%7D,%22y2Axis%22:%7B%22label%22:%22%22,%22scale%22:%22LINEAR%22%7D%7D%7D&amp;project=klara-performance" class="external-link" rel="nofollow">pubsub metric</a></p>

![[48575152168-image-20250710-085854.png]]



![[48575152168-image-20250715-072702.png]]


<p><br />
</p></td>
<td rowspan="2">

![[48575152168-image-20250714-045758.png]]



![[48575152168-image-20250714-045343.png]]

</td>
<td rowspan="2"><p>in conclusion</p>
<ul>
<li><p>1 pod (with minimum resource 1 cpu 2 GB) can proceed up to 100 msgs/s and more or less 10 threads</p></li>
<li><p>the duration here is not only show how data index performant but also depends on the performance of publishing event to pubsub from client</p></li>
<li><p>apply scaling policy based on the number of pending messages/requests</p></li>
</ul></td>
</tr>
<tr>
<td><ul>
<li><p>data index (5 pods)</p>
<ul>
<li><p>2 CPU 2GB RAM</p></li>
<li><p>pubsub consumer</p>
<ul>
<li><p>12 executor threads</p></li>
<li><p>message pool size 200</p></li>
<li><p>ack deadline 10s (extend max 30s)</p></li>
</ul></li>
</ul></li>
</ul>

![[48575152168-image-20250714-034248.png]]


<ul>
<li><p>serverless workflow (10 pods)</p>
<ul>
<li><p>2 CPU 2GB RAM</p></li>
</ul></li>
</ul>
<p><strong>Load</strong>: <a href="https://cloudlogging.app.goo.gl/XCBXHb3dpB1UuGYV8" class="external-link" rel="nofollow">965k</a> events/messages in total (for 70K workflow instances)</p></td>
<td><p>40 mins</p></td>
<td><ul>
<li><p>ack max 550 msgs/s</p></li>
<li><p>99% percentile 300ms <a href="https://cloudlogging.app.goo.gl/BBnaKZmZdjqED5wD7" class="external-link" rel="nofollow">log</a></p></li>
</ul></td>
<td><p><a href="https://console.cloud.google.com/monitoring/metrics-explorer;endTime=2025-07-10T10:31:59.687Z;startTime=2025-07-10T09:40:42.560Z?pageState=%7B%22xyChart%22:%7B%22constantLines%22:%5B%5D,%22dataSets%22:%5B%7B%22plotType%22:%22LINE%22,%22pointConnectionMethod%22:%22GAP_DETECTION%22,%22targetAxis%22:%22Y1%22,%22timeSeriesFilter%22:%7B%22aggregations%22:%5B%7B%22alignmentPeriod%22:%2260s%22,%22crossSeriesReducer%22:%22REDUCE_NONE%22,%22groupByFields%22:%5B%5D,%22perSeriesAligner%22:%22ALIGN_RATE%22%7D%5D,%22apiSource%22:%22DEFAULT_CLOUD%22,%22crossSeriesReducer%22:%22REDUCE_NONE%22,%22filter%22:%22metric.type%3D%5C%22pubsub.googleapis.com%2Fsubscription%2Fsent_message_count%5C%22%20resource.type%3D%5C%22pubsub_subscription%5C%22%20resource.label.%5C%22subscription_id%5C%22%3D%5C%22pubsub-subscription-performance-epost-workflow-events%5C%22%20metric.label.%5C%22delivery_type%5C%22%3D%5C%22streaming_pull%5C%22%22,%22groupByFields%22:%5B%5D,%22minAlignmentPeriod%22:%2260s%22,%22perSeriesAligner%22:%22ALIGN_RATE%22%7D%7D,%7B%22plotType%22:%22LINE%22,%22pointConnectionMethod%22:%22GAP_DETECTION%22,%22targetAxis%22:%22Y1%22,%22timeSeriesFilter%22:%7B%22aggregations%22:%5B%7B%22alignmentPeriod%22:%2260s%22,%22crossSeriesReducer%22:%22REDUCE_NONE%22,%22groupByFields%22:%5B%5D,%22perSeriesAligner%22:%22ALIGN_RATE%22%7D%5D,%22apiSource%22:%22DEFAULT_CLOUD%22,%22crossSeriesReducer%22:%22REDUCE_NONE%22,%22filter%22:%22metric.type%3D%5C%22pubsub.googleapis.com%2Fsubscription%2Fpull_ack_message_operation_count%5C%22%20resource.type%3D%5C%22pubsub_subscription%5C%22%20resource.label.%5C%22subscription_id%5C%22%3D%5C%22pubsub-subscription-performance-epost-workflow-events%5C%22%20metric.label.%5C%22response_code%5C%22%3D%5C%22success%5C%22%22,%22groupByFields%22:%5B%5D,%22minAlignmentPeriod%22:%2260s%22,%22perSeriesAligner%22:%22ALIGN_RATE%22%7D%7D,%7B%22plotType%22:%22LINE%22,%22pointConnectionMethod%22:%22GAP_DETECTION%22,%22targetAxis%22:%22Y2%22,%22timeSeriesFilter%22:%7B%22aggregations%22:%5B%7B%22alignmentPeriod%22:%2260s%22,%22crossSeriesReducer%22:%22REDUCE_NONE%22,%22groupByFields%22:%5B%5D,%22perSeriesAligner%22:%22ALIGN_MEAN%22%7D%5D,%22apiSource%22:%22DEFAULT_CLOUD%22,%22crossSeriesReducer%22:%22REDUCE_NONE%22,%22filter%22:%22metric.type%3D%5C%22pubsub.googleapis.com%2Fsubscription%2Fnum_unacked_messages_by_region%5C%22%20resource.type%3D%5C%22pubsub_subscription%5C%22%20resource.label.%5C%22subscription_id%5C%22%3D%5C%22pubsub-subscription-performance-epost-workflow-events%5C%22%20metric.label.%5C%22region%5C%22%3D%5C%22europe-west6%5C%22%22,%22groupByFields%22:%5B%5D,%22minAlignmentPeriod%22:%2260s%22,%22perSeriesAligner%22:%22ALIGN_MEAN%22%7D%7D,%7B%22plotType%22:%22LINE%22,%22pointConnectionMethod%22:%22GAP_DETECTION%22,%22targetAxis%22:%22Y2%22,%22timeSeriesFilter%22:%7B%22aggregations%22:%5B%7B%22alignmentPeriod%22:%2260s%22,%22crossSeriesReducer%22:%22REDUCE_PERCENTILE_99%22,%22groupByFields%22:%5B%5D,%22perSeriesAligner%22:%22ALIGN_DELTA%22%7D%5D,%22apiSource%22:%22DEFAULT_CLOUD%22,%22crossSeriesReducer%22:%22REDUCE_PERCENTILE_99%22,%22filter%22:%22metric.type%3D%5C%22pubsub.googleapis.com%2Fsubscription%2Fack_latencies%5C%22%20resource.type%3D%5C%22pubsub_subscription%5C%22%20resource.label.%5C%22subscription_id%5C%22%3D%5C%22pubsub-subscription-performance-epost-workflow-events%5C%22%20metric.label.%5C%22delivery_type%5C%22%3D%5C%22streaming_pull%5C%22%20resource.label.%5C%22project_id%5C%22%3D%5C%22klara-performance%5C%22%20metadata.system_labels.%5C%22name%5C%22%3D%5C%22pubsub-subscription-performance-epost-workflow-events%5C%22%22,%22groupByFields%22:%5B%5D,%22minAlignmentPeriod%22:%2260s%22,%22perSeriesAligner%22:%22ALIGN_DELTA%22%7D%7D,%7B%22plotType%22:%22LINE%22,%22pointConnectionMethod%22:%22GAP_DETECTION%22,%22targetAxis%22:%22Y1%22,%22timeSeriesFilter%22:%7B%22aggregations%22:%5B%7B%22alignmentPeriod%22:%2260s%22,%22crossSeriesReducer%22:%22REDUCE_NONE%22,%22groupByFields%22:%5B%5D,%22perSeriesAligner%22:%22ALIGN_RATE%22%7D%5D,%22apiSource%22:%22DEFAULT_CLOUD%22,%22crossSeriesReducer%22:%22REDUCE_NONE%22,%22filter%22:%22metric.type%3D%5C%22pubsub.googleapis.com%2Fsubscription%2Fpull_ack_request_count%5C%22%20resource.type%3D%5C%22pubsub_subscription%5C%22%20resource.label.%5C%22subscription_id%5C%22%3D%5C%22pubsub-subscription-performance-epost-workflow-events%5C%22%20resource.label.%5C%22project_id%5C%22%3D%5C%22klara-performance%5C%22%20metadata.system_labels.%5C%22name%5C%22%3D%5C%22pubsub-subscription-performance-epost-workflow-events%5C%22%22,%22groupByFields%22:%5B%5D,%22minAlignmentPeriod%22:%2260s%22,%22perSeriesAligner%22:%22ALIGN_RATE%22%7D%7D,%7B%22plotType%22:%22LINE%22,%22pointConnectionMethod%22:%22GAP_DETECTION%22,%22targetAxis%22:%22Y1%22,%22timeSeriesFilter%22:%7B%22aggregations%22:%5B%7B%22alignmentPeriod%22:%2260s%22,%22crossSeriesReducer%22:%22REDUCE_NONE%22,%22groupByFields%22:%5B%5D,%22perSeriesAligner%22:%22ALIGN_RATE%22%7D%5D,%22apiSource%22:%22DEFAULT_CLOUD%22,%22crossSeriesReducer%22:%22REDUCE_NONE%22,%22filter%22:%22metric.type%3D%5C%22pubsub.googleapis.com%2Fsubscription%2Fmod_ack_deadline_request_count%5C%22%20resource.type%3D%5C%22pubsub_subscription%5C%22%20resource.label.%5C%22subscription_id%5C%22%3D%5C%22pubsub-subscription-performance-epost-workflow-events%5C%22%20resource.label.%5C%22project_id%5C%22%3D%5C%22klara-performance%5C%22%20metadata.system_labels.%5C%22name%5C%22%3D%5C%22pubsub-subscription-performance-epost-workflow-events%5C%22%22,%22groupByFields%22:%5B%5D,%22minAlignmentPeriod%22:%2260s%22,%22perSeriesAligner%22:%22ALIGN_RATE%22%7D%7D%5D,%22options%22:%7B%22mode%22:%22COLOR%22%7D,%22y1Axis%22:%7B%22label%22:%22%22,%22scale%22:%22LINEAR%22%7D,%22y2Axis%22:%7B%22label%22:%22%22,%22scale%22:%22LINEAR%22%7D%7D%7D&amp;project=klara-performance" class="external-link" rel="nofollow">pubsub metric</a></p>

![[48575152168-image-20250711-013836.png]]

![[48575152168-image-20250715-072837.png]]

</td>
</tr>
<tr>
<td><ul>
<li><p>data index (1 pods )</p>
<ul>
<li><p>2 CPU 2GB RAM</p></li>
<li><p>pubsub consumer</p>
<ul>
<li><p>16 executor threads</p></li>
<li><p>message pool size 220</p></li>
<li><p>ack deadline 10s (extend max 30s)</p></li>
</ul></li>
</ul></li>
</ul>
<p><strong>Load</strong>: 17k events/messages</p></td>
<td><p>5mins</p></td>
<td><ul>
<li><p>ack max 80 msgs/s</p></li>
</ul></td>
<td>

![[48575152168-image-20250716-012439.png]]

![[48575152168-image-20250716-014451.png]]

</td>
<td></td>
<td></td>
</tr>
<tr>
<td><ul>
<li><p>data index (1 pods )</p>
<ul>
<li><p>4 CPU 2GB RAM</p></li>
<li><p>pubsub consumer</p>
<ul>
<li><p>16 executor threads</p></li>
<li><p>message pool size 500</p></li>
<li><p>ack deadline 10s (extend max 30s)</p></li>
</ul></li>
</ul></li>
<li><p>serverless workflow (10 pods)</p>
<ul>
<li><p>2 CPU 2GB RAM</p></li>
</ul></li>
</ul>
<p><strong>Load</strong>: <a href="https://cloudlogging.app.goo.gl/vUXxzKJtFsDUWovAA" class="external-link" rel="nofollow">2.4</a> mil events/messages in total (for &gt;100K workflow instances)</p></td>
<td><p>3h30</p></td>
<td><ul>
<li><p>ack 80 msgs/s</p></li>
<li><p>99% percentile 5s</p></li>
</ul></td>
<td><p><a href="https://console.cloud.google.com/monitoring/metrics-explorer;endTime=2025-07-04T10:30:03.417Z;startTime=2025-07-04T07:41:34.560Z?pageState=%7B%22xyChart%22:%7B%22constantLines%22:%5B%5D,%22dataSets%22:%5B%7B%22plotType%22:%22LINE%22,%22pointConnectionMethod%22:%22GAP_DETECTION%22,%22targetAxis%22:%22Y1%22,%22timeSeriesFilter%22:%7B%22aggregations%22:%5B%7B%22alignmentPeriod%22:%2260s%22,%22crossSeriesReducer%22:%22REDUCE_NONE%22,%22groupByFields%22:%5B%5D,%22perSeriesAligner%22:%22ALIGN_RATE%22%7D%5D,%22apiSource%22:%22DEFAULT_CLOUD%22,%22crossSeriesReducer%22:%22REDUCE_NONE%22,%22filter%22:%22metric.type%3D%5C%22pubsub.googleapis.com%2Fsubscription%2Fsent_message_count%5C%22%20resource.type%3D%5C%22pubsub_subscription%5C%22%20resource.label.%5C%22subscription_id%5C%22%3D%5C%22pubsub-subscription-performance-epost-workflow-events%5C%22%20metric.label.%5C%22delivery_type%5C%22%3D%5C%22streaming_pull%5C%22%22,%22groupByFields%22:%5B%5D,%22minAlignmentPeriod%22:%2260s%22,%22perSeriesAligner%22:%22ALIGN_RATE%22%7D%7D,%7B%22plotType%22:%22LINE%22,%22pointConnectionMethod%22:%22GAP_DETECTION%22,%22targetAxis%22:%22Y1%22,%22timeSeriesFilter%22:%7B%22aggregations%22:%5B%7B%22alignmentPeriod%22:%2260s%22,%22crossSeriesReducer%22:%22REDUCE_NONE%22,%22groupByFields%22:%5B%5D,%22perSeriesAligner%22:%22ALIGN_RATE%22%7D%5D,%22apiSource%22:%22DEFAULT_CLOUD%22,%22crossSeriesReducer%22:%22REDUCE_NONE%22,%22filter%22:%22metric.type%3D%5C%22pubsub.googleapis.com%2Fsubscription%2Fpull_ack_message_operation_count%5C%22%20resource.type%3D%5C%22pubsub_subscription%5C%22%20resource.label.%5C%22subscription_id%5C%22%3D%5C%22pubsub-subscription-performance-epost-workflow-events%5C%22%20metric.label.%5C%22response_code%5C%22%3D%5C%22success%5C%22%22,%22groupByFields%22:%5B%5D,%22minAlignmentPeriod%22:%2260s%22,%22perSeriesAligner%22:%22ALIGN_RATE%22%7D%7D,%7B%22plotType%22:%22LINE%22,%22pointConnectionMethod%22:%22GAP_DETECTION%22,%22targetAxis%22:%22Y2%22,%22timeSeriesFilter%22:%7B%22aggregations%22:%5B%7B%22alignmentPeriod%22:%2260s%22,%22crossSeriesReducer%22:%22REDUCE_PERCENTILE_95%22,%22groupByFields%22:%5B%5D,%22perSeriesAligner%22:%22ALIGN_DELTA%22%7D%5D,%22apiSource%22:%22DEFAULT_CLOUD%22,%22crossSeriesReducer%22:%22REDUCE_PERCENTILE_95%22,%22filter%22:%22metric.type%3D%5C%22pubsub.googleapis.com%2Fsubscription%2Fack_latencies%5C%22%20resource.type%3D%5C%22pubsub_subscription%5C%22%20resource.label.%5C%22subscription_id%5C%22%3D%5C%22pubsub-subscription-performance-epost-workflow-events%5C%22%22,%22groupByFields%22:%5B%5D,%22minAlignmentPeriod%22:%2260s%22,%22perSeriesAligner%22:%22ALIGN_DELTA%22%7D%7D,%7B%22plotType%22:%22LINE%22,%22pointConnectionMethod%22:%22GAP_DETECTION%22,%22targetAxis%22:%22Y1%22,%22timeSeriesFilter%22:%7B%22aggregations%22:%5B%7B%22alignmentPeriod%22:%2260s%22,%22crossSeriesReducer%22:%22REDUCE_NONE%22,%22groupByFields%22:%5B%5D,%22perSeriesAligner%22:%22ALIGN_RATE%22%7D%5D,%22apiSource%22:%22DEFAULT_CLOUD%22,%22crossSeriesReducer%22:%22REDUCE_NONE%22,%22filter%22:%22metric.type%3D%5C%22pubsub.googleapis.com%2Fsubscription%2Fpull_ack_request_count%5C%22%20resource.type%3D%5C%22pubsub_subscription%5C%22%20resource.label.%5C%22subscription_id%5C%22%3D%5C%22pubsub-subscription-performance-epost-workflow-events%5C%22%20resource.label.%5C%22project_id%5C%22%3D%5C%22klara-performance%5C%22%20metadata.system_labels.%5C%22name%5C%22%3D%5C%22pubsub-subscription-performance-epost-workflow-events%5C%22%22,%22groupByFields%22:%5B%5D,%22minAlignmentPeriod%22:%2260s%22,%22perSeriesAligner%22:%22ALIGN_RATE%22%7D%7D,%7B%22plotType%22:%22LINE%22,%22pointConnectionMethod%22:%22GAP_DETECTION%22,%22targetAxis%22:%22Y1%22,%22timeSeriesFilter%22:%7B%22aggregations%22:%5B%7B%22alignmentPeriod%22:%2260s%22,%22crossSeriesReducer%22:%22REDUCE_NONE%22,%22groupByFields%22:%5B%5D,%22perSeriesAligner%22:%22ALIGN_MEAN%22%7D%5D,%22apiSource%22:%22DEFAULT_CLOUD%22,%22crossSeriesReducer%22:%22REDUCE_NONE%22,%22filter%22:%22metric.type%3D%5C%22pubsub.googleapis.com%2Fsubscription%2Fnack_requests%5C%22%20resource.type%3D%5C%22pubsub_subscription%5C%22%20resource.label.%5C%22subscription_id%5C%22%3D%5C%22pubsub-subscription-performance-epost-workflow-events%5C%22%20resource.label.%5C%22project_id%5C%22%3D%5C%22klara-performance%5C%22%20metadata.system_labels.%5C%22name%5C%22%3D%5C%22pubsub-subscription-performance-epost-workflow-events%5C%22%22,%22groupByFields%22:%5B%5D,%22minAlignmentPeriod%22:%2260s%22,%22perSeriesAligner%22:%22ALIGN_MEAN%22%7D%7D%5D,%22options%22:%7B%22mode%22:%22COLOR%22%7D,%22y1Axis%22:%7B%22label%22:%22%22,%22scale%22:%22LINEAR%22%7D,%22y2Axis%22:%7B%22label%22:%22%22,%22scale%22:%22LINEAR%22%7D%7D%7D&amp;project=klara-performance" class="external-link" rel="nofollow">pubsub metric</a></p>

![[48575152168-image-20250715-073108.png]]

</td>
<td>

![[48575152168-image-20250715-071949.png]]

</td>
<td></td>
</tr>
</tbody>
</table>

</div>
