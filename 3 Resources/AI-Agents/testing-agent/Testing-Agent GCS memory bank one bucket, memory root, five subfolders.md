---
ai_hash: 8523fc9ba09b365d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-29
entities:
- Testing-Agent
- ai-agentic-framework/test-agent
- GCS bucket
- mt-receive-ai-agent-memory
- klara-nonprod
- memory/
- ROOT = "memory"
- knowledge_gathering/memory/bank.py
- knowledge-gathering
- test-plan-definition
- context_id
- index/
- notes/
- refine/<context_id>/
- test-plan/<context_id>/
- runs/
- knowledge-index graph
- curated notes
- questions, answers, understanding.md, state
- plan, decisions, scenarios, steps, features
- run-logs
- KG bank
- Step 2
- Step 3+4
- test_plan_definition/memory/writers.py
- docs/USAGE.md
- clear_memory.sh
source: session 2026-08-29
status: seedling
tags:
- testing-agent
- gcs
- memory-bank
- ai-agentic-framework
title: 'Testing-Agent GCS memory bank: one bucket, memory/ root, five subfolders'
type: concept
---

# Testing-Agent GCS memory bank: one bucket, memory/ root, five subfolders

The Testing-Agent (ai-agentic-framework/test-agent) persists all pipeline output to a **single GCS bucket** — `GCS_BUCKET = mt-receive-ai-agent-memory` (project `klara-nonprod`) — under one prefix root, `memory/`. The root is defined once as `ROOT = "memory"` in `knowledge_gathering/memory/bank.py`. Both agents (knowledge-gathering and test-plan-definition) share the same bucket; each stage writes its **own sub-prefix**, so two runs that share a `context_id` never clobber each other.

Under `memory/` the code writes **five** subfolders (a common mistake is to assume three):

- `index/` — knowledge-index graph (json + md, written with a compare-and-set generation guard) — KG bank
- `notes/` — `jira/<KEY>`, `confluence/<id>`, `insight/<id>` curated notes — KG bank
- `refine/<context_id>/` — Step 2: questions, answers, understanding.md, state — KG bank
- `test-plan/<context_id>/` — Step 3+4: plan, decisions, scenarios, steps, features — `test_plan_definition/memory/writers.py`
- `runs/` — run-logs (`<ts>_run/refine/plan-<id>.md`) — both agents

**Why it matters:** to reset the bank you must clear all five prefixes; wiping only the per-context folders leaves `index/` pointing at deleted notes. Sources: `knowledge_gathering/memory/bank.py`, `test_plan_definition/memory/writers.py`, `docs/USAGE.md`.

## Related
[[clear_memory.sh wipes the Testing-Agent GCS memory bank (preview-first)]]

## Related

- [[clear_memory.sh wipes the Testing-Agent GCS memory bank (preview-first)]]

%% ai-graph-start %%

**Related notes:**
- [[clear_memory.sh wipes the Testing-Agent GCS memory bank (preview-first)]]
- [[Wipe test-agent-v2 memory and taskstore via test-agent-v2tools]]
- [[test-agent-v2 cloud resource and credential map (klara-nonprod)]]
- [[Pipeline stages sharing a context_id need separate memory-bank path prefixes]]
- [[Testing Agent builds each pipeline stage as a package mirroring the knowledge_gathering skeleton]]

**Relations:**
- Testing-Agent — *is also known as* — ai-agentic-framework/test-agent
- Testing-Agent — *persists output to* — GCS bucket
- GCS bucket — *is named* — mt-receive-ai-agent-memory
- mt-receive-ai-agent-memory — *is in project* — klara-nonprod
- GCS bucket — *uses prefix root* — memory/
- memory/ — *is defined by* — ROOT = "memory"
- ROOT = "memory" — *is located in* — knowledge_gathering/memory/bank.py
- knowledge-gathering — *shares* — GCS bucket
- test-plan-definition — *shares* — GCS bucket
- knowledge-gathering — *writes its own* — sub-prefix
- test-plan-definition — *writes its own* — sub-prefix
- memory/ — *contains subfolder* — index/
- memory/ — *contains subfolder* — notes/
- memory/ — *contains subfolder* — refine/<context_id>/
- memory/ — *contains subfolder* — test-plan/<context_id>/
- memory/ — *contains subfolder* — runs/
- index/ — *stores* — knowledge-index graph
- index/ — *is a* — KG bank
- notes/ — *stores* — curated notes
- notes/ — *is a* — KG bank
- refine/<context_id>/ — *stores* — questions, answers, understanding.md, state
- refine/<context_id>/ — *is associated with* — Step 2
- refine/<context_id>/ — *is a* — KG bank
- test-plan/<context_id>/ — *stores* — plan, decisions, scenarios, steps, features
- test-plan/<context_id>/ — *is associated with* — Step 3+4
- test-plan/<context_id>/ — *is written by* — test_plan_definition/memory/writers.py
- runs/ — *stores* — run-logs
- runs/ — *is used by* — knowledge-gathering
- runs/ — *is used by* — test-plan-definition
- knowledge_gathering/memory/bank.py — *is a source for* — memory bank structure
- test_plan_definition/memory/writers.py — *is a source for* — memory bank structure
- docs/USAGE.md — *is a source for* — memory bank structure
- clear_memory.sh — *wipes* — Testing-Agent GCS memory bank

%% ai-graph-end %%