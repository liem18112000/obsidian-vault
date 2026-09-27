---
ai_hash: dee9d7e1f39797f5
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-03
entities: []
source: session 2026-09-03 (perf-cluster 2.2M seed)
status: seedling
tags:
- data-seeding
- idempotency
- mongodb
- resume
- ops
- earchive
title: Resume a large append seed by recounting to a target
type: howto
---

# Resume a large append seed by recounting to a target

To resume a large data-generation job after a mid-run crash **without re-truncating** the rows already written, drive it toward a target count instead of a fixed batch:

1. Run the generator in **APPEND** mode — skip truncate, reuse existing parent rows (folders/dirs), stamp the same enrichment fields so new rows match the old ones.
2. Each iteration, read the current row count with a cheap read-only query and set `DOC_COUNT = TARGET - current_count`.
3. Loop until `current_count >= TARGET`.

This makes the seed **idempotent and self-healing**: a port-forward drop or OOM just means the next iteration recounts and tops up the remainder — it never overshoots the target and never wipes prior progress. Pairs directly with [[kubectl port-forward drops after ~1 hour on GKE]], which is *why* a long seed needs the loop in the first place.

**Concrete (earchive-data-prepare, klara-performance, 2026-09-03):** `APPEND=true FOLDER_COUNT=0` preserves the existing docs + 10 folders (`APPEND mode — skipping truncate` / `reusing existing folders: 10`); count via the `mongodb` driver `countDocuments()` over a dedicated port-forward using `readPreference=primaryPreferred&directConnection=true` (so it reads even if the primary has failed over), creds = tenantId for user/pass/authSource. A separate count port-forward on its own local port avoids colliding with the generators forward.

## Related

- [[kubectl port-forward drops after ~1 hour on GKE]]

%% ai-graph-start %%

**Related notes:**
- [[Resume a partial earchive seed with APPEND instead of re-truncating]]
- [[Recount before every reach-a-target retry — crashed runs insert silently]]
- [[Long real-API seed aborts on socket hang up unless port-forward reconnects]]
- [[kubectl port-forward drops after ~1 hour on GKE]]
- [[earchive-data-prepare wrapper exits 0 even when the generator dies mid-run (verify the log footer)]]

%% ai-graph-end %%