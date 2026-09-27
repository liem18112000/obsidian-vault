---
ai_hash: c0f7acf0b3725262
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-03
entities:
- KGA
- knowledge-gathering-agent
- KGA self-exploration stack
- G2
- KGA_LLM_HYPOTHESIZE
- G3
- KGA_FOLLOW_WEB
- G4
- KGA_LLM_LEADS
- G5
- KGA_EXPLORE_LOOP
- knowledge_gathering
- TPD
- LLM
- external-web URLs
- GCS
- context_id
- Cloud Runs
- Terraform
- env flags
- gcloud run update
- KGA_EXPLORE_MAX_ROUNDS
- KGA_EXPLORE_TIME_BUDGET
- KGA_EXPLORE_ROUND_NODES
- KGA_EXPLORE_ROUND_SECONDS
- KGA service
- LLM-focused search terms
- external-LLM leads
- multi-round explore loop
- round-0
- loop state
- Cloud Runs timeout
source: session 2026-09-03
status: seedling
tags:
- kga
- test-agent
- knowledge-gathering
- feature-flags
title: KGA self-exploration G2-G5 map to four KGA_ env flags
type: term
---

# KGA self-exploration G2-G5 map to four KGA_ env flags

The KGA (knowledge-gathering-agent) "self-exploration" stack is four independently gated phases, each a default-OFF env flag (truthy = `1/true/yes/on`), all read in `knowledge_gathering` (the KGA service only, not TPD):

- **G2** `KGA_LLM_HYPOTHESIZE` — LLM-focused search terms (round-0 only).
- **G3** `KGA_FOLLOW_WEB` — fetch ticket-linked external-web URLs (else recorded-not-fetched).
- **G4** `KGA_LLM_LEADS` — external-LLM leads + grounding gate.
- **G5** `KGA_EXPLORE_LOOP` — the bounded, resumable multi-round explore loop that turns the single pre-crawl fan-out into fan-out -> crawl -> reflect -> next-focus -> repeat.

Key design point: the **G5 core loop needs NO LLM** — round-N focus is heuristic salient tokens over round N-1 node titles; G2/G4 are optional LLM add-ons layered on top and run round-0 only, thread-offloaded. G5 bounds (env-tunable): `KGA_EXPLORE_MAX_ROUNDS`=3, `KGA_EXPLORE_TIME_BUDGET`=300s total, `KGA_EXPLORE_ROUND_NODES`=20, `KGA_EXPLORE_ROUND_SECONDS`=120s — all kept under Cloud Runs 600s timeout, and loop state is persisted to GCS by `context_id` so a redeploy resumes mid-loop.

## Related
[[Terraform-managed Cloud Run: set env flags in TF, not gcloud run update]]

## Related

- [[Terraform-managed Cloud Run: set env flags in TF]]
- [[not gcloud run update]]

%% ai-graph-start %%

**Related notes:**
- [[Converge an exploration loop on marginal yield (zero new items), not a fixed iteration count]]
- [[Terraform-managed Cloud Run set env flags in TF, not gcloud run update]]
- [[test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent target]]
- [[Drive the KGA A2A agent offline via Starlette TestClient for evaluation]]
- [[Deploying the test-agent-v2 Cloud Run stack (names, tags, plan)]]

**Relations:**
- KGA — *IS_AN_ACRONYM_FOR* — knowledge-gathering-agent
- KGA self-exploration stack — *HAS_PHASE* — G2
- KGA self-exploration stack — *HAS_PHASE* — G3
- KGA self-exploration stack — *HAS_PHASE* — G4
- KGA self-exploration stack — *HAS_PHASE* — G5
- G2 — *MAPS_TO* — KGA_LLM_HYPOTHESIZE
- G3 — *MAPS_TO* — KGA_FOLLOW_WEB
- G4 — *MAPS_TO* — KGA_LLM_LEADS
- G5 — *MAPS_TO* — KGA_EXPLORE_LOOP
- KGA_LLM_HYPOTHESIZE — *IS_A* — env flag
- KGA_FOLLOW_WEB — *IS_A* — env flag
- KGA_LLM_LEADS — *IS_A* — env flag
- KGA_EXPLORE_LOOP — *IS_A* — env flag
- KGA_LLM_HYPOTHESIZE — *READ_IN* — knowledge_gathering
- KGA_FOLLOW_WEB — *READ_IN* — knowledge_gathering
- KGA_LLM_LEADS — *READ_IN* — knowledge_gathering
- KGA_EXPLORE_LOOP — *READ_IN* — knowledge_gathering
- knowledge_gathering — *IS_A* — KGA service
- knowledge_gathering — *EXCLUDES* — TPD
- KGA_LLM_HYPOTHESIZE — *USES* — LLM-focused search terms
- KGA_LLM_HYPOTHESIZE — *RUNS_AT* — round-0
- KGA_FOLLOW_WEB — *FETCHES* — external-web URLs
- KGA_LLM_LEADS — *USES* — external-LLM leads
- KGA_EXPLORE_LOOP — *IS_A* — multi-round explore loop
- KGA_EXPLORE_LOOP — *DOES_NOT_USE* — LLM
- G2 — *IS_AN_ADD_ON_FOR* — KGA_EXPLORE_LOOP
- G4 — *IS_AN_ADD_ON_FOR* — KGA_EXPLORE_LOOP
- G2 — *USES* — LLM
- G4 — *USES* — LLM
- G4 — *RUNS_AT* — round-0
- loop state — *PERSISTED_TO* — GCS
- loop state — *IDENTIFIED_BY* — context_id
- KGA_EXPLORE_LOOP — *HAS_BOUND* — KGA_EXPLORE_MAX_ROUNDS
- KGA_EXPLORE_LOOP — *HAS_BOUND* — KGA_EXPLORE_TIME_BUDGET
- KGA_EXPLORE_LOOP — *HAS_BOUND* — KGA_EXPLORE_ROUND_NODES
- KGA_EXPLORE_LOOP — *HAS_BOUND* — KGA_EXPLORE_ROUND_SECONDS
- KGA_EXPLORE_LOOP — *KEPT_UNDER* — Cloud Runs timeout
- Terraform — *MANAGES* — Cloud Runs
- Terraform — *SETS* — env flags
- gcloud run update — *IS_NOT_USED_TO_SET* — env flags

%% ai-graph-end %%