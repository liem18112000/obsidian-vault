---
ai_hash: bc0de4fedd1ab6d0
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-21
entities: []
source: session 2026-09-21 (agent live-eval)
status: seedling
tags:
- github-actions
- secrets
- ci-cd
- workflow-dispatch
- gh-cli
title: GitHub secrets are write-only; run in Actions to use a CD key, push-trigger
  a feature branch to avoid main
type: howto
---

# GitHub secrets are write-only; run in Actions to use a CD key, push-trigger a feature branch to avoid main

GitHub Actions secrets are **write-only**: you can set them (`gh secret set`) and list their names (`gh secret list`), but there is no API/CLI to read a value back. So a CD-only key (e.g. `LEO_OPENAI_API_KEY`) cannot be pulled to a laptop — the only place it is readable is inside a workflow run, injected as `${{ secrets.NAME }}`.

To actually USE such a key: run the work AS a workflow. Job sets `env: OPENAI_API_KEY: ${{ secrets.LEO_OPENAI_API_KEY }}`, runs the script, and uploads the result with `actions/upload-artifact`; fetch it locally with `gh run download <id> -n <name>`. The value never touches the transcript or local disk in plaintext.

To trigger it WITHOUT merging to main: `workflow_dispatch` alone is not enough — a dispatchable workflow must exist on the default branch to appear/dispatch. Instead use a `push` trigger scoped to the feature branch (`on: push: branches: [feat/...]`, optionally `paths:`); the commit that adds the workflow file itself fires the run, on the feature branch, no main involvement. CD stays inert because deploy pipelines gate on `main` / `v*` tags.

Discovered running a live LLM eval for customer360-agent via GitHub Actions using the CD OpenAI key.

%% ai-graph-start %%

**Related notes:**
- [[CD secrets must be wired into cd.yml deploy step env, not just added to GitHub]]
- [[A custom CI secret name must be passed to an SDK explicitly — SDKs only auto-read their fixed env var]]
- [[GHCR-always plus Docker Hub-optional GitHub Actions publishing pattern]]
- [[GitHub Actions 'secret is not set' usually means a name mismatch - verify with gh secret list]]
- [[workflow_dispatch Run button only appears on the default branch - use gh workflow run --ref to dispatch from a feature branch]]

%% ai-graph-end %%