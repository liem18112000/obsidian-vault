---
title: "claude-proxy login expires mid-session -> implement silently degrades to heuristic stubs (score 0.00)"
created: 2026-09-23
type: lesson
status: seedling
source: "session 2026-09-23"
tags: [claude-proxy, login, subscription, heuristic, testing-agent, gotcha]
---

# claude-proxy login expires mid-session -> implement silently degrades to heuristic stubs (score 0.00)

GOTCHA (local claude-proxy): the Claude SUBSCRIPTION login inside claude-proxy can EXPIRE mid-session (long session / heavy use). When it does, `claude -p` returns is_error:true, result "Not logged in · Please run /login", terminal_reason:"api_error", 0 tokens. The agent LLM calls then all get an error string instead of JSON, so: (1) scenario/scope/judge JSON parsing fails — e.g. `pydantic ValidationError for JudgeVerdict ... json_invalid`, `DynamicNodeFailError: Dynamic node tpd_scenario_judge failed`; (2) implement_plan silently DEGRADES to the heuristic fallback — hallmark output: hundreds of templated stubs ("<node title> — happy path / security case / i18n case" across the WHOLE unclassified pack incl. images/PDFs), coverage 100% mechanically, score 0.00, "no judge configured — single unscored pass · generation degraded to the heuristic fallback (LLM timed out / unconfigured)". Looks like a code regression but it is just the login. DIAGNOSE: `docker exec <claude-proxy> claude -p ok --output-format json` and read is_error/result/terminal_reason. FIX: re-login — `docker compose exec claude-proxy claude` then /login (persists in the claude_config volume). NOTE the down/up preserves the volume but the OAuth token still ages out. See [[Use a local Claude subscription from Docker via a claude-CLI OpenAI proxy]].

## Related

- [[Use a local Claude subscription from Docker via a claude-CLI OpenAI proxy]]
