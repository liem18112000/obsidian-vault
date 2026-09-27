---
title: "Fix TPD scenario generator truncation — raise max_tokens, keep one call"
created: 2026-09-16
type: lesson
status: seedling
source: "Testing-Agent run-188f96b8"
tags: [testing-agent, implement-plan, max-tokens, bugfix, assured-loop]
---

# Fix TPD scenario generator truncation — raise max_tokens, keep one call

**Root cause (confirmed in code)** of the TPD `implement_plan` empty-step / 0.08 fallback: the scenario generator `claude_scenarios` (`test-agent-v2/src/test_plan_definition/implement/generate/llm.py`) makes ONE LLM call capped at `max_tokens=6000`, while `scenarios_prompt` (`common/testplan/llm/prompts.py`) instructs "cover every behaviour×kind, NO cap, aim for 100%". On a rich pack the JSON array overruns 6000 tokens → truncated → `Scenarios(**data)` pydantic validation raises → the exception is swallowed in `run_json_agent` (`common/testplan/llm/adk.py`, `except Exception → return None`) → `claude_scenarios` returns None → the assured loop degrades to `heuristic_scenarios` (empty-step, one per pack-node × 4 default kinds). The judge scores it ~0.08 and, because the heuristic ignores `reflections`/`guidance`, every round regenerates the identical set → frozen.

**Fix applied (working tree):** raised the scenario generator cap to `_SCEN_MAX_TOKENS = 16000` (one call, so the I3 / call-count contract in `test_plan_implement.py` still holds — generate=1, +judge=2, +steps=4 on detail). 

**Rejected approach:** chunking generation one-kind-per-call. It fixes truncation but multiplies LLM calls and BROKE 7 call-count/fallback tests (`test_implement_default_makes_two_llm_calls_generate_plus_judge` et al.). The call-count contract is load-bearing — keep generate to a single call; raise the cap instead of batching.

**Two caveats:** (1) the fix is working-tree only — the DEPLOYED Cloud Run TPD agent runs the old image, so `implement_plan` via the MCP gateway keeps falling back until a redeploy (build+apply the tpd image). (2) SECONDARY bug still open: user-chosen extra kinds (security/concurrency/i18n) never reached `plan.test_kinds` (case-design answer-not-persisted), so even post-fix the LLM path emits only the 4 seed kinds until that is fixed.

## Related

- [[Testing-Agent implement_plan silent heuristic fallback = per-node x kind empty-step scenarios]]
