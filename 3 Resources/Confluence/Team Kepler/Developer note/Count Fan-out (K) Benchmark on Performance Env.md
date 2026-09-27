---
ai_hash: fe56724a1d2c29d4
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49520541698'
confluence_path: Team Kepler > Developer note
created: 2026-06-20
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- performance
title: Count Fan-out (K) Benchmark on Performance Env
type: source
updated: 2026-06-22
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49520541698/Count+Fan-out+K+Benchmark+on+Performance+Env
---

# Count Fan-out (K) Benchmark on Performance Env

*Confluence source · Team Kepler › Developer note · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49520541698/Count+Fan-out+K+Benchmark+on+Performance+Env) · updated 2026-06-22*

## Environment Specification

**Env:** performance (`klara-performance`, ns `performance`)

**Service:**

- `luz-docs` StatefulSet,

|             |         |         |         |             |
|-------------|---------|---------|---------|-------------|
| CPU req     | CPU lim | RAM req | RAM lim | HPA min–max |
| performance | *1000m* | *15*    | *10Gi*  | *10Gi*      |

- Mongo DB

|                    |         |         |         |         |                       |
|--------------------|---------|---------|---------|---------|-----------------------|
| env (cluster)      | CPU req | CPU lim | RAM req | RAM lim | WT read/write tickets |
| performance (rs03) | 2000m   | 7000m   | 4G      | 8G      | 128 / 128             |

**Tenant:**

- `45b05710-b9d4-4d3e-935e-83c4525369fa` (mongo cluster `luz-mongodb04`)

**Knob under test:**

- `LUZ_DOCS_MATERIALIZE_COUNT_FANOUT_PARTITIONS` (= `luz.docs.materialize.count-fanout-partitions`), called **K**. Changed via `kubectl set env` (forces a rollout — value is `@Inject @ConfigProperty`, read once at bean creation).

- `MAX_THREAD_TASK_WILDFLY=100` (main request worker pool)

- `UPLOAD_FILE_NUMBER_THREAD=100`

- `DEFAULT_ENRICHER_NUMBER_THREAD=18`

- `ENRICHMENT_EVERY_REQUEST_NUMBER_THREAD=4`

- `URGENT_ENRICHER_NUMBER_THREAD` and `URGENT_*_RESPONSE_HANDLING_THREAD`: 10 → **2**

- `GOOGLE_PUB_SUB_THREAD_PULL_NUMBER` absent (others = 10)

## What was measured

- `POST /luz_docs/api/{tenant}/documents/count`with the fixed query body below.

```
{
    "query": {
        "and": [
            {
                "or": [
                    {
                        "term": {
                            "isStored": true
                        }
                    },
                    {
                        "exists": {
                            "field": "folderIds",
                            "type": "array"
                        }
                    }
                ]
            },
            {
                "not": {
                    "terms": {
                        "letterInfo.mediaType": [
                            "application/vnd.ch.klara.epost.smartletter.draft.v1+json",
                            "application/vnd.ch.klara.epost.smartletter.template.v1+json",
                            "application/vnd.ch.klara.epost.smartletter.receipt.v1+json"
                        ]
                    }
                }
            }
        ]
    }
}
```

Data description:

- For each doc-count **size** (cumulative, +120 000/level, 120k→960k) and each **K**.

- CPU/mem of the `luz-docs` container sampled (`kubectl top --containers`) during the calls.

- Client cap: `curl --max-time 120` — any true latency \>120 s is **censored at 120**.

- This query matches every seeded doc, so `totalRecordCount` == collection size at every level — confirming results and that there is no count cap on the materialized path.

Indexes present:

- `idx_isPublic_updatedDate`,

- `idx_effectiveSecurityClassCodes_updatedDate`,

- `idx_folderNames`,

- `idx_isPublic_shard`,

- `idx_effectiveSecurityClassCodes_shard`,

- `idx_shard`.

![[image-20260622-020314.png]]

### Every measured value is a *cold, live* count

**No measured run was ever served from cache:** the lowest latency anywhere is 0.85 s (a 120k live count, not the sub-ms a cache hit returns), and every cell shows its 5 runs clustered at the *same* live cost (e.g. 720k/K=8 = 11.1–11.7 s × 5, never instant).

So this sweep measures **raw parallel-count compute cost** — exactly what you want for choosing K.

### Results — latency (seconds, mean of 5 runs/cell)

|  |  |  |  |  |  |  |  |  |  |
|----|----|----|----|----|----|----|----|----|----|
| **docs** | **K=1** | **K=2** | **K=4** | **K=6** | **K=8** | **K=12** | **K=16** | **K=20** | **K=24** |
| **120 000** | 1.93 | 1.41 | 1.05 | 0.93 | 0.92 | 0.89 | 0.89 | **0.88** | 0.92 |
| **240 000** | 4.43 | 2.19 | 1.53 | 1.20 | **1.06** | 1.09 | 1.12 | 1.12 | 1.10 |
| **360 000** | 16.56 | 8.25 | 3.95 | 3.02 | **2.64** | 2.80 | 2.89 | 2.86 | 2.89 |
| **480 000** | 35.12 | 17.50 | 9.58 | 6.67 | **5.75** | 5.75 | 6.21 | 6.24 | 6.17 |
| **600 000** | 53.58 | 27.69 | 14.47 | 10.49 | **8.76** | 8.81 | 9.33 | 9.40 | 9.19 |
| **720 000** | 72.93 | 37.54 | 19.43 | 13.79 | **11.49** | 11.91 | 12.42 | 12.47 | 12.21 |
| **840 000** | 120.0† | 89.30 | **60.77** | 90.05 | 66.21 | 84.60 | 85.45 | 78.32 | 83.94 |
| **960 000** | 120.0† | 120.0† | 117.83 | 116.66 | **116.47** | 116.69 | 118.85 | 120.0† | 118.02 |

† censored at the 120 s curl cap (true latency ≥ 120 s). **Bold** = best K at that size. The whole 960k row sits within ~3 s at ~116–120 s — **K makes no measurable difference** once the working set is disk-bound.

![[image-20260620-115723.png]]

Chart (log-scale Y):

- the three regimes are visible at a glance — greens (120k–240k) flat near the floor (K barely matters);

- blues (360k–720k) slope down to the K=8 knee then flatten (~6× win);

- reds (840k–960k) pegged against the 120 s ceiling with no K dependence (disk-bound wall).

### Results — peak CPU/mem (`luz-docs` container, sampled during calls)

CPU is sampled with single `kubectl top` snapshots, so it is **noisy cell-to-cell** — but the per-level *peak* tells the whole story:

|             |              |               |                                   |
|-------------|--------------|---------------|-----------------------------------|
| docs        | peak CPU (m) | peak mem (Mi) | regime                            |
| 120 000     | ~4 200       | ~1 370        | CPU-bound (cache-resident)        |
| 240 000     | ~4 300       | ~1 370        | CPU-bound                         |
| 360 000     | ~4 350       | ~1 500        | CPU-bound                         |
| 480 000     | ~1 850       | ~1 270        | CPU-bound (sampling missed peak)  |
| 600 000     | ~4 290       | ~1 280        | CPU-bound                         |
| 720 000     | ~4 420       | ~1 340        | CPU-bound (last healthy level)    |
| **840 000** | **~225**     | **~800**      | **disk/IO-bound — CPU collapses** |
| **960 000** | **~99**      | **~850**      | **disk/IO-bound**                 |

Through 720k the pod saturates **~4 cores** regardless of K (fan-out spreads a fixed amount of in-cache scan work; total work ≈ constant, peak CPU flat once K≥2). Memory stays flat ~1.3 GiB — fan-out does **not** inflate heap, and there is never memory pressure (limit 10 Gi).

![[image-20260620-115804.png]]

Chart (dual axis):

- Peak CPU (orange, left) tracks ~4 cores through 720k then **falls off a cliff** to \<250 m at 840k+ — the pod has gone idle, blocked on disk reads (disk-I/O-bound, not CPU-bound).

- Peak memory (blue, right) stays flat ~0.8–1.5 GiB throughout — never the bottleneck.

- The 480k CPU dip is a single-snapshot sampling miss, not a real trough.

### Findings

1.  **Fan-out is a large win on the cold path — up to ~720k.**

    1.  K=1→K=8 cuts mean latency ~2.1× (120k), ~4.2× (240k), and **~6.3× (360k–720k)**.

2.  **Knee at K≈8, flat/slightly-worse to K=24.**

    1.  Across 240k–720k the optimum is consistently **K=8**; K=6 is within noise of it; K≥12 is *marginally worse*.

    2.  At 120k the knee is earlier (~K=6) since the job is already \<1 s.

3.  **Hard scaling wall at ~840k — fan-out stops working.**

    1.  Between 720k and 840k latency jumps **11.5 s → 60–90 s (≈6×)** and the clean K-curve **disappears**

    2.  At 960k every K converges to ~117–120 s.

4.  **No resource downside to K itself.** Where the work fits cache, CPU peaks ~4 cores (well under the 15-core limit) and memory is flat ~1.3 GiB at every K. K does not trade memory for speed.

#### Why the cliff: O(N) with a cache-miss penalty

The count is fundamentally **O(matched docs)** — it walks every matching `_shard` index entry (fan-out K just splits that walk across K threads, dividing wall-clock by a constant, never changing the slope). But the *per-doc* cost is not constant: it depends on whether the index page is already in RAM.

|                         |                |        |
|-------------------------|----------------|--------|
| Where the index page is | Time per entry | ratio  |
| WiredTiger cache (RAM)  | ~100 ns        | 1×     |
| Disk (SSD), cache miss  | ~100 µs        | ~1000× |

While the scanned index fits in WiredTiger cache, every seek is a RAM hit → ~16 µs/doc effective → shallow line. Once the working set exceeds cache, pages get evicted and re-read from disk (thrashing) → per-doc cost climbs toward the disk number → the line cliffs. Same O(N), constant multiplied by the cache-miss penalty.

- 720k/K=8 = 11.5 s → ~16 µs/doc (cache-resident). Extrapolated, 960k would be ~17 s.

- 960k/K=8 = 116 s → ~125 µs/doc (~8× higher) → the ~100 s gap *is* the disk penalty.

The CPU collapse (4 400 m → \<250 m) is the signature: cache-resident = cores busy comparing in-RAM keys (CPU-bound); disk-bound = cores idle waiting on I/O. It also explains why K stops helping — K threads issue reads into the *same* disk queue and contend instead of parallelizing.

![[image-20260620-115951.png]]

- The fix is not to change O(N) but to keep per-doc cost in the cheap RAM — i.e. keep the working set ≤ cache: more mongo RAM / bigger node, a leaner covering index (more entries per cached page), or sharding (each node caches only its slice).

- *Any of these keeps the line on the shallow green path instead of the cliff.*

- *To actually break O(N), counts must be precomputed (maintained* `$inc`*/*`$dec` *counters per fixed dimension — total / per-folder / per-security-class), giving O(1) reads at the cost of write-time upkeep.*

### Recommendation

- **Keep K = 8**

- **K is a cold-path-only lever and only helps while the scanned set fits MongoDB's cache**

%% ai-graph-start %%

**Related notes:**
- [[Shard count fan-out most of the win is at K=4, diminishing returns after]]
- [[Test parallelize executor]]
- [[Dev benchmark _shard count fan-out ~1.8x, diminishing past K=12; local port-forward hid the gain]]
- [[eArchive Performance measurement & scalability assessment at 800000 documents]]
- [[Production security count is already COUNT_SCAN (covered); benchmark query's FETCH is inherent (multikey+$or+$nin)]]

%% ai-graph-end %%