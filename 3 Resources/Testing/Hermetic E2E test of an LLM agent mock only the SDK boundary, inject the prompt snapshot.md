---
title: "Hermetic E2E test of an LLM agent: mock only the SDK boundary, inject the prompt snapshot"
created: 2026-09-21
type: howto
status: seedling
source: "session 2026-09-21 (customer360-agent E2E)"
tags: [testing, e2e, llm, litellm, fastapi, pytest, mocking]
---

# Hermetic E2E test of an LLM agent: mock only the SDK boundary, inject the prompt snapshot

To test an LLM-backed agent end-to-end **hermetically** (no live model, no DB), mock at exactly two seams and leave everything else real:

1. **The LLM SDK boundary** — patch `litellm.completion` (the single call every provider routes through) to return an OpenAI-shaped object: `SimpleNamespace(choices=[SimpleNamespace(message=SimpleNamespace(content=<json str>))])`. The agent then runs its real parse/validate/guardrail path over that text.
2. **The prompt store** — inject the published snapshot in-memory (`get_store()._snapshot = {key: PromptTemplate(...)}`) instead of hitting the DB that `database-init` seeds in prod.

Everything between stays real: HTTP routing, bearer-token auth, request->brief mapping, prompt assembly, JSON parsing, channel guardrails, response serialization. This answers "can the agent actually be used?" — unit tests that patch the whole planner do not.

Two assertions make it a genuine E2E rather than a stub check: (a) assert the built prompt the mock received contains the seeded instruction body AND the candidate items (proves the assembly path ran); (b) assert bad/rejected LLM output maps to the intended HTTP status (e.g. 502), not a 500.

Discovered writing `customer360-agent/tests/test_e2e.py` for the customer360 AI Agent (FastAPI :8009, `/plan/email` + `/plan/zalo`).

## Related

- [[pydantic-settings JSON-parses complex fields at the source]]
- [[before validators]]
