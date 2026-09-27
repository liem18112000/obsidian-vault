---
ai_hash: f90e1360e7e8b696
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
- app .env
- GCS_BUCKET
- GCP_PROJECT
- ../.env
- test-agent/.env
- knowledge_gathering/config.py
- FOLDERS
- refine
- test-plan
- gcloud storage ls
- gcloud storage rm
- gcloud
- ADC
- index
- notes
- runs
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
[[Testing-Agent GCS memory bank: one bucket, memory/ root, five subfolders]]

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
- [[Run test-agent-v2 locally with docker-compose (no GCP)]]

**Relations:**
- clear_memory.sh — *wipes* — GCS memory bank
- clear_memory.sh — *is located at* — test-agent/tools/clear_memory.sh
- clear_memory.sh — *enables clean start for* — pipeline run
- clear_memory.sh — *requires* — CONFIRM=1
- clear_memory.sh — *mirrors convention of* — earchive-data-clean
- clear_memory.sh — *derives config from* — app .env
- app .env — *contains* — GCS_BUCKET
- app .env — *contains* — GCP_PROJECT
- app .env — *is located at* — ../.env
- ../.env — *is also known as* — test-agent/.env
- knowledge_gathering/config.py — *uses* — test-agent/.env
- clear_memory.sh — *processes folder* — index
- clear_memory.sh — *processes folder* — notes
- clear_memory.sh — *processes folder* — refine
- clear_memory.sh — *processes folder* — test-plan
- clear_memory.sh — *processes folder* — runs
- FOLDERS — *limits wipe to* — refine
- FOLDERS — *limits wipe to* — test-plan
- clear_memory.sh — *uses command* — gcloud storage ls
- clear_memory.sh — *uses command* — gcloud storage rm
- clear_memory.sh — *requires* — gcloud
- gcloud — *authenticated via* — ADC
- GCS memory bank — *is for* — Testing-Agent
- GCS memory bank — *has root* — memory/ root
- memory/ root — *contains* — five subfolders
- five subfolders — *include* — index
- five subfolders — *include* — notes
- five subfolders — *include* — refine
- five subfolders — *include* — test-plan
- five subfolders — *include* — runs
- GCS memory bank — *is* — one bucket
- clear_memory.sh — *is related to* — Testing-Agent GCS memory bank: one bucket, memory/ root, five subfolders
- clear_memory.sh — *is related to* — Testing-Agent GCS memory bank: one bucket
- clear_memory.sh — *is related to* — memory/ root
- clear_memory.sh — *is related to* — five subfolders

%% ai-graph-end %%