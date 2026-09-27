---
ai_hash: 8fafe1982737f54b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49730715847'
confluence_path: 'Team Kepler > AI-First Framework — Mission Team: Receive > Testing
  Agents'
created: 2026-09-07
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- ai-agents
title: Agent Loop 1 - Knowledge Gathering - v2
type: source
updated: 2026-09-11
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49730715847/Agent+Loop+1+-+Knowledge+Gathering+-+v2
---

# Agent Loop 1 - Knowledge Gathering - v2

*Confluence source · Team Kepler › AI-First Framework — Mission Team: Receive › Testing Agents · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49730715847/Agent+Loop+1+-+Knowledge+Gathering+-+v2) · updated 2026-09-11*

## Overview

### **Purpose.**

Companion to `RESEARCH-agentic-qa-enhancements.md`, which is entirely **test-plan / execution-side** (its four pillars all wrap `implement_plan` and add the missing test-*execution* stage).

This report covers the **first** agent instead: turning the **Knowledge Gathering Agent (KGA)** from a *deterministic link-follower* into a **self-exploring researcher** that can start from a **blank / thin issue — a title and nothing else** — and actively reach three source tiers to assemble a grounded pack:

1.  **Internal** — the agent's own GCS Memory Bank (prior gathers + confirmed insights).

2.  **Atlassian** — Jira & Confluence, but by **search** (JQL / CQL), not only by pre-existing links.

3.  **External** — the public internet (web search) and a public LLM (e.g. Gemini) as a **lead generator**.

## Anatomy & the exact gaps

### The three gaps, mapped to your three tiers

|  |  |  |
|----|----|----|
| Requirement (your ask) | Current behaviour | Precise gap |
| **Blank issue → guess & find** | Crawl starts from a concrete seed and follows only *existing* links. | No **hypothesis-from-title** and no **query generation**. A thin seed → ~1 node → stop. No recovery path for the known "0-link false-negative." |
| **Atlassian (Jira, Confluence)** | ✅ Fetched and followed — but only links already present on a node. | No **JQL/CQL search**. The Jira/Confluence clients fetch *by id* only; they can't answer "find issues/pages about `<title terms>`." |
| **External (internet, Gemini/LLM)** | `external-web` / `figma` / `google-doc` are **classified and recorded, never fetched** (`_fetchable` excludes them). | Entire external tier absent from the crawl. No web fetch, no external-LLM call. |
| **Internal (agent memory)** | `search_memory` / `get_note` exist as **manual read-only MCP tools**. | The crawl never **self-seeds from memory** or reuses prior insights; memory is write-during-crawl, read-on-demand. |

### What must NOT change

- **The human gate stays.** Self-exploration only *widens the candidate pack*; `refine → approve` still gate what becomes the test basis. Every discovered node must carry **provenance** so the human can prune.

- **The deterministic crawl stays.** New tiers feed it *seeds*; they don't replace `crawl.py`.

- **Codegraph grounding stays** (standing rule): a discovered repo still routes through recommend → user-confirm → build, not auto-clone.

## Target architecture — the Self-Exploration Loop

In words: a bounded controller wraps the existing crawl and, when the seed is thin runs

**hypothesize → fan-out across three tiers → ground → promote → crawl → reflect → converge**.

### The controller — bounded, resumable, one LLM call per round

The loop is the agentic capstone (analogue of the sibling's Assured Loop). Design rules it must obey:

- **One blocking LLM call per round** (the hypothesize step), offloaded to a thread — three serial blocking Vertex calls previously blew the Cloud Run liveness/request timeout.

- **Per-tier sub-budgets** on top of the existing `max_nodes / max_seconds`; a slow/failed tier degrades to a **declared gap**, never a hang (the crawl already turns fetch failures into gaps — extend the same discipline to searches and web fetches).

- **Persist loop state to GCS keyed by** `context_id` so a Cloud Run redeploy **resumes**, not restarts (a redeploy has wiped in-flight memory-bank state before, forcing a full re-run).

- **Convergence, not fixed depth**: stop when a round adds no new in-scope nodes or the budget is spent.

### Tier 1 — Internal memory (self-seed & reuse) ·

- Before the crawl, query the GCS index for prior nodes and **insights** whose id/title/type match the seed's terms.

- High-confidence matches are

  - \(a\) added as `extra_seeds`,

  - \(b\) surfaced to the interrogation as *prior knowledge*.

### Tier 2 — Atlassian search (JQL / CQL)

- Add **search** to the Atlassian clients — `search_jql(text)` and `search_cql(text)` — and a thin **expander** that builds queries from title + hypotheses.

- Hits are returned as candidate `jira:` / `confluence:` ids and **promoted** to seeds. This is what actually rescues a blank ticket: the title *"Export fails for restricted folders"* has no links, but a CQL/JQL sweep finds the spec page and the sibling bug.

- Implement either as a pre-crawl step or as a new `NodeFetcher` for a `jira-search:` / `confluence-search:` pseudo-kind that emits `LinkRecord`s.

### Tier 3a — External web (search + fetch)

- Promote `external-web` from *recorded* to *fetchable*: a `WebFetcher` that runs a web search over the query, fetches the top results, and distills them like any other node — **with citation** (`source_url` preserved, `origin="web-search"`).

- Add `external-web` (and, when needed, `google-doc`) to a widened **and** to a new outbound `Scope` gate so web nodes are followed only when explicitly enabled.

- Every web fact is cited; uncitable content is dropped.

### Tier 3b — External LLM (Other Agents)

- An expander that asks a LLM or Agent to **enumerate** likely subsystems, domain terms, related feature names, and plausible edge cases for the title.

- Its output is parsed into **candidate queries and seeds only** — it is fed *back into Tiers 1–3a*, never written into the pack as fact.

- The **grounding gate (③)** then keeps a lead only if it resolves to a real Jira/Confluence/web/code source; unresolved leads are dropped or demoted to an explicit "unconfirmed lead" the human can chase. This is the safeguard that keeps hallucinated requirements out of the test basis.

![[image-20260911-084938.png]]

## Extension — the live GCP estate (tiers 5/6/7)

The three source tiers above are all **document** sources — they answer *what the system is meant to do*. A companion plan adds three **operational** tiers that answer *what is actually deployed and running*, so the pack exercises the real failure modes, not only the designed ones. They are the operational peers of tiers 1–4 and feed the **same** hypothesize → ground → promote → crawl loop; only the source changes.

> **Full design + diagram:** `../../test-agent-v2/docs/PLAN-gcp-service-exploration-tiers.md` · `gcp-service-exploration-tiers.excalidraw` · `.png`

**Why this fits the loop, not fights it.** One GCP-explore **sub-agent** (a single `LlmAgent` of the same shape as `hypothesize`/`leads`) plans envs, ranks prominence, and distills/labels — it never *invents* a service or an edge; discovery and edges come only from real GCP fields (the **grounding gate**, ported from tier 3b). The reach is read-only, offloaded to threads, opt-in (`Scope.explore_gcp`, like `follow_web`), and bounded by the crawl's existing budget. Candidate service-nodes pass through the **same ③ GROUND gate** as every other tier before promotion — which is exactly what the new band in the diagram shows.

|  |  |  |
|----|----|----|
| Tier | Does | Rides which existing seam |
| **5 · DISCOVER** | Enumerate the *prominent* services across every env (`dev`, `dev-staging`, `performance`, `test`, `prod`) for **GKE**, **Cloud Run**, **managed** (Cloud SQL / Pub/Sub) via Cloud Asset Inventory; rank by term-match ∪ liveness ∪ env → promote top-N. | new **seed producer** in `expansion_round` → `gcpsvc:<env>/<platform>/<name>` ids |
| **6 · LOGS** | For each service, read Cloud Logging over an **expanding window** 7 → 14 → 21 → 28 d, widening only until *enough* signal (marginal-yield on time); distill purpose · error-sigs · deps; **redact** secrets/PII. | new `NodeFetcher(kind="gcpsvc")` — the "fetch" seam |
| **7 · RELATE** | Infer service-to-service communication edges from log fields · config · Cloud Trace; emit them as `LinkRecord`s so the crawl **walks the service graph**. | in-scope `LinkRecord` + `gcpsvc` in `_fetchable` — the "follow" seam |

%% ai-graph-start %%

**Related notes:**
- [[Sub Agentic Loop 1.2 - GCP Service Exploration]]
- [[Knowledge-Gathering loop is a bounded frontier crawl with a verify edge]]
- [[Agent self-learning memory]]
- [[Agent Loop 3 - Test-Plan Definition]]
- [[Agent Loop 4 - Test-Plan Execution]]

%% ai-graph-end %%