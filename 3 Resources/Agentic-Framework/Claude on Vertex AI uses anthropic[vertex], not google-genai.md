---
ai_hash: 6144ed1437ffe6be
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-27
entities:
- Claude models
- Vertex AI
- Anthropic SDK
- anthropic[vertex]
- google-genai
- Gemini
- Python
- AnthropicVertex
- project_id
- region
- claude-sonnet-5
- global (GCP region)
- europe-west1 (GCP region)
- GCP
- IAM
- runtime service account
- roles/aiplatform.user
- Vertex AI API
- Knowledge-Gathering agent
- LUZ-159671
- Distill step
- VERTEX_PROJECT
- VERTEX_LOCATION
- VERTEX_MODEL
- a2a-sdk
- agent-card.json
- agent.json
- agent card
source: session 2026-08-27 — test-agent Distill
status: seedling
tags:
- vertex-ai
- claude
- anthropic
- gcp
- gotcha
title: Claude on Vertex AI uses anthropic[vertex], not google-genai
type: lesson
---

# Claude on Vertex AI uses anthropic[vertex], not google-genai

To call **Claude models on Vertex AI** from Python, use the Anthropic SDK with the Vertex extra — `pip install "anthropic[vertex]"`, then `from anthropic import AnthropicVertex` (client takes `project_id` + `region`). Do NOT use `google-genai` — that is Google’s SDK for **Gemini**, a different model family.

**Gotcha:** `claude-sonnet-5` on Vertex is served only in the **`global`** region for our project, so set `vertex_region = global` (a real GCP location like `europe-west1` returns model-not-found). IAM is the same as any Vertex call: the runtime service account needs `roles/aiplatform.user` (Vertex AI API), no extra role for Claude.

Decision on the Knowledge-Gathering agent (LUZ-159671): the Distill step uses `AnthropicVertex` with `claude-sonnet-5` in `global`; env `VERTEX_PROJECT/VERTEX_LOCATION/VERTEX_MODEL`.

## Related

- [[a2a-sdk serves the agent card at agent-card.json (new) or agent.json (old)]]

%% ai-graph-start %%

**Related notes:**
- [[Claude Code runs on Vertex AI via three env vars with gcloud ADC]]
- [[List Anthropic models on Vertex via the publisherModels REST endpoint]]
- [[Claude models are available on GCP Vertex AI Model Garden]]
- [[Claude Sonnet 5 confirmed working on Vertex AI for klara-nonprod]]
- [[Claude on Vertex AI availability is per-project per-region (klara-nonprod)]]

**Relations:**
- Claude models — *RUNS_ON* — Vertex AI
- Claude models — *USES_SDK* — anthropic[vertex]
- Claude models — *DOES_NOT_USE_SDK* — google-genai
- anthropic[vertex] — *IS_A* — Anthropic SDK
- anthropic[vertex] — *INSTALL_COMMAND* — pip install "anthropic[vertex]"
- AnthropicVertex — *IS_IMPORTED_FROM* — anthropic
- AnthropicVertex — *CLIENT_PARAMETER* — project_id
- AnthropicVertex — *CLIENT_PARAMETER* — region
- google-genai — *IS_SDK_FOR* — Gemini
- claude-sonnet-5 — *IS_A* — Claude model
- claude-sonnet-5 — *DEPLOYED_ON* — Vertex AI
- claude-sonnet-5 — *SERVED_IN_REGION* — global (GCP region)
- global (GCP region) — *IS_A_TYPE_OF* — GCP
- europe-west1 (GCP region) — *IS_A_TYPE_OF* — GCP
- europe-west1 (GCP region) — *CAUSES_MODEL_NOT_FOUND_FOR* — claude-sonnet-5
- IAM — *MANAGES_ACCESS_FOR* — Vertex AI
- runtime service account — *REQUIRES_ROLE* — roles/aiplatform.user
- roles/aiplatform.user — *GRANTS_ACCESS_TO* — Vertex AI API
- Knowledge-Gathering agent — *HAS_ID* — LUZ-159671
- Distill step — *IS_PART_OF* — Knowledge-Gathering agent
- Distill step — *USES_CLIENT* — AnthropicVertex
- Distill step — *USES_MODEL* — claude-sonnet-5
- Distill step — *USES_REGION* — global (GCP region)
- Distill step — *USES_ENV_VAR* — VERTEX_PROJECT
- Distill step — *USES_ENV_VAR* — VERTEX_LOCATION
- Distill step — *USES_ENV_VAR* — VERTEX_MODEL
- a2a-sdk — *SERVES* — agent-card.json
- a2a-sdk — *SERVES* — agent.json
- agent-card.json — *REPRESENTS_NEW_FORMAT_OF* — agent card
- agent.json — *REPRESENTS_OLD_FORMAT_OF* — agent card

%% ai-graph-end %%