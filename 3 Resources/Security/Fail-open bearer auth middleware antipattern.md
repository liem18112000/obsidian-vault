---
ai_hash: e2a368870c961a24
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-19
entities: []
source: test-agent-v2 code review 2026-09-19
status: seedling
tags:
- security
- auth
- antipattern
- gotcha
- fail-closed
title: Fail-open bearer auth middleware antipattern
type: lesson
---

# Fail-open bearer auth middleware antipattern

A conditional auth gate written as `if token and header != f"Bearer {token}": return 401` **fails open**: when the expected-token env var is unset or empty, `token` is falsy, so the whole comparison is skipped and *every* request passes — silently, with no error and no log.

## Why it bites
It looks defensive (there IS a check) but the guard is conditional on the secret existing. A dropped/blank/rotated-to-empty secret, or running the container outside the config that injects it, turns the only gate off. The danger compounds when the service has **public ingress** and a **destructive surface** (an admin API exposing wipe/forget/reset).

## Fix — fail closed
- A public-ingress service with an unset/empty *expected* token must **refuse to start** (or 503 every non-health route), never serve.
- Log an ERROR at startup when a bearer/secret is expected-but-missing, so a blank secret is never silent.
- Compare with `hmac.compare_digest(got, expected)` (constant-time), not `!=`.
- Keep any permissive/no-auth path behind an explicit opt-in env (`ALLOW_INSECURE=1`) used only for local dev.

Seen in test-agent-v2 `common/adk/auth.py` and `common/bridge/asgi.py`, guarding a public admin agent that exposed `wipe-all`/`forget-memory`.

## Related
[[TRUNCATE CASCADE defeats a table allowlist]]

## Related

- [[TRUNCATE CASCADE defeats a table allowlist]]

%% ai-graph-start %%

**Related notes:**
- [[ALLOW_INSECURE=1 + blank token opens the fail-closed bearer gate]]
- [[TRUNCATE CASCADE defeats a table allowlist]]
- [[Declare + enforce bearer auth on an a2a-sdk 1.x server]]
- [[Arm a new login gate by env presence so shipping auth cannot lock the operator out]]
- [[masking-token-in-fetch-command-breaks-downstream]]

%% ai-graph-end %%