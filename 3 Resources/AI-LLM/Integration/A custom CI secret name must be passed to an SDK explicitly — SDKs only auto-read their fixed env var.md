---
ai_hash: 44911eff57c42714
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-05
entities:
- CI secret name
- SDK
- environment variable
- OpenAI SDK
- Anthropic
- OPENAI_API_KEY
- ANTHROPIC_API_KEY
- LEO_OPENAI_API_KEY
- OpenAI client
- auth error
- GitHub Actions
- GitHub Actions secret
- GitHub Actions variable
- LEO_OPENAI_MODEL_NAME
- CHAT_MODEL
- gpt-4.1
- OpenAI structured output
- JSON schema
- Anthropic embeddings endpoint
- api_key parameter
source: session 2026-09-05 (docs-vector-search OpenAI switch, LEO_OPENAI_* names)
status: seedling
tags:
- openai
- sdk
- github-actions
- secrets
- config
- gotcha
title: A custom CI secret name must be passed to an SDK explicitly — SDKs only auto-read
  their fixed env var
type: gotcha
---

# A custom CI secret name must be passed to an SDK explicitly — SDKs only auto-read their fixed env var

Provider SDKs auto-read **one fixed environment variable** for their key — the OpenAI SDK reads `OPENAI_API_KEY`, Anthropic reads `ANTHROPIC_API_KEY`. If your CI stores the key under a **different name** (e.g. a namespaced secret like `LEO_OPENAI_API_KEY`), a zero-arg client (`OpenAI()`) will NOT find it and fails with an auth error even though the secret "is set".

## Fix — read the custom name yourself and pass it in
```python
OPENAI_API_KEY = os.getenv("LEO_OPENAI_API_KEY") or os.getenv("OPENAI_API_KEY")
client = OpenAI(api_key=OPENAI_API_KEY)   # api_key=None still falls back to the SDK default env
```
Same idea for a namespaced model **variable**: `os.getenv("LEO_OPENAI_MODEL_NAME") or os.getenv("CHAT_MODEL", "gpt-4.1")`. Keep the standard name as a fallback so local dev (which usually exports the standard var) still works.

## In GitHub Actions, expose secret + variable as env
Secrets and repo/org variables are separate namespaces: `${{ secrets.NAME }}` vs `${{ vars.NAME }}`.
```yaml
    env:
      LEO_OPENAI_API_KEY:    ${{ secrets.LEO_OPENAI_API_KEY }}
      LEO_OPENAI_MODEL_NAME: ${{ vars.LEO_OPENAI_MODEL_NAME }}
```

## Related — OpenAI structured output for extraction
OpenAI forces JSON to a schema via `response_format={"type":"json_schema","json_schema":{"name":..,"schema":..,"strict":True}}`; strict mode requires `additionalProperties:false` and every property in `required`.

Related: [[Anthropic has no first-party embeddings endpoint]]

## Related

- [[Anthropic has no first-party embeddings endpoint]]

%% ai-graph-start %%

**Related notes:**
- [[GitHub secrets are write-only; run in Actions to use a CD key, push-trigger a feature branch to avoid main]]
- [[GitHub Actions 'secret is not set' usually means a name mismatch - verify with gh secret list]]
- [[OpenAI request gotchas 8192-token embedding limit and max_completion_tokens]]
- [[CD secrets must be wired into cd.yml deploy step env, not just added to GitHub]]
- [[Anthropic has no first-party embeddings endpoint]]

**Relations:**
- CI secret name — *must be passed to* — SDK
- SDK — *auto-reads* — environment variable
- OpenAI SDK — *reads* — OPENAI_API_KEY
- Anthropic — *reads* — ANTHROPIC_API_KEY
- LEO_OPENAI_API_KEY — *is a custom* — CI secret name
- OpenAI client — *does not auto-read* — LEO_OPENAI_API_KEY
- OpenAI client — *fails with* — auth error
- GitHub Actions — *exposes* — GitHub Actions secret
- GitHub Actions — *exposes* — GitHub Actions variable
- GitHub Actions secret — *as* — environment variable
- GitHub Actions variable — *as* — environment variable
- LEO_OPENAI_API_KEY — *is a* — GitHub Actions secret
- LEO_OPENAI_MODEL_NAME — *is a* — GitHub Actions variable
- OpenAI client — *accepts* — api_key parameter
- api_key parameter — *can be set from* — LEO_OPENAI_API_KEY
- api_key parameter — *can be set from* — OPENAI_API_KEY
- LEO_OPENAI_MODEL_NAME — *is a namespaced* — environment variable
- CHAT_MODEL — *is a standard* — environment variable
- gpt-4.1 — *is a default for* — CHAT_MODEL
- OpenAI structured output — *uses* — JSON schema
- Anthropic — *has no* — Anthropic embeddings endpoint

%% ai-graph-end %%