---
title: "TPD test_kinds must be additive over the base four, not replace them"
created: 2026-09-16
type: lesson
status: seedling
source: "test-agent-v2, LUZ-158230, 2026-09-16"
tags: [testing-agent, implement-plan, test-kinds, root-cause, bugfix]
---

# TPD test_kinds must be additive over the base four, not replace them

Root cause of the LUZ-158230 all-"performance" garbage suite (41 scenarios, 0 happy/0 negative), and the fix.

**The bug:** the implement stage resolved "which test kinds to generate" as `plan.test_kinds or DEFAULTS` in four places (heuristic scenario gen, the LLM scenarios prompt, the coverage matrix, the HTML report). That is REPLACE-semantics: any non-empty test_kinds REPLACES the base four (happy/negative/boundary/error) instead of extending them. So when test_kinds got mangled to a single kind, the whole suite dropped the four.

**How test_kinds got mangled:** `implement_plan(guidance=...)` folds guidance-scraped EXTRA kinds into plan.test_kinds on EVERY call (pipeline.py). With test_kinds empty and a steer text mentioning "performance" (my step-oracle answer said "concurrent import calls" / "Performance (API-only)"), the fold-in set test_kinds = ["performance"]. Replace-semantics then generated ONLY performance. (Compounded by the LLM generator degrading to the heuristic fallback, so the per-node stub generator ran.)

**The fix (once-and-for-all):** a single resolver `common.testplan.models.effective_kinds(plan)` returning the base four UNION the elicited extras (additive — the four are a seed the open taxonomy EXTENDS, never replaces); happy-only via metrics is the one intentional collapse. Wired into all four sites. Now even a mangled single-kind test_kinds yields happy/negative/boundary/error + that kind — the catastrophic collapse is impossible for any ticket. Coverage matrix now uses the same additive set as generation, so it cannot under/over-count.

**Design principle:** the kind taxonomy is OPEN and ADDITIVE. `test_kinds or defaults` (replace) is the anti-pattern; union the defaults. In practice `kinds_from_answer` already returns defaults+extras, so real plans were unaffected — only a mangled/collapsed test_kinds triggered it.

**Still open (separate):** the LLM scenario generator itself degraded (timeout/exception → heuristic). Needs the deployed Cloud Run logs to tell timeout (raise TPD_GEN_TIMEOUT_S) vs exception. And the case-design ingest maps a multi-extra answer to a single option (only one extra survives). Both are lower severity than the defaults-drop.

Related: [[implement_plan heuristic-fallback emits one performance stub per node]] · [[LUZ-158230 ePost ZIP import - test scope decisions]]

## Related

- [[implement_plan heuristic-fallback emits one performance stub per node]]
- [[LUZ-158230 ePost ZIP import - test scope decisions]]
