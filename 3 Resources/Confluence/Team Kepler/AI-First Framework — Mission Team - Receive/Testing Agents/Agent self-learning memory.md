---
title: "Agent self-learning memory"
created: 2026-09-07
updated: 2026-09-07
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49732091905/Agent+self-learning+memory
confluence_id: "49732091905"
confluence_path: "Team Kepler > AI-First Framework — Mission Team: Receive > Testing Agents > Agent Loop 2 - Self-learning"
tags: [confluence, ai-agents]
---

# Agent self-learning memory

*Confluence source · Team Kepler › AI-First Framework — Mission Team: Receive › Testing Agents › Agent Loop 2 - Self-learning · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49732091905/Agent+self-learning+memory) · updated 2026-09-07*

## Goal

After **any** Testing-Agent pipeline step, the agent should **distil the valuable, non-obvious things it just learned into the shared GCS agent memory** as durable, cited, recallable nodes — so **every future run of any agent** starts smarter.

Today only `refine` writes durable knowledge (Q&A → `Insight`); `gather` writes crawl notes, and `define`/`implement` write **nothing** to memory.

This proposal generalizes "collect insight" into a cross-step **self-learning** capability.

## Principle

*Nothing ungrounded, nothing unbounded, human owns the gate.*

A "lesson" must be **cited** and is **vetoable**; capture is **best-effort** and never breaks the pipeline; recall is **de-biased** (a bad lesson must not poison future runs).

## Data model

![[image-20260907-073052.png]]

## What counts as a "valuable lesson"

Prioritize **high-signal, non-obvious, reusable** items; skip the obvious/derivable:

- **Corrections** — the human overrode the agent (strongest signal; `confidence=high`).

- **Corrected facts about the feature-under-test** (e.g. tenant/behaviour facts).

- **Deployed-agent limitations / gotchas** surfaced during the run.

- **Cross-run contradictions** — a new answer contradicts a prior lesson (→ `supersede`).

- **Recurring declared gaps.** Explicitly **not**: raw tool output, secrets, one-off values, anything already in the pack/codegraph.

## Recall — closing the loop

Lessons are worthless unless future runs see them. Recall already half-works (`INSIGHT` nodes are in the index and G0 self-seed surfaces them insights-first). Add:

- **Prioritize** `LESSON`**/**`CORRECTION` **kinds** in self-seed and in the refine/define pack preamble ("Prior lessons") so the agent is reminded before it repeats a mistake.

- **Cross-run scoping:** lessons are `scope=shared` → recalled at **gather/expansion** time (not inside a run-scoped refine pack). Confirm interaction with (`load_pack` filters by `run_id`): lessons influence seeding/hypothesis and are surfaced, they are not re-interrogated as pack nodes.

## Capture step

A shared, dependency-light helper — new package `common/learn/` (mirrors `common/interrogate/`):

```
capture_lessons(bank, *, context_id, run_id, step, artifacts, distiller=None) -> list[Insight]
```

### Flow (bounded, best-effort, degrade-to-noop):

1.  **Collect candidate signals** from the step's own artifacts — cheap, no LLM:

    - *refine:* human answers that **overrode** the agent recommendation (→ `correction`), declared gaps, `new_seed` re-grounding events.

    - *define/implement:* scope corrections the human made, plan/skeleton vs approved-understanding **contradictions**, generated-vs-expected mismatches.

    - *gather:* 0-link/thin-seed false-negatives, dev-panel repos found, drift/stop signals from the explore loop.

    - *review/compare:* explicit corrected facts (e.g. "QR removal is individual-only"), not-built gaps.

2.  **Distil** — ONE `asyncio.to_thread`-offloaded LLM call turns candidate signals into `statement`

    - `source_refs` + `confidence` + `kind`. No signals → no call. Failure → heuristic passthrough or noop. (Same offload discipline as `refine.next_questions`; see the Cloud-Run-timeout constraint.)

3.  **Grounding gate** — drop any lesson whose `source_refs` don't resolve to a real pack/index node (reuse `explore.index.graph_grounded` / `match_index_nodes`). Mirrors the G4 lead-grounding gate.

4.  **Dedup** — content-key (normalised statement) + recall of near-duplicate existing lessons; skip or `supersede` instead of re-writing.

5.  **Persist** — `bank.upsert_insight` + one batched CAS `update_index`. Append a `capture` run-log.

## Bounds, config, rollout

- **One LLM call per step, thread-offloaded, and OFF the response critical path** (§4b) — capture never gates the step's reply or the next step. Best-effort; degrade to heuristic/noop; never raise into the pipeline. (Cloud-Run liveness/timeout history: serial blocking Vertex calls have killed an instance.)

- **Flags:** `KGA_CAPTURE_LESSONS`, `TPD_CAPTURE_LESSONS` (+ `*_RECALL_LESSONS` for the recall side) — code default OFF (`common/learn/config.py` `_on`), **declared and currently enabled (**`="1"`**) in** `deployments/services.tf`. Dark-launch capability; presently ON in the deployed config.

- **CAS index writes** batched per step; run-log of captures for auditability.

## Execution model — ASYNC, off the response critical path

Capture must **never delay a step's response or gate the next step**. The handler computes and returns the step result to the client **first**; distillation runs **afterward, concurrently**. The human gate stays instant — the client sees the step complete and moves on while the agent learns in the background.

### **Cloud-Run caveat (why "async" ≠ naive fire-and-forget):**

once the request's response is sent, Cloud Run **throttles CPU on the instance** unless CPU-always-allocated is set — so a bare post-response `asyncio.create_task(...)` can be starved or killed mid-distillation (and serial blocking Vertex calls have already tripped `ERROR_TIMEOUT` and killed an instance here).

### **Chosen design — durable background job on the memory bank's own store (default).**

The step handler, right before returning, **enqueues a lightweight** `CaptureJob` (context_id, run_id, step, a compact signal payload) onto a durable queue and returns immediately. A **background drain** — a small worker coroutine, and/or opportunistic draining at the head of the *next* request to that agent — pulls pending jobs and runs the distillation + grounding gate + `upsert_insight`. Properties:

- **Fully decoupled** from the response: the client's step reply and the next step never wait on capture.

- **Not lost on a cold hand-off:** the job is persisted, so an instance recycle between enqueue and distillation just means the next drain picks it up (**at-least-once** — the drain removes a job only after its capture runs; the content-key insight ids / `supersede` dedup + CAS index write make re-processing idempotent).

- **Reuses existing infra, no new queue service.**

*Enqueue is O(1)*, so it adds negligible latency to the handler — the expensive LLM distillation happens entirely in the drain, off the request path.

> **Backend (as implemented in L1):** the queue lives on the **memory bank's own GCS** (`memory/learn/ capture-queue.json`), read/written with a generic compare-and-set (`MemoryBank.mutate_json`, a generalization of the index CAS loop). Chosen over the A2A **Cloud SQL** `DatabaseTaskStore` because that store has a fixed A2A-Task schema (awkward for arbitrary jobs), whereas the bank's GCS is self-contained, equally durable, reuses the store the lessons already live in, and is unit-testable with the fake bucket. A dedicated Cloud SQL `capture_jobs` table remains a drop-in alternative backend if the queue ever needs SQL-side querying.

### **Fallback (only if the task-store route is deferred):**

`asyncio.create_task(capture_lessons(...))` after the result event is enqueued, under Cloud Run **CPU-always-allocated** (or `min-instances ≥ 1`); simpler but best-effort and **drops the lesson** if the instance recycles mid-capture. Not the default.

Either way the LLM call stays **thread-offloaded + bounded**, and **dedup (content-key +** `supersede`**) plus the CAS index write** make concurrent/duplicate/re-processed captures safe.

## Safety — this is the memory-bias problem again

Self-learning makes the de-bias phases **more** important, not less. Mandatory safeguards:

- **Confidence tiers** — human-confirmed `high`; agent-derived `low` (vetoable), never auto-`shared`.

- **Mandatory citations** (§4.3) — no grounding, no lesson.

- **Human veto / retraction** — a `veto-lesson <id>` MCP tool sets `status=vetoed` (reusing the `rejected` pattern); vetoed lessons are excluded from recall and never re-promoted.

- **De-biased recall** — lesson recall runs through **B4 IDF hub-penalty + B5 grounding**: an over-general or off-topic lesson can't flood an unrelated run's promotions. (When semantic recall lands, the same B4/B5 gates run on the **vector path**

- **Promotion criteria** — `context` → `shared` only when human-confirmed **or** corroborated across ≥N runs; default stays `context`.

- **Supersede on contradiction** — a newer, higher-confidence lesson replaces an older one rather than both being recalled.

## Open questions

- Where is the human veto surfaced in the client flow (a review gate after each step, or a periodic `search-lessons` sweep)?

- `context` → `shared` promotion: human-confirm only, or corroboration threshold N?

- LLM cost/latency budget per step (one extra call per stage × pipeline length).

- Do we ever auto-**correct the pack** from a recalled lesson, or only surface it?
