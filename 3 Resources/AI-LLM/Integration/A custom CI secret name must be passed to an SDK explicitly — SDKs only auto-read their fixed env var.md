---
title: "A custom CI secret name must be passed to an SDK explicitly — SDKs only auto-read their fixed env var"
created: 2026-09-05
type: gotcha
status: seedling
source: "session 2026-09-05 (docs-vector-search OpenAI switch, LEO_OPENAI_* names)"
tags: [openai, sdk, github-actions, secrets, config, gotcha]
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
