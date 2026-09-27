---
ai_hash: 054cd0b256212937
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-03
entities:
- Exploration Loop
- Marginal Yield
- Fixed Iteration Count
- Agent
- Unknown-Size Space
- Round
- New Items
- Next Query/Focus
- Previous Round
- Discovered Content
- Result
- Accumulated Visited Set
- New-Item Count
- Hard Bound
- Max Rounds
- Total Wall-Clock/Token Budget
- Convergence
- Self-Exploration
- Salient Token
- Title of Node
- Related Item
- Seed
- Derived Focus
- Broad Token Set
- LLM Query Enrichment
- Substring-OR Matcher
- Loop State
- Stable ID
- Context/Session ID
- Durable Storage
- Core Loop
- LLM
- Model Call
- Event Loop
- Test-Agent KGA Self-Exploration Controller (G5)
- Fan-out
- Crawl
- Reflect
- Derive-Focus
- Retrieval Tiering
- Knowledge Source
- Token Set
- Under-Exploration
- Wasted Rounds
- Rich Cases
- Exhausted Ones
source: session 2026-09-03 — KGA G5 controller
status: seedling
tags:
- agent-design
- agentic-loop
- convergence
- retrieval
- resumable
title: Converge an exploration loop on marginal yield (zero new items), not a fixed
  iteration count
type: lesson
---

# Converge an exploration loop on marginal yield (zero new items), not a fixed iteration count

When an agent explores an unknown-size space in rounds (crawl more, search more, discover more), **stop on MARGINAL YIELD — a round that adds nothing new — not on a fixed number of iterations or a fixed depth.** A fixed count both under-explores rich cases and wastes rounds on exhausted ones; the "did this round add any new item?" signal adapts to the actual space.

Concretely, per round: derive the next query/focus from what the *previous* round discovered, run it, and dedup results against an accumulated `visited` set. Converge when the new-item count for a round is zero (and also guard with a hard bound — max rounds + a total wall-clock/token budget — so a pathological case still terminates). Check convergence at more than one point: before the expensive step (nothing new to expand) and after it (the step produced nothing new).

**Deriving the next rounds focus from discovered content is the engine of self-exploration** — e.g. take the salient tokens of the titles of nodes found this round and search on those; that surfaces related items the seed never linked (in one real run it derived "release 7.29", a term absent from the seed). Keep the derived focus TIGHT (few high-signal tokens) — broad token sets over-match ([[LLM query enrichment for a substring-OR matcher must contract, not expand, the token set]]).

**Make the loop RESUMABLE**: persist `{round, visited, focus, reflections}` keyed by a stable id (context/session id) to durable storage at each round boundary; on start, load and resume from the stored round rather than restarting. This is what lets a loop survive a redeploy / request-timeout / crash mid-exploration instead of re-doing all the expensive work. Keep the core loop cheap (no LLM per round if the focus can be derived mechanically); gate any per-round model calls behind flags and offload them so they never block the event loop.

Surfaced building the test-agent KGA self-exploration controller (G5): fan-out → crawl → reflect → derive-focus → converge, bounded + resumable, default-off. Related: [[Retrieval tiering: query knowledge sources cheapest and most-trusted first]].

## Related

- [[Retrieval tiering: query knowledge sources cheapest and most-trusted first]]
- [[LLM query enrichment for a substring-OR matcher must contract]]
- [[not expand]]
- [[the token set]]

%% ai-graph-start %%

**Related notes:**
- [[Knowledge-Gathering loop is a bounded frontier crawl with a verify edge]]
- [[LLM query enrichment for a substring-OR matcher must contract, not expand, the token set]]
- [[KGA self-exploration G2-G5 map to four KGA_ env flags]]
- [[Retrieval tiering query knowledge sources cheapest and most-trusted first]]
- [[A link-following crawl pulls in graph-adjacent but topically-tangential nodes]]

**Relations:**
- Exploration Loop — *converges_on* — Marginal Yield
- Exploration Loop — *avoids* — Fixed Iteration Count
- Agent — *explores* — Unknown-Size Space
- Agent — *explores_in* — Round
- Marginal Yield — *means* — Zero New Items
- Fixed Iteration Count — *causes* — Under-Exploration
- Fixed Iteration Count — *causes* — Wasted Rounds
- Fixed Iteration Count — *under_explores* — Rich Cases
- Fixed Iteration Count — *wastes_rounds_on* — Exhausted Ones
- Next Query/Focus — *derived_from* — Discovered Content
- Discovered Content — *from* — Previous Round
- Result — *deduplicated_against* — Accumulated Visited Set
- New-Item Count — *signals* — Convergence
- Convergence — *guarded_by* — Hard Bound
- Hard Bound — *includes* — Max Rounds
- Hard Bound — *includes* — Total Wall-Clock/Token Budget
- Discovered Content — *enables* — Self-Exploration
- Derived Focus — *uses* — Salient Token
- Salient Token — *from* — Title of Node
- Derived Focus — *identifies* — Related Item
- Derived Focus — *has_property* — Tight
- Broad Token Set — *leads_to* — Over-Matching
- LLM Query Enrichment — *contracts* — Token Set
- Exploration Loop — *is_resumable* — true
- Exploration Loop — *persists* — Loop State
- Loop State — *keyed_by* — Stable ID
- Stable ID — *is_a* — Context/Session ID
- Loop State — *stored_in* — Durable Storage
- Core Loop — *has_property* — Cheap
- Model Call — *includes* — LLM
- Model Call — *should_not_block* — Event Loop
- Test-Agent KGA Self-Exploration Controller (G5) — *is_a* — Self-Exploration
- Test-Agent KGA Self-Exploration Controller (G5) — *has_step* — Fan-out
- Test-Agent KGA Self-Exploration Controller (G5) — *has_step* — Crawl
- Test-Agent KGA Self-Exploration Controller (G5) — *has_step* — Reflect
- Test-Agent KGA Self-Exploration Controller (G5) — *has_step* — Derive-Focus
- Test-Agent KGA Self-Exploration Controller (G5) — *has_step* — Convergence
- Test-Agent KGA Self-Exploration Controller (G5) — *is* — Bounded
- Test-Agent KGA Self-Exploration Controller (G5) — *is* — Resumable
- Retrieval Tiering — *related_to* — Exploration Loop
- LLM Query Enrichment — *related_to* — Exploration Loop

%% ai-graph-end %%