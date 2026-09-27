---
ai_hash: d73f35866b055dba
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-03
entities:
- KGA
- KGA self-exploration stack
- KGA_LLM_HYPOTHESIZE
- KGA_FOLLOW_WEB
- KGA_LLM_LEADS
- KGA_EXPLORE_LOOP
- knowledge_gathering
- TPD
- LLM
- GCS
- Cloud Runs
- Terraform
- KGA_EXPLORE_MAX_ROUNDS
- KGA_EXPLORE_TIME_BUDGET
- KGA_EXPLORE_ROUND_NODES
- KGA_EXPLORE_ROUND_SECONDS
- Terraform-managed Cloud Run
- env flag
- loop state
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
[[Terraform-managed Cloud Run set env flags in TF, not gcloud run update|Terraform-managed Cloud Run: set env flags in TF, not gcloud run update]]

## Related

- [[Terraform-managed Cloud Run set env flags in TF, not gcloud run update]]

%% ai-graph-start %%

**Related notes:**
- [[Terraform-managed Cloud Run set env flags in TF, not gcloud run update]]
- [[Converge an exploration loop on marginal yield (zero new items), not a fixed iteration count]]
- [[Agent Loop 1 - Knowledge Gathering - v2]]
- [[Sub Agentic Loop 1.2 - GCP Service Exploration]]
- [[test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent target]]

**Relations:**
- KGA self-exploration stack — *HAS_PHASE* — KGA_LLM_HYPOTHESIZE
- KGA self-exploration stack — *HAS_PHASE* — KGA_FOLLOW_WEB
- KGA self-exploration stack — *HAS_PHASE* — KGA_LLM_LEADS
- KGA self-exploration stack — *HAS_PHASE* — KGA_EXPLORE_LOOP
- KGA_LLM_HYPOTHESIZE — *IS_IDENTIFIED_AS* — G2
- KGA_FOLLOW_WEB — *IS_IDENTIFIED_AS* — G3
- KGA_LLM_LEADS — *IS_IDENTIFIED_AS* — G4
- KGA_EXPLORE_LOOP — *IS_IDENTIFIED_AS* — G5
- KGA_LLM_HYPOTHESIZE — *IS_A_TYPE_OF* — env flag
- KGA_FOLLOW_WEB — *IS_A_TYPE_OF* — env flag
- KGA_LLM_LEADS — *IS_A_TYPE_OF* — env flag
- KGA_EXPLORE_LOOP — *IS_A_TYPE_OF* — env flag
- KGA_LLM_HYPOTHESIZE — *HAS_DEFAULT_STATE* — OFF
- KGA_FOLLOW_WEB — *HAS_DEFAULT_STATE* — OFF
- KGA_LLM_LEADS — *HAS_DEFAULT_STATE* — OFF
- KGA_EXPLORE_LOOP — *HAS_DEFAULT_STATE* — OFF
- KGA_LLM_HYPOTHESIZE — *IS_READ_IN_SERVICE* — knowledge_gathering
- KGA_FOLLOW_WEB — *IS_READ_IN_SERVICE* — knowledge_gathering
- KGA_LLM_LEADS — *IS_READ_IN_SERVICE* — knowledge_gathering
- KGA_EXPLORE_LOOP — *IS_READ_IN_SERVICE* — knowledge_gathering
- knowledge_gathering — *IS_SERVICE_FOR* — KGA
- knowledge_gathering — *IS_NOT_SERVICE* — TPD
- KGA_LLM_HYPOTHESIZE — *FUNCTION* — LLM-focused search terms
- KGA_LLM_HYPOTHESIZE — *RUNS_DURING* — round-0 only
- KGA_FOLLOW_WEB — *FUNCTION* — fetch ticket-linked external-web URLs
- KGA_LLM_LEADS — *FUNCTION* — external-LLM leads + grounding gate
- KGA_EXPLORE_LOOP — *DESCRIPTION* — bounded, resumable multi-round explore loop
- KGA_EXPLORE_LOOP — *DOES_NOT_REQUIRE* — LLM
- KGA_LLM_HYPOTHESIZE — *IS_OPTIONAL_ADD_ON_FOR* — LLM
- KGA_LLM_LEADS — *IS_OPTIONAL_ADD_ON_FOR* — LLM
- KGA_LLM_HYPOTHESIZE — *IS_THREAD_OFFLOADED* — true
- KGA_LLM_LEADS — *IS_THREAD_OFFLOADED* — true
- KGA_EXPLORE_LOOP — *HAS_BOUND* — KGA_EXPLORE_MAX_ROUNDS
- KGA_EXPLORE_LOOP — *HAS_BOUND* — KGA_EXPLORE_TIME_BUDGET
- KGA_EXPLORE_LOOP — *HAS_BOUND* — KGA_EXPLORE_ROUND_NODES
- KGA_EXPLORE_LOOP — *HAS_BOUND* — KGA_EXPLORE_ROUND_SECONDS
- KGA_EXPLORE_MAX_ROUNDS — *IS_TUNABLE_BY* — env
- KGA_EXPLORE_TIME_BUDGET — *IS_TUNABLE_BY* — env
- KGA_EXPLORE_ROUND_NODES — *IS_TUNABLE_BY* — env
- KGA_EXPLORE_ROUND_SECONDS — *IS_TUNABLE_BY* — env
- loop state — *IS_PERSISTED_TO* — GCS
- Cloud Runs — *HAS_TIMEOUT_OF* — 600s
- Terraform — *MANAGES* — Cloud Run
- Terraform-managed Cloud Run — *SETS* — env flags
- Terraform-managed Cloud Run — *RELATED_TO_TOPIC* — set env flags in TF, not gcloud run update
- KGA self-exploration — *MAPS_TO* — KGA_LLM_HYPOTHESIZE
- KGA self-exploration — *MAPS_TO* — KGA_FOLLOW_WEB
- KGA self-exploration — *MAPS_TO* — KGA_LLM_LEADS
- KGA self-exploration — *MAPS_TO* — KGA_EXPLORE_LOOP

%% ai-graph-end %%