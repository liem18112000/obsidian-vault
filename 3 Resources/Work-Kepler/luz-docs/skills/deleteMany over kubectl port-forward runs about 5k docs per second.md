---
ai_hash: 9f1e7ee5610868c6
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-11
entities: []
source: session 2026-06-11 (first earchive-data-clean run)
status: seedling
tags:
- luz
- mongodb
- performance
- earchive
title: deleteMany over kubectl port-forward runs about 5k docs per second
type: observation
---

# deleteMany over kubectl port-forward runs about 5k docs per second

Calibration point for estimating tenant-clean durations on the Luz dev MongoDB clusters: `deleteMany({})` of 128,202 eArchive documents (rich ~production-shaped docs with `documentTextContent` lorem paragraphs) through a `kubectl port-forward` to the replica primary took 24.3 s — roughly 5,000 docs/s. The 88-folder collection cleared in 0.2 s.

So a full canary-sized wipe is ~25 s end to end; budget minutes only for multi-million-doc tenants. Measured 2026-06-11 on tenant b260a45a (luz-mongodb03-cluster-rs-0) via the earchive-data-clean skill.

## Related

- [[Destructive Luz skills use a preview-first CONFIRM gate]]
- [[eArchive dev skills are self-contained copies, not shared helpers]]

%% ai-graph-start %%

**Related notes:**
- [[Performance Mongo is a mongos-routed sharded cluster — truncate via the tenant's shard primary, not mongos]]
- [[eArchive dev skills are self-contained copies, not shared helpers]]
- [[eArchive count baseline latency on dev ~80s for 128k docs (fan-out off)]]
- [[luz-docs documentscount is ~130s on an 800k tenant — the 16-shard fan-out, not counting, is the bottleneck]]
- [[Destructive Luz skills use a preview-first CONFIRM gate]]

%% ai-graph-end %%