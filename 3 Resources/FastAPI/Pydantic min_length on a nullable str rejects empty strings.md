---
ai_hash: 719d4364ed41defa
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-05
entities: []
source: c360 python code review 2026-09-05
status: seedling
tags:
- pydantic
- validation
- gotcha
title: Pydantic min_length on a nullable str rejects empty strings
type: lesson
---

# Pydantic min_length on a nullable str rejects empty strings

In Pydantic, `field: str | None = Field(default=None, min_length=1)` still runs `min_length` validation when the value is an empty string, because `""` is not `None`. A client sending `session_id=""` (very common before a session exists) fails validation, and if that field is on a batch request the whole batch is 422-rejected and dropped.

When empty means "absent", coerce `""` to `None` in a `field_validator(mode="before")` (or accept empty explicitly) rather than relying on `min_length` to tolerate it.

## Related

- [[FastAPI request-size guards inside the handler run after the body is in RAM]]

%% ai-graph-start %%

**Related notes:**
- [[FastAPI request-size guards inside the handler run after the body is in RAM]]
- [[pydantic-settings JSON-parses complex fields at the source, before validators]]

%% ai-graph-end %%