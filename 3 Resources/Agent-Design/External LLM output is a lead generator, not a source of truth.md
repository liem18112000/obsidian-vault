---
ai_hash: 9b5a95a2d87c9ac7
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-03
entities:
- External LLM output
- Lead generator
- Source of truth
- Agent
- Foreign public LLM
- Gemini
- Model family
- Knowledge source
- Hypotheses
- Search queries
- Working set
- Fact
- Grounding gate
- Candidate fact
- Verifiable, fetchable source
- Ticket
- Wiki page
- Web page
- Actual code
- Leads
- Unconfirmed lead
- Human
- LLM's hallucinations
- Downstream
- Testing agent
- Ungrounded 'requirement'
- Wrong test
- Real spec
- Downstream generator
- Triangulation
- Independent sources
- Single-source claim
- Retrieval tiering
- Last, least-trusted tier
- Knowledge Gathering Agent
- test-agent/docs/RESEARCH-self-exploring-knowledge-gather.md
- 'Retrieval tiering: query knowledge sources cheapest and most-trusted first'
source: session 2026-09-03 — self-exploring KGA design
status: seedling
tags:
- agent-design
- rag
- llm
- grounding
- hallucination
- qa
title: External LLM output is a lead generator, not a source of truth
type: lesson
---

# External LLM output is a lead generator, not a source of truth

When an agent uses a **foreign public LLM** (e.g. Gemini, a different model family from the one doing the main work) as a *knowledge source*, its output must be treated as **hypotheses and search queries only** — likely subsystems, domain terms, related items, plausible edge cases — and **never written into the working set as fact**.

A **grounding gate** then admits a candidate fact only if it **resolves to a verifiable, fetchable source** (a ticket, a wiki page, a web page, actual code). Leads that cannot be corroborated are **dropped, or demoted to an explicit "unconfirmed lead"** a human can chase — they never silently enter the pack.

**Why it matters:** skipping the gate injects the LLM's hallucinations straight into whatever the pack feeds downstream. In a testing agent, an ungrounded "requirement" becomes a **wrong test**; the LLM's fluent guess is indistinguishable from a real spec until it is grounded.

**Practical corollaries**
- Use a **different model family** for the lead generator than for the downstream generator, so you do not compound one model's blind spots (same reasoning as the LLM-as-judge self-enhancement-bias caution).
- Prefer **triangulation**: a claim backed by >=2 independent sources is high-confidence; a single-source (especially LLM-only) claim is a lead, not a fact.
- The gate is the enforcement point of the tiering discipline in [[Retrieval tiering query knowledge sources cheapest and most-trusted first|Retrieval tiering: query knowledge sources cheapest and most-trusted first]] — the external LLM is the **last, least-trusted tier**.

Surfaced designing the self-exploring Knowledge Gathering Agent (test-agent/docs/RESEARCH-self-exploring-knowledge-gather.md).

## Related

- [[Retrieval tiering query knowledge sources cheapest and most-trusted first|Retrieval tiering: query knowledge sources cheapest and most-trusted first]]

%% ai-graph-start %%

**Related notes:**
- [[Retrieval tiering query knowledge sources cheapest and most-trusted first]]
- [[LLM query enrichment for a substring-OR matcher must contract, not expand, the token set]]
- [[Ground-then-refine gathering grounds, refinement interprets and confirms]]
- [[AI as an accelerator with a human review gate]]
- [[LLM-as-a-judge biases position, verbosity, self-enhancement]]

**Relations:**
- External LLM output — *is a* — Lead generator
- External LLM output — *is not a* — Source of truth
- Agent — *uses* — Foreign public LLM
- Foreign public LLM — *is a type of* — Knowledge source
- Foreign public LLM — *example* — Gemini
- External LLM output — *must be treated as* — Hypotheses
- External LLM output — *must be treated as* — Search queries
- External LLM output — *must not be written into* — Working set
- Working set — *contains* — Fact
- Grounding gate — *admits* — Candidate fact
- Candidate fact — *resolves to* — Verifiable, fetchable source
- Verifiable, fetchable source — *includes* — Ticket
- Verifiable, fetchable source — *includes* — Wiki page
- Verifiable, fetchable source — *includes* — Web page
- Verifiable, fetchable source — *includes* — Actual code
- Leads — *cannot be* — Corroborated
- Leads — *are* — Dropped
- Leads — *are demoted to* — Unconfirmed lead
- Unconfirmed lead — *can be chased by* — Human
- Skipping the gate — *injects* — LLM's hallucinations
- LLM's hallucinations — *feed* — Downstream
- Ungrounded 'requirement' — *becomes* — Wrong test
- Wrong test — *occurs in* — Testing agent
- LLM's fluent guess — *is indistinguishable from* — Real spec
- Lead generator — *uses different* — Model family
- Model family — *differs from that of* — Downstream generator
- Triangulation — *prefers* — Claim backed by Independent sources
- Single-source claim — *is a* — Lead
- Single-source claim — *is not a* — Fact
- Grounding gate — *is enforcement point of* — Retrieval tiering
- External LLM — *is the* — Last, least-trusted tier
- Last, least-trusted tier — *is part of* — Retrieval tiering
- Concept — *surfaced designing* — Knowledge Gathering Agent
- Knowledge Gathering Agent — *documented at* — test-agent/docs/RESEARCH-self-exploring-knowledge-gather.md
- External LLM output is a lead generator, not a source of truth — *related to* — Retrieval tiering: query knowledge sources cheapest and most-trusted first

%% ai-graph-end %%