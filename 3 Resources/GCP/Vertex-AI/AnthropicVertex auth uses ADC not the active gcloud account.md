---
ai_hash: d633677b282f70f1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-27
entities: []
source: session 2026-08-27 models/test_vertex_claude.py
status: seedling
tags:
- vertex-ai
- anthropic
- gcloud
- adc
- gotcha
- windows
title: AnthropicVertex auth uses ADC not the active gcloud account
type: lesson
---

# AnthropicVertex auth uses ADC not the active gcloud account

The Anthropic Vertex SDK (\`AnthropicVertex(project_id=, region=)\`) authenticates with **Google Application Default Credentials**, which come from google.auth.default() -> the ADC file (\`%APPDATA%\gcloud\application_default_credentials.json\`), **not** from whatever account \`gcloud auth login\` made active. These are two separate credential stores.

**Gotcha:** an ADC file can go stale while the gcloud CLI login is still fine. A stale ADC refresh token raises \`RefreshError: invalid_grant: Token has been expired or revoked\` on the first API call. Durable fix: \`gcloud auth application-default login\`.

**Workaround to use the *current* gcloud account without re-doing ADC login:** grab a short-lived token from \`gcloud auth print-access-token\` and pass it to the client:
\`AnthropicVertex(project_id=..., region=..., access_token=<token>)\`. The token is not auto-refreshed, so it is fine for a short probe (<1h), not a long-running service.

**Windows gotcha:** \`gcloud\` is \`gcloud.cmd\`; Python \`subprocess.run(["gcloud", ...])\` fails with WinError 2 unless you resolve it first via \`shutil.which("gcloud")\` (or use shell=True). Git Bash resolves it transparently, hiding the problem until you call it from Python.

Also pass \`default_headers={"x-goog-user-project": project}\` for the quota/billing project when using user ADC.

Related: [[Claude on Vertex AI availability is per-project per-region (klara-nonprod)]]

## Related

- [[Claude on Vertex AI availability is per-project per-region (klara-nonprod)]]

%% ai-graph-start %%

**Related notes:**
- [[Claude Code runs on Vertex AI via three env vars with gcloud ADC]]
- [[Mounting host gcloud ADC into a container to authenticate Vertex AI]]
- [[Anthropic has no third-party OAuth; in-app Claude login means driving the claude auth CLI]]
- [[List Anthropic models on Vertex via the publisherModels REST endpoint]]
- [[Claude on Vertex AI uses anthropic[vertex], not google-genai]]

%% ai-graph-end %%