---
ai_hash: a99b1582bda4be49
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-29
entities:
- clear_memory.sh
- Testing-Agent
- GCS memory bank
- test-agent/tools/clear_memory.sh
- pipeline run
- CONFIRM=1
- earchive-data-clean
- GCS_BUCKET
- GCP_PROJECT
- ../.env
- knowledge_gathering/config.py
- index
- notes
- refine
- test-plan
- runs
- FOLDERS
- gcloud storage ls
- gcloud storage rm
- ADC
- layout note
- 'Testing-Agent GCS memory bank: one bucket, memory/ root, five subfolders'
- 'Testing-Agent GCS memory bank: one bucket'
- memory/ root
- five subfolders
source: session 2026-08-29
status: seedling
tags:
- testing-agent
- gcs
- tooling
- bash
- ai-agentic-framework
title: clear_memory.sh wipes the Testing-Agent GCS memory bank (preview-first)
type: howto
---

# clear_memory.sh wipes the Testing-Agent GCS memory bank (preview-first)

`test-agent/tools/clear_memory.sh` truncates (deletes every object in) the Testing-Agent GCS memory bank so the next pipeline run starts clean.

**Design decisions:**
- **Preview-first / dry-run by default.** A plain run only lists per-folder object counts and deletes nothing; you must re-run with `CONFIRM=1` to actually delete. This mirrors the `earchive-data-clean` skill convention — destructive ops never fire on the first invocation.
- **Config derives from the app .env.** It reads `GCS_BUCKET` / `GCP_PROJECT` from `../.env` (test-agent/.env — the same source `knowledge_gathering/config.py` uses) unless already exported, so the tool and the code can never disagree on which bucket is the bank. Only those two keys are grepped out — no secrets are sourced.
- **Per-folder loop over the five prefixes** (`index notes refine test-plan runs`). `FOLDERS="refine test-plan"` limits the wipe to a subset; `GCS_BUCKET=other` overrides the bucket.

**Mechanics:** counts objects with `gcloud storage ls "<prefix>**"` (the `**` glob matches leaf objects recursively; filter out trailing-slash placeholder lines), deletes with `gcloud storage rm --recursive <prefix>`. Requires gcloud authenticated via ADC.

**Gotcha:** to fully reset you want all five folders — see the linked layout note for why clearing only per-context folders leaves a dangling `index/`.

## Related
[[Testing-Agent GCS memory bank one bucket, memory root, five subfolders|Testing-Agent GCS memory bank: one bucket, memory/ root, five subfolders]]

## Related

- [[Testing-Agent GCS memory bank: one bucket]]
- [[memory/ root]]
- [[five subfolders]]

%% ai-graph-start %%

**Related notes:**
- [[Testing-Agent GCS memory bank one bucket, memory root, five subfolders]]
- [[Wipe test-agent-v2 memory and taskstore via test-agent-v2tools]]
- [[Pipeline stages sharing a context_id need separate memory-bank path prefixes]]
- [[Testing-agent admin tools get_run dumps an unbounded 1.4MB payload (MCP-unusable)]]
- [[test-agent-v2 cloud resource and credential map (klara-nonprod)]]

**Relations:**
- clear_memory.sh — *wipes* — GCS memory bank
- clear_memory.sh — *is located at* — test-agent/tools/clear_memory.sh
- test-agent/tools/clear_memory.sh — *truncates* — GCS memory bank
- test-agent/tools/clear_memory.sh — *deletes objects in* — GCS memory bank
- test-agent/tools/clear_memory.sh — *enables clean start for* — pipeline run
- test-agent/tools/clear_memory.sh — *has design principle* — preview-first
- test-agent/tools/clear_memory.sh — *has design principle* — dry-run by default
- test-agent/tools/clear_memory.sh — *requires* — CONFIRM=1
- test-agent/tools/clear_memory.sh — *mirrors convention of* — earchive-data-clean
- test-agent/tools/clear_memory.sh — *derives config from* — ../.env
- test-agent/tools/clear_memory.sh — *reads* — GCS_BUCKET
- test-agent/tools/clear_memory.sh — *reads* — GCP_PROJECT
- GCS_BUCKET — *from* — ../.env
- GCP_PROJECT — *from* — ../.env
- knowledge_gathering/config.py — *uses* — ../.env
- test-agent/tools/clear_memory.sh — *loops over* — five prefixes
- five prefixes — *include* — index
- five prefixes — *include* — notes
- five prefixes — *include* — refine
- five prefixes — *include* — test-plan
- five prefixes — *include* — runs
- FOLDERS — *limits wipe to* — subset
- test-agent/tools/clear_memory.sh — *uses command* — gcloud storage ls
- test-agent/tools/clear_memory.sh — *uses command* — gcloud storage rm
- test-agent/tools/clear_memory.sh — *requires authentication via* — ADC
- GCS memory bank — *has structure described in* — layout note
- index — *can be* — dangling
- Testing-Agent — *owns* — GCS memory bank
- Testing-Agent GCS memory bank: one bucket, memory/ root, five subfolders — *describes* — GCS memory bank
- Testing-Agent GCS memory bank: one bucket — *describes* — GCS memory bank
- GCS memory bank — *includes* — memory/ root
- GCS memory bank — *includes* — five subfolders

%% ai-graph-end %%