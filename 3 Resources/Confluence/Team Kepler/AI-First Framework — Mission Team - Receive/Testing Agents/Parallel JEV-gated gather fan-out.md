---
ai_hash: 5e6aeed06ee977d1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49775935509'
confluence_path: 'Team Kepler > AI-First Framework — Mission Team: Receive > Testing
  Agents > TypeSafe AI''s Jev: A System One Model for Fast, Structured Decisions'
created: 2026-09-22
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- ai-agents
- jev
title: Parallel JEV-gated gather fan-out
type: source
updated: 2026-09-22
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49775935509/Parallel+JEV-gated+gather+fan-out
---

# Parallel JEV-gated gather fan-out

*Confluence source · Team Kepler › AI-First Framework — Mission Team: Receive › Testing Agents › TypeSafe AI's Jev: A System One Model for Fast, Structured Decisions · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49775935509/Parallel+JEV-gated+gather+fan-out) · updated 2026-09-22*

## Overview

Gather's pre-crawl fan-out — three LLM planners then four seed producers — runs **strictly serially today**; only the crawl itself is parallel. Two things follow from the code:

1.  **The serial part is independently parallelizable.** The 3 planners share one input and write distinct keys; 4 of the seed producers are independent reads. Wrap them in the *same* `asyncio.gather` pattern the implement stage already uses, and wall-clock drops from **Σ of the hops to MAX of the hops**.

2.  **It pays here where it didn't for implement.** Implement's fan-out is pinned to `Semaphore(1)` because every batch funnels into **one shared Vertex quota** (throughput-bound — measured ~0 speedup). Gather's sources hit **distinct backends** (Atlassian REST, GCP discovery/logging, public web, GCS/pgvector, Bitbucket). Distinct I/O → parallel actually cuts the clock. *Same structure, different bottleneck.*

On top of the fan-out, front it with a **JEV activation gate**: one calibrated `Noul(P, confidence)` per candidate source answering *"will this source yield in-scope knowledge for this ticket?"* — run the source only when JEV is **confident it won't help** enough to skip it. This is the exact JEV cascade already shipped for the assured judge (`DecisionProvider` port, default OFF, strictly additive), reused as a **source selector** instead of a suite scorer.

> **The principle.** *Don't fire the sources one at a time, and don't fire the ones a fast calibrated gate says won't pay. Pick the worthwhile subset, then fan it out in parallel. Worst case is exactly today's behaviour.*

## Where it slots (grounded in the code)

`GatherAgent._run_async_impl` runs the pipeline in this order — everything inside **one** A2A request, no cross-request parallelism:

|  |  |  |
|----|----|----|
| Phase | Sub-activity | Concurrency today |
| probe | `seed_probe` (Jira `get_issue`) | serial, always first |
| plan (only if `explore=True`) | **hypothesize** → focus terms | serial (1/3) |
|  | **leads** → external leads | serial (2/3) |
|  | **cloud_explore** → service hints | serial (3/3) |
| expand | memory **self-seed** | serial |
|  | semantic self-seed (opt-in, default OFF) | serial |
|  | **atlassian search** (JQL+CQL) | serial |
|  | **ground_leads** (needs `leads`) | serial |
|  | **cloud discover** (service discovery) | serial |
| crawl | frontier BFS over all seeds | **concurrent** (`Semaphore(8)` + `asyncio.gather`, `crawl.py:36,55`) |

![[image-20260922-081231.png]]

The planners run one-after-another at `agent.py`; the seed producers are sequential `await`s in `expansion_round`. **Only the crawl is parallel.** So the fan-out that feeds the crawl is exactly the serial stretch worth attacking — and unlike the earlier "serial blocking Vertex blew Cloud Run liveness" incident, every blocking call here is already `asyncio.to_thread`-offloaded, so the win on the table is pure **latency (Σ→MAX)**, not unblocking.

## Part 1 — Parallelize the fan-out (the "fan-out like implement" ask)

### The two independent clusters

From the dependency graph (`agent.py:62-84`, `expand.py:48-69`):

- **Hard sequence (real data dep):** `seed_probe` → everything · `hypothesize` **→** `terms` **→ every term-consuming seed producer** (`_plan` does `terms = hyp`; `atlassian_search` / `cloud_discover` / `semantic_self_seed` all read it) · `leads` → `ground_leads` · `cloud_explore` → `cloud discover` · (all seeds) → `crawl`. The `hypothesize → terms` edge is why the fan-out is **two waves** (planners ∥, then seeds ∥), not one — overlapping them would feed seeds the pre-hypothesis terms.

- **Independent → parallelizable:**

  - **Cluster A — the 3 planners** (`hypothesize`, `leads`, `cloud_explore`): all consume the same `PlanInput(probe)` and write distinct session keys (`kga_hypothesis` / `kga_leads` / `kga_cloud_plan`). No inter-dependency.

  - **Cluster B — the seed producers** (`memory self-seed`, `atlassian search`, `cloud discover`; `ground_leads` runs in a **second wave** after `leads`). Independent reads on different backends.

`ground_leads` is the only intra-fan-out dependency — model it as a two-wave fan-out (leads-wave, then ground_leads joins the seed-wave), not a reason to keep the whole thing serial.

### The mechanism — reuse the implement pattern, verbatim shape

Implement's fan-out is `asyncio.gather` over a bounded `asyncio.Semaphore` (`implement/generate/llm.py:93-109`). Copy that shape into `expansion_round` and `_plan`:

```
# explore/expand.py — seed producers, was: 4 sequential awaits
sem = asyncio.Semaphore(_fanout_concurrency())          # KGA_FANOUT_CONCURRENCY, default 4
async def _run(src):                                    # src = one seed producer coroutine
    async with sem:
        return await src()
waves = await asyncio.gather(*(_run(s) for s in fired), return_exceptions=True)
seeds = _merge_dedup(waves)                             # exclude-chaining becomes a post-fanout merge
```

Three details the code forces:

1.  **Dedup moves after the fan-out.** Today each step threads `exclude | set(new_seeds)` to the next (`expand.py:51,55,60,67`) to avoid duplicate seeds. Under parallelism that chaining can't exist — replace it with a single merge-dedup over the collected wave. **This is a behaviour change, not a pure refactor:** the capped producers (`atlassian_search` top-5, `cloud_discover` top-8) apply `exclude` *before* the cap, so a parallel producer that sees only the initial exclude can spend a cap slot on a seed a sibling already found — the merge drops the duplicate, netting one fewer unique seed (bounded tail-recall; the caps target near-disjoint id-spaces). Recovered partly by running the free `memory_self_seed` first and folding its seeds into the wave's exclude; the residual is gated by the PQS/recall A/B, upgrade path = post-dedup cap re-fill.

2.  **Planners need separate ADK invocation contexts.** The 3 planners write distinct keys but share one `ctx.session.state` (`agent.py:57,69`). This is the *exact* trap that forced implement's `Semaphore(1)` — concurrent ADK runs on shared session state race. Fix is the one from `PLAN-parallel-generation.md` Phase A: give each planner its own invocation context / unique session id, not a shared `ctx`. (Cheaper than it sounds — planners are short single-shot calls.)

3.  `return_exceptions=True` **+ per-source degrade.** One source failing must not kill the wave — mirror the implement fallback: a failed producer contributes no seeds, the rest proceed. Gather already tolerates a thin seed set.

### Why it pays here but was pinned to 1 for implement

This is the load-bearing argument. `implement/generate/llm.py:54-58` documents *why* the batch concurrency is `1`: concurrent in-process ADK Runners re-broke batches **and** — even once fixed — the live A/B showed ~0 speedup because **all batches share one Vertex Claude quota** (throughput-bound). Every arrow lands on the same bottleneck node.

Gather's sources land on **different** nodes: Atlassian REST, GCP discovery + Cloud Logging, the public web, GCS memory + pgvector, the Bitbucket API. No shared quota wall. So the identical `asyncio.gather` structure that bought implement nothing buys gather a real Σ→MAX drop. **Copy the pattern — get the speedup implement couldn't.** (The one shared resource downstream is the GCS `MemoryBank`, addressed under Risks — writes are CAS-guarded and deferred to end-of-crawl.)

![[image-20260922-081701.png]]

## Part 2 — JEV as the activation gate ("should it run that gather?")

Parallel-and-blind still fires **every** source, spending LLM \$ on planners and rate-limit budget on the free reads even when the ticket obviously won't benefit. The user's ask — *mark each candidate with a confidence level and gate whether to run it* — is precisely JEV's `Noul` primitive, reused as a selector.

### The gate

For each candidate source, one `decision.noul(state, statement)` (`common/adk/providers/decision.py:45`) where `state` = the shared ticket seed + terms + probe (sent **once** — JEV answers all N in one parallel pass, `~70–500 ms` total) and `statement` = *"Source* `<X>` *will surface knowledge in scope for this ticket."* JEV returns `Verdict{value, probs, confidence}` (`decision.py:16-23`).

The gate rule is the **JEV cascade** (`D-JEV-2`: front, never replace), applied to activation:

```
conf ≥ conf_min  AND  P ≥ τ_run   →  FIRE
conf ≥ conf_min  AND  P <  τ_run   →  SKIP    ← the ONLY skip
conf <  conf_min                   →  FIRE    ← fall back — never trust a low-confidence skip
```

A source is dropped **only** when JEV is both confident *and* below the run bar. Everything else fires. Backend OFF (`TPD_DECISION_BACKEND` unset → `get_decision_provider()` returns `None`, `providers/__init__.py:31`) → the gate is a no-op and all sources fire. **Worst case = today.**

### Gate harder on the costly sources

The gate's value is asymmetric, so its default bar should be too:

|  |  |  |
|----|----|----|
| Source class | What a SKIP saves | Default stance |
| 3 LLM planners | real **Vertex \$** + quota | gate **on**, normal `τ_run` |
| free reads (atlassian / cloud discover / web) | Atlassian & GCP **rate-limit budget**, and **pack noise → precision** | gate **light** (only trims obvious misses) |
| capped sources (cloud discover top-N) | — | use JEV `Score` to *rank* the top-N, not just yes/no |

A confident skip on a planner is a dollar saved; a confident skip on a free read is mostly a **precision** win (fewer off-topic nodes in the pack → higher PQS precision, the exact metric the KGA eval already tracks). Both are upside; neither is on the critical correctness path.

![[image-20260922-081824.png]]

### Composition

Gate first, then fan-out. `select_sources(probe) -> fired[]` (one JEV call, all sources) returns the subset; the Part-1 fan-out runs *that subset* in parallel; merge/dedup; hand to the unchanged crawl. The two are orthogonal — the gate shrinks the set, the fan-out flattens its latency.

%% ai-graph-start %%

**Related notes:**
- [[Shared model quota makes LLM fan-out worthless]]
- [[Agent Loop 1 - Knowledge Gathering - v2]]
- [[Cap-before-exclude parallelism recall trap]]
- [[Integrating Jev into Test-Agent-V2 for Enhanced Decision-Making]]
- [[Agent self-learning memory]]

%% ai-graph-end %%