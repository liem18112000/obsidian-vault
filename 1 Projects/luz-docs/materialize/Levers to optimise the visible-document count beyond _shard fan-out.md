---
ai_hash: e35ac2b476aba8de
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-17
entities:
- Visible Document Count
- Shard Fan-out
- Optimization Levers
- Caching
- Total Count
- Tenant
- Code Set
- luz_cache
- Cache Key (sorted code-set+tenant)
- Cascade Hooks
- Cache Miss
- Bitmap Union
- Roaring Exact Bitmap
- HLL Approximate Bitmap
- O(doc×code) Amplification
- Popcount
- Per-Code Bitmaps
- Doc-ID to Int Map
- Covered COUNT_SCAN
- FETCH Operation
- Explain Plan
- Gateway Query
- Extra Predicates
- folderIds exists Predicate
- mediaType not-in Predicate
- Database Index
- Keys-Only Count
- Connection Pool
- K Concurrent Sub-Counts
- jsonstore
- Mongo Connections
- Connection Pool Size
- Read-Replica Routing
- Heavy Sub-Counts
- Read-Preference Knob
- Frozen Gateway
- Bigger Pod
- More Cores
- Fan-out Ceiling
- Approximate Totals
- Product Lever
- User Interface
- '''Has More'' Indicator'
- '''999+'' Cap'
- Requirement
- Action Order
- 'Step 3: Explain Plan'
- 'Step 1: Caching'
- 'Step 4: Connection Pool'
- 'Step 2: Bitmap Union'
- 'Step 7: Approximate Totals'
- Tail Optimization
- 'Related Note: Shard Count Fan-out Benchmark'
- 'Related Note: Bitmap Union for Visible Document Count'
source: LUZ-154613 session 2026-06-17
status: seedling
tags:
- luz-docs
- performance
- count
- caching
- roaring
- index
title: Levers to optimise the visible-document count beyond _shard fan-out
type: argument
---

# Levers to optimise the visible-document count beyond _shard fan-out

Levers to cut the materialised visible-document count time beyond the _shard fan-out (which capped at ~1.8x, ceiling = cores). Ranked:

1. CACHE the count — the total for (tenant, code-set) only changes on create/delete/security-change. Cache in luz_cache keyed by sorted code-set+tenant, invalidate in existing cascade hooks. Repeat counts ~0ms; fan-out only helps a cache miss. Cheap, days.
2. BITMAP UNION (Roaring exact / HLL approx) — removes the O(doc×code) amplification entirely; count = popcount(OR of per-code bitmaps), sub-ms regardless of K. Biggest absolute win; weeks (maintain per-code bitmaps on writes + doc-id↔int map).
3. CONFIRM covered COUNT_SCAN not FETCH — 24s cold / 4s warm suggests sub-counts FETCH docs. Run explain on the actual gateway query; extra predicates (folderIds exists, mediaType not-in) may force FETCH/$expr. Add an index covering them beside _shard → keys-only count. Cheapest high-value check, do FIRST.
4. CONNECTION POOL >= K — K concurrent sub-counts need K jsonstore/Mongo connections; if pool<K they queue and fan-out silently serialises (likely why K=8≈K=4). Verify pool scales with K.
5. READ-REPLICA routing for heavy sub-counts — needs a read-preference knob on the frozen gateway.
6. Bigger pod / more cores — raises the fan-out ceiling, linear/finite.
7. DON'T compute exact totals (product lever) — UI usually needs 'has more', cap at '999+' → instant. Biggest win if requirement allows.

Act order: (3) explain → (1) cache → (4) pool → then (2) bitmap vs (7) approximate for the tail.

## Related

- [[Dev benchmark _shard count fan-out ~1.8x, diminishing past K=12; local port-forward hid the gain]]
- [[Visible-document count as cardinality of a bitmap union]]

%% ai-graph-start %%

**Related notes:**
- [[Production security count is already COUNT_SCAN (covered); benchmark query's FETCH is inherent (multikey+$or+$nin)]]
- [[Shard count fan-out most of the win is at K=4, diminishing returns after]]
- [[Visible-document count as cardinality of a bitmap union]]
- [[Frozen JsonStore gateway makes _id-range count fan-out a dead end — pivot to bitmapHLL]]
- [[Divide-and-Conquer Visible-Document Count]]

**Relations:**
- Optimization Levers — *optimises* — Visible Document Count
- Optimization Levers — *beyond* — Shard Fan-out
- Caching — *is a type of* — Optimization Levers
- Caching — *optimises* — Visible Document Count
- Caching — *uses* — luz_cache
- luz_cache — *keyed by* — Cache Key (sorted code-set+tenant)
- Caching — *invalidates via* — Cascade Hooks
- Shard Fan-out — *helps with* — Cache Miss
- Total Count — *is for* — Tenant
- Total Count — *is for* — Code Set
- Bitmap Union — *is a type of* — Optimization Levers
- Bitmap Union — *optimises* — Visible Document Count
- Bitmap Union — *removes* — O(doc×code) Amplification
- Bitmap Union — *includes* — Roaring Exact Bitmap
- Bitmap Union — *includes* — HLL Approximate Bitmap
- Visible Document Count — *calculated by* — Popcount
- Popcount — *operates on* — Per-Code Bitmaps
- Per-Code Bitmaps — *requires* — Doc-ID to Int Map
- Covered COUNT_SCAN — *is a type of* — Optimization Levers
- Covered COUNT_SCAN — *is preferred over* — FETCH Operation
- Explain Plan — *applies to* — Gateway Query
- Extra Predicates — *can force* — FETCH Operation
- Extra Predicates — *includes* — folderIds exists Predicate
- Extra Predicates — *includes* — mediaType not-in Predicate
- Database Index — *covers* — Extra Predicates
- Database Index — *enables* — Keys-Only Count
- Connection Pool — *is a type of* — Optimization Levers
- Connection Pool — *optimises* — Visible Document Count
- Connection Pool — *supports* — K Concurrent Sub-Counts
- K Concurrent Sub-Counts — *needs* — jsonstore
- K Concurrent Sub-Counts — *needs* — Mongo Connections
- Connection Pool Size — *less than* — K Concurrent Sub-Counts
- Connection Pool Size — *causes* — Shard Fan-out
- Connection Pool Size — *should scale with* — K Concurrent Sub-Counts
- Read-Replica Routing — *is a type of* — Optimization Levers
- Read-Replica Routing — *optimises* — Visible Document Count
- Read-Replica Routing — *optimises* — Heavy Sub-Counts
- Read-Replica Routing — *requires* — Read-Preference Knob
- Read-Preference Knob — *on* — Frozen Gateway
- Bigger Pod — *is a type of* — Optimization Levers
- Bigger Pod — *optimises* — Visible Document Count
- Bigger Pod — *raises* — Fan-out Ceiling
- More Cores — *is a type of* — Optimization Levers
- More Cores — *optimises* — Visible Document Count
- More Cores — *raises* — Fan-out Ceiling
- Approximate Totals — *is a type of* — Optimization Levers
- Approximate Totals — *optimises* — Visible Document Count
- Approximate Totals — *is a* — Product Lever
- Approximate Totals — *used by* — User Interface
- User Interface — *needs* — 'Has More' Indicator
- Approximate Totals — *involves* — '999+' Cap
- Approximate Totals — *is instant if* — Requirement
- Action Order — *starts with* — Step 3: Explain Plan
- Action Order — *then* — Step 1: Caching
- Action Order — *then* — Step 4: Connection Pool
- Action Order — *then considers* — Step 2: Bitmap Union
- Action Order — *then considers* — Step 7: Approximate Totals
- Step 2: Bitmap Union — *is for* — Tail Optimization
- Step 7: Approximate Totals — *is for* — Tail Optimization
- Optimization Levers — *related to* — Related Note: Shard Count Fan-out Benchmark
- Optimization Levers — *related to* — Related Note: Bitmap Union for Visible Document Count
- Visible Document Count — *is* — Cardinality of a Bitmap Union

%% ai-graph-end %%