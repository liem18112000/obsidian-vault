---
ai_hash: c46378386781e34b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-19
entities:
- eArchive
- latency
- dev
- 128k docs
- fan-out
- Dev tenant d0783310-d67f-4ab7-9aab-dcaef3f17f48
- 8-12 folders
- POST /{tenant}/documents/count
- deployed image cf8687633
- branch 42830e38b
- ParallelizeCount
- K=6
- 4s target
- count_document.feature
- scan-bound
- LUZ-154613-f-cache-count
- count caching
- config provisioned ahead of code
- configmap luz-docs-env-configmap-dg5bm564h8
- LUZ_DOCS_MATERIALIZE_COUNT_FANOUT_PARTITIONS
- LUZ_DOCS_TENANTS_USE_MATERIALIZED
- fan-out code
- 5e37c1353
- df3cb3bbc
- git branch --contains <sha>
- git ls-tree -r <sha>
- IT rest client
- DEFAULT_TIMEOUT
- api/client/luz_docs_rest_client.py
- count_documents
- ReadTimeout
- per-request timeout
- kubectl port-forward service/api-forwarder
- sustained load
- RemoteDisconnected
- localhost
- 0.0.0.0
- sandbox
- luz-docs parallelized count undercounts documents missing _shard
- luz_docs documentscount is scan-bound and cannot reach sub-second at 128k
source: session 2026-06-19
status: seedling
tags:
- luz-docs
- performance
- count
- dev
- measurement
title: 'eArchive count baseline latency on dev: ~80s for 128k docs (fan-out off)'
type: observation
---

# eArchive count baseline latency on dev: ~80s for 128k docs (fan-out off)

Dev tenant `d0783310-d67f-4ab7-9aab-dcaef3f17f48` (128,000 docs, 8-12 folders), 2026-06-19. `POST /{tenant}/documents/count`:

| Build | Result |
|---|---|
| deployed image `cf8687633` (no fan-out code) | 77.9 / 73.2 / 92.8 s — **~80s**, all 200, total=128000 |
| branch `42830e38b` (ParallelizeCount, fan-out K=6) | cold 94.7 s; warm 14.3 / 21.7 / 15.8 / 20.9 s — **~18s warm, ~4x** |

**~20x over the 4s target** the IT `count_document.feature` asserts, before and after fan-out. ~18s warm says the count is scan-bound; the 4s target likely needs the sibling branch `LUZ-154613-f-cache-count` (count caching), not fan-out alone.

**Why the baseline had no fan-out — config provisioned ahead of code.** Configmap `luz-docs-env-configmap-dg5bm564h8` already carried `LUZ_DOCS_MATERIALIZE_COUNT_FANOUT_PARTITIONS: "6"` and `LUZ_DOCS_TENANTS_USE_MATERIALIZED: "*"`, but the class reading them was not in the deployed commit (fan-out landed later, in `5e37c1353` + `df3cb3bbc`).
**Lesson:** `git branch --contains <sha>` only proves the sha is an ANCESTOR of the branch, not that it includes a later feature — inspect the tree at that exact sha (`git ls-tree -r <sha>`) to check whether a deployed image has a feature.

**Two gotchas for measuring this endpoint:**
- The IT rest client's `DEFAULT_TIMEOUT = 60` (`api/client/luz_docs_rest_client.py`) is shorter than the count latency, so `count_documents` raises ReadTimeout before it can record a timing. Raise the per-request timeout.
- `kubectl port-forward service/api-forwarder` drops under sustained load (10 sequential ~80s counts killed it with RemoteDisconnected). Restart before long perf runs; bind to localhost only (0.0.0.0 is blocked by the sandbox).

## Related

- [[luz-docs parallelized count undercounts documents missing _shard]]
- [[luz_docs documentscount is scan-bound and cannot reach sub-second at 128k]]

%% ai-graph-start %%

**Related notes:**
- [[luz-docs documentscount is ~130s on an 800k tenant — the 16-shard fan-out, not counting, is the bottleneck]]
- [[eArchive 800k bottleneck is view-controller not K]]
- [[luz_docs documentscount is scan-bound and cannot reach sub-second at 128k]]
- [[eArchive request flow and log correlation (perf)]]
- [[Shard count fan-out most of the win is at K=4, diminishing returns after]]

**Relations:**
- eArchive — *has* — baseline latency
- baseline latency — *on* — dev
- baseline latency — *for* — 128k docs
- baseline latency — *is* — ~80s (fan-out off)
- Dev tenant d0783310-d67f-4ab7-9aab-dcaef3f17f48 — *has* — 128k docs
- Dev tenant d0783310-d67f-4ab7-9aab-dcaef3f17f48 — *has* — 8-12 folders
- POST /{tenant}/documents/count — *is* — endpoint
- deployed image cf8687633 — *has* — no fan-out code
- deployed image cf8687633 — *measured latency* — ~80s
- branch 42830e38b — *implements* — ParallelizeCount
- branch 42830e38b — *uses* — fan-out K=6
- branch 42830e38b — *measured latency* — ~18s warm
- ~18s warm — *is* — ~4x faster than ~80s
- ~80s — *is* — ~20x over the 4s target
- ~18s warm — *is* — ~20x over the 4s target
- 4s target — *asserted by* — count_document.feature
- ~18s warm — *indicates* — scan-bound
- 4s target — *needs* — LUZ-154613-f-cache-count
- LUZ-154613-f-cache-count — *is* — count caching
- baseline — *had* — no fan-out
- no fan-out — *due to* — config provisioned ahead of code
- configmap luz-docs-env-configmap-dg5bm564h8 — *carried* — LUZ_DOCS_MATERIALIZE_COUNT_FANOUT_PARTITIONS
- configmap luz-docs-env-configmap-dg5bm564h8 — *carried* — LUZ_DOCS_TENANTS_USE_MATERIALIZED
- fan-out code — *landed in* — 5e37c1353
- fan-out code — *landed in* — df3cb3bbc
- git branch --contains <sha> — *proves* — sha is an ANCESTOR
- git ls-tree -r <sha> — *inspects* — tree at that exact sha
- git ls-tree -r <sha> — *checks deployed image for* — feature
- IT rest client — *has* — DEFAULT_TIMEOUT
- DEFAULT_TIMEOUT — *is* — 60
- DEFAULT_TIMEOUT — *in* — api/client/luz_docs_rest_client.py
- DEFAULT_TIMEOUT — *is* — shorter than count latency
- count_documents — *raises* — ReadTimeout
- per-request timeout — *should be* — raised
- kubectl port-forward service/api-forwarder — *drops under* — sustained load
- kubectl port-forward service/api-forwarder — *killed with* — RemoteDisconnected
- kubectl port-forward service/api-forwarder — *should be* — restarted
- kubectl port-forward service/api-forwarder — *should bind to* — localhost only
- 0.0.0.0 — *is* — blocked by sandbox
- luz-docs parallelized count undercounts documents missing _shard — *is* — related
- luz_docs documentscount is scan-bound and cannot reach sub-second at 128k — *is* — related

%% ai-graph-end %%