---
ai_hash: 80edc8d2ac421aba
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-16
entities:
- TPD scenario generator
- max_tokens
- TPD
- implement_plan
- claude_scenarios
- test-agent-v2/src/test_plan_definition/implement/generate/llm.py
- LLM call
- scenarios_prompt
- common/testplan/llm/prompts.py
- JSON array
- 6000 tokens
- Scenarios
- pydantic validation
- Exception
- run_json_agent
- common/testplan/llm/adk.py
- None
- heuristic_scenarios
- judge
- reflections
- guidance
- _SCEN_MAX_TOKENS
- '16000'
- I3 / call-count contract
- test_plan_implement.py
- generate
- steps
- chunking generation
- one-kind-per-call
- 7 call-count/fallback tests
- test_implement_default_makes_two_llm_calls_generate_plus_judge
- working-tree
- DEPLOYED Cloud Run TPD agent
- old image
- MCP gateway
- redeploy
- tpd image
- SECONDARY bug
- user-chosen extra kinds
- plan.test_kinds
- case-design answer-not-persisted
- LLM path
- 4 seed kinds
- Testing-Agent implement_plan silent heuristic fallback = per-node x kind empty-step
  scenarios
source: Testing-Agent run-188f96b8
status: seedling
tags:
- testing-agent
- implement-plan
- max-tokens
- bugfix
- assured-loop
title: Fix TPD scenario generator truncation — raise max_tokens, keep one call
type: lesson
---

# Fix TPD scenario generator truncation — raise max_tokens, keep one call

**Root cause (confirmed in code)** of the TPD `implement_plan` empty-step / 0.08 fallback: the scenario generator `claude_scenarios` (`test-agent-v2/src/test_plan_definition/implement/generate/llm.py`) makes ONE LLM call capped at `max_tokens=6000`, while `scenarios_prompt` (`common/testplan/llm/prompts.py`) instructs "cover every behaviour×kind, NO cap, aim for 100%". On a rich pack the JSON array overruns 6000 tokens → truncated → `Scenarios(**data)` pydantic validation raises → the exception is swallowed in `run_json_agent` (`common/testplan/llm/adk.py`, `except Exception → return None`) → `claude_scenarios` returns None → the assured loop degrades to `heuristic_scenarios` (empty-step, one per pack-node × 4 default kinds). The judge scores it ~0.08 and, because the heuristic ignores `reflections`/`guidance`, every round regenerates the identical set → frozen.

**Fix applied (working tree):** raised the scenario generator cap to `_SCEN_MAX_TOKENS = 16000` (one call, so the I3 / call-count contract in `test_plan_implement.py` still holds — generate=1, +judge=2, +steps=4 on detail). 

**Rejected approach:** chunking generation one-kind-per-call. It fixes truncation but multiplies LLM calls and BROKE 7 call-count/fallback tests (`test_implement_default_makes_two_llm_calls_generate_plus_judge` et al.). The call-count contract is load-bearing — keep generate to a single call; raise the cap instead of batching.

**Two caveats:** (1) the fix is working-tree only — the DEPLOYED Cloud Run TPD agent runs the old image, so `implement_plan` via the MCP gateway keeps falling back until a redeploy (build+apply the tpd image). (2) SECONDARY bug still open: user-chosen extra kinds (security/concurrency/i18n) never reached `plan.test_kinds` (case-design answer-not-persisted), so even post-fix the LLM path emits only the 4 seed kinds until that is fixed.

## Related

- [[Testing-Agent implement_plan silent heuristic fallback = per-node x kind empty-step scenarios]]

%% ai-graph-start %%

**Related notes:**
- [[run_json_agent needed a per-call timeout or a slow Vertex call hangs implement past the server ceiling]]
- [[TPD IMPLEMENT makes one LLM call by default (scenarios only)]]
- [[TPD test_kinds must be additive over the base four, not replace them]]
- [[Testing-Agent implement_plan silent heuristic fallback = per-node x kind empty-step scenarios]]
- [[Deployed TPD implement_plan trips Cloud Run liveness (event-loop blocked by Vertex gen)]]

**Relations:**
- TPD scenario generator — *causes* — truncation
- TPD — *has_component* — implement_plan
- implement_plan — *experiences* — empty-step / 0.08 fallback
- claude_scenarios — *is_a* — TPD scenario generator
- claude_scenarios — *is_defined_in* — test-agent-v2/src/test_plan_definition/implement/generate/llm.py
- claude_scenarios — *makes* — LLM call
- LLM call — *is_capped_at* — max_tokens=6000
- scenarios_prompt — *is_defined_in* — common/testplan/llm/prompts.py
- scenarios_prompt — *instructs* — NO cap
- JSON array — *overruns* — 6000 tokens
- JSON array — *becomes* — truncated
- truncated — *causes* — pydantic validation
- pydantic validation — *raises_for* — Scenarios
- Exception — *is_swallowed_by* — run_json_agent
- run_json_agent — *is_defined_in* — common/testplan/llm/adk.py
- run_json_agent — *returns* — None
- claude_scenarios — *returns* — None
- claude_scenarios returns None — *leads_to* — heuristic_scenarios
- heuristic_scenarios — *is_a* — empty-step / 0.08 fallback
- judge — *scores* — heuristic_scenarios
- heuristic_scenarios — *ignores* — reflections
- heuristic_scenarios — *ignores* — guidance
- Fix — *raises_cap_to* — _SCEN_MAX_TOKENS = 16000
- _SCEN_MAX_TOKENS — *is_set_to* — 16000
- Fix — *maintains* — one call
- I3 / call-count contract — *is_defined_in* — test_plan_implement.py
- I3 / call-count contract — *requires_generate_calls* — 1
- I3 / call-count contract — *requires_judge_calls* — 2
- I3 / call-count contract — *requires_steps_calls* — 4
- chunking generation — *is_a* — Rejected approach
- chunking generation — *is* — one-kind-per-call
- chunking generation — *multiplies* — LLM calls
- chunking generation — *broke* — 7 call-count/fallback tests
- 7 call-count/fallback tests — *includes* — test_implement_default_makes_two_llm_calls_generate_plus_judge
- call-count contract — *is* — load-bearing
- Fix — *is_applied_to* — working-tree
- DEPLOYED Cloud Run TPD agent — *runs* — old image
- implement_plan — *via* — MCP gateway
- implement_plan — *experiences* — falling back
- redeploy — *is_needed_for* — DEPLOYED Cloud Run TPD agent
- SECONDARY bug — *is* — open
- user-chosen extra kinds — *do_not_reach* — plan.test_kinds
- case-design answer-not-persisted — *is_cause_of* — user-chosen extra kinds
- LLM path — *emits* — 4 seed kinds
- LLM path — *will_emit_more_kinds_after* — SECONDARY bug
- Testing-Agent implement_plan silent heuristic fallback = per-node x kind empty-step scenarios — *is_related_to* — empty-step / 0.08 fallback

%% ai-graph-end %%