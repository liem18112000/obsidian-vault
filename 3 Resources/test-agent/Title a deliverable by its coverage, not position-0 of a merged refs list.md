---
ai_hash: 15b66ed172dbbef1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-21
entities: []
source: session 2026-09-21 (run-8eafe7ea QA/QC export test)
status: seedling
tags:
- test-agent
- qa
- gotcha
- reporting
- design-principle
title: Title a deliverable by its coverage, not position-0 of a merged refs list
type: lesson
---

# Title a deliverable by its coverage, not position-0 of a merged refs list

A deliverable that aggregates many references (a report, a plan, a summary) should derive its **title / primary entity from what it actually covers** — the dominant subject of its content — never from position-0 of a concatenated reference list. `keys[0]` of a merged refs list is a fragile proxy for "the subject": the first entry can be an out-of-scope or tangential reference.

## The concrete bug that taught this
`test-agent-v2` `common/testplan/report/html.py` (`build_report_html`) set the report title with:

```python
keys = all_jira_keys([context_id], plan.source_refs, *[d.source_refs for d in decisions])
ticket = keys[0] if keys else context_id
```

`all_jira_keys` collects keys in **insertion order** across the merged lists. So the title took the first Jira key seen while scanning plan + decision `source_refs`. In run-8eafe7ea the title rendered **LUZ-158243** — a ticket the plan explicitly lists as *out of scope* — instead of **LUZ-158230**, which every one of the 39 generated scenarios actually covers.

## The fix (root cause, single render seam)
Derive the title from what the scenarios cover — the mode of their coverage keys — with a fallback chain:

```python
from collections import Counter
covered = Counter(k for s in scenarios for k in all_jira_keys(s.source_refs))
ticket = covered.most_common(1)[0][0] if covered else (keys[0] if keys else context_id)
```

Fix it once at the render seam, not per-caller. Reuses the existing `all_jira_keys` helper — no new abstraction.

## Testing gotcha
This class of bug is **invisible to a single-ticket fixture** — with only one Jira key, `keys[0]` is trivially correct. The regression test must seed `plan.source_refs` with an *out-of-scope key ordered first* and scenarios covering a *different* key, then assert the title names the covered one.

## Related
[[test-agent common shared engine]]

## Related

- [[test-agent common shared engine]]

%% ai-graph-start %%

**Related notes:**
- [[TPD assured-loop judge penalizes cross-run scenario duplication]]
- [[TPD test_kinds must be additive over the base four, not replace them]]
- [[TPD methodology list had dupes + substring false-positives]]
- [[test-agent-v2 always-enriched HTML report generator]]
- [[Testing-Agent refine confidence is capped by un-ingested spec PDFs]]

%% ai-graph-end %%