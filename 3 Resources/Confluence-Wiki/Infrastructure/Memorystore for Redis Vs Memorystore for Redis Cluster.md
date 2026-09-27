---
title: "Memorystore for Redis Vs Memorystore for Redis Cluster"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48326967433/Memorystore+for+Redis+Vs+Memorystore+for+Redis+Cluster
space: "LUZ"
topic: infra
relevance: 0.714
depth: 2.45
updated: 2025-02-11
attachments: 0
tags:
  - confluence
  - infra
  - space/luz
---

# Memorystore for Redis Vs Memorystore for Redis Cluster

> [!info] Imported from Confluence
> Space **LUZ** · updated 2025-02-11 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48326967433/Memorystore+for+Redis+Vs+Memorystore+for+Redis+Cluster)
> Relevance 0.714 · topic `infra`

## **Key Differences at a Glance**

<div>

|  |  |  |
|----|----|----|
| Feature | **Memorystore for Redis (Standard & Basic)** | **Memorystore for Redis Cluster** |
| **Scaling** | Vertical (increase node size) | Horizontal (add more shards) |
| **Max Data Size** | 300GB | **10+ TB** (scalable with shards) |
| **Performance** | Limited by single-node CPU | **Multi-node, multi-threaded** (higher throughput) |
| **Failover (HA)** | Standard tier supports 1 replica | **Supports multiple replicas per shard** |
| **Read Scaling** | No, only 1 replica for HA | **Yes, clients can read from replicas** |
| **Best for** | **Small-medium workloads** | **Large-scale, high-traffic apps** |

</div>

------------------------------------------------------------------------

### **Decision**

- ✅ **Use Memorystore for Redis (Standard)** if you have a **single-node workload**, require basic caching, and do not need complex scalability.

- ✅ **Use Memorystore for Redis Cluster** if you need **high performance, automatic sharding, multi-node scaling, and read replicas for better throughput**.

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th><p>Feature</p></th>
<th><p>Memorystore for Redis (Standard Tier)</p></th>
<th><p>Memorystore for Redis Cluster</p></th>
</tr>
&#10;<tr>
<td><p><strong>High Availability</strong></p></td>
<td><p>99.9%</p>
<p>Provides <strong>failover</strong> with <strong>replica node</strong> in a different zone. Automatic failover in case of primary failure.</p></td>
<td><p>High availability across <strong>shards</strong>, but failover happens at the shard level.</p>
<p>To enable high availability for your instance, you must provision at least 1 replica node for every shard.</p>
<p>The cheapest node type redis-shared-core-nano does not offer SLA.</p>
<p><code>https://cloud.google.com/memorystore/docs/cluster/cluster-node-specification?utm_source=chatgpt.com#node_characteristics</code></p></td>
</tr>
<tr>
<td><p><strong>Persistence</strong></p></td>
<td><p><strong>RDB (Redis Database)</strong> persistence support. Only in-memory.</p>
<p><a href="https://cloud.google.com/memorystore/docs/redis/memorystore-for-redis-overview#differences_between_managed_and_open_source_redis" class="external-link" rel="nofollow">no support AOF</a></p></td>
<td><p>Support 2 modes: AOF and RDB</p>
<ul>
<li><p>THE AOF (Append Only File) persistence mode prioritizes data durability. It durably stores data by recording every write command to a log file called the AOF file</p></li>
<li><p>The RDB (Redis database) persistence feature protects your data by saving snapshots of your data on durable storage. You choose the frequency of these snapshots by selecting a snapshot interval ranging from a minimum of 1 hour to a maximum of 24 hours</p></li>
</ul>
<p>AOF persistence pricing: Zurich(europe-west6) $0.00076652</p></td>
</tr>
<tr>
<td><p><strong>Backup</strong></p></td>
<td><p>yes, automatic backup by enabling the snapshot in interval time (minimum 1h) → data loss still happens in 1 hour gap</p>
<p>or manual import and export feature</p></td>
<td><p>Supports, either automation or on demand.</p>
<p>Pricing: Zurich(europe-west6) $0.00013889/h</p></td>
</tr>
<tr>
<td><p><strong>Performance</strong></p></td>
<td><p>minimum network throughput 10 Gbps</p>
<p>Enable <strong>read replica</strong> feature can increase the throughput for read operation</p></td>
<td><p>Using the OSS memtier benchmarking tool in the <code>us-central1</code> region yielded 120,000 - 130,000 operations per second per 2 vCPU node (<code>redis-standard-small</code> and <code>redis-highmem-medium</code>) with microseconds latency and 1KiB data size.</p></td>
</tr>
<tr>
<td><p><strong>Cost</strong></p></td>
<td><p>Instance <strong>without read replica enable</strong></p>
<p>Capacity <strong>1 GB</strong> $0.08371 per GB/hour → <strong>$71.54 per month</strong></p>
<p>Capacity <strong>5 GB</strong> $0.07063 per GB/hour (0 read replica) → <strong>$255.50 per month</strong></p>
<p>Instance <strong>with read replica enable</strong></p>
<ul>
<li><p>Capacity <strong>5 GB</strong> $0.07063 per GB/hour</p></li>
<li><p>1 Read replica: $0.03008 per GB/hour</p></li>
</ul>
<p>→ <strong>$270.10 per month</strong></p></td>
<td><p>Higher cost due to <strong>multi-node deployment</strong> and <strong>network overhead</strong> but scales better for large workloads.</p>
<p>Suppose we provision a 1 shard instance with one replica per shard, using the redis-standard-small - 6.5 GB node type. You want to deploy this instance in Zurich(europe-west6), where the price per-node-per-hour is $0.1993.</p>
<p>The hourly cost is (1 shard + 1 replica node) * $0.1993 = $0.3986 per hour.</p>
<p>Total cost of 1 shard, 1 replica, node type is redis-standard-small(2 CPUs, 6.5 GB), with AOF persistence and backup</p>
<p>= $0.00076652 + $0.00013889 + $0.3986</p>
<p>= $0.39950541 per hour</p>
<p>= $287.6438952 per month</p></td>
</tr>
</tbody>
</table>

</div>

## Final decision:

Redis cluster for Prod and the others use Redis standard. And for Performance, we should test with standard.
