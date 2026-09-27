---
title: "pydantic-settings JSON-parses complex fields at the source, before validators"
created: 2026-09-21
type: gotcha
status: seedling
source: "session 2026-09-21 (customer360-agent deploy)"
tags: [pydantic, pydantic-settings, fastapi, gotcha, env-config]
---

# pydantic-settings JSON-parses complex fields at the source, before validators

pydantic-settings v2 decodes complex fields (`dict`, `list`, and other non-scalar types) as **JSON inside the settings source** (`EnvSettingsSource`), *before* the model is validated. So a `@field_validator(mode="before")` cannot rescue a bad value — the failure happens in `sources/base.py` `__call__ -> source()`, not during field validation.

The practical trap: an **empty-string** env var for such a field crashes app startup. `LLM_EXTRA_CONFIG=` (present but empty) makes pydantic attempt `json.loads("")` -> `SettingsError: error parsing value for field "llm_extra_config"`, even though the field has `default_factory=dict`. A *missing* var is fine (default applies); an *empty* one is fatal.

**Fixes:**
1. Only emit the env var when it has a real value (dont write `KEY=` empty). Chosen fix in `deployments/server/deploy-agent.sh` for the customer360-agent service — the deploy script was unconditionally writing `LLM_EXTRA_CONFIG=$LLM_EXTRA_CONFIG`, which crash-looped the container.
2. Or type the field as `str` and `json.loads()` it yourself in a validator/computed property, so parsing moves out of the source and you control the empty case.

Discovered deploying customer360-agent (FastAPI + LiteLLM) onto its UAT vServer box.

## Related

- [[Python/pydantic]]
