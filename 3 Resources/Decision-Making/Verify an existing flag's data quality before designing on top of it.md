---
ai_hash: b34ac87d9a201b72
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Evaluation of final solution including implementation needs (AI)'
status: seedling
tags:
- data-quality
- design-review
- reuse
- access-control
- confluence-distilled
title: Verify an existing flag's data quality before designing on top of it
type: lesson
---

# Verify an existing flag's data quality before designing on top of it

"There's already a flag for that" is the cheapest-looking solution in any design discussion, and it is only cheap if the flag's **data** is correct. Existing fields accumulate drift, and inheriting that drift means inheriting a bug you did not write.

A worked example. A proposal to gate a feature on an existing `allowScannedLetter` flag, evaluated honestly:

**Why:**
- **No new access required** — the consuming service already reads tenant addresses and this flag.
- **It is a simple boolean.**
- **It has precedent** — previously used, together with subscription status, to build the address list for an earlier solution.
- **It already encodes the business rule** — including a 5-day buffer at the end of a subscription.

**Why not:**
- **Data-quality issues were found**: *non-subscribed tenants still had the flag set to `true`* — apparently a synchronisation problem, or stale data.
- Some tenants have the flag set by a **manual process**.

So the flag means the right thing *by design* and the wrong thing *for some rows*. Depending on it without fixing the synchronisation means a feature that grants access to tenants who should not have it — a permission bug, not a data-tidiness issue.

> [!tip] Before reusing a field, query it against reality
> Run the cross-check: *how many rows have this flag set but fail the condition it is supposed to represent?* One query turns "we can just use the existing flag" from an assumption into a number. If the number is not zero, the design has a prerequisite — fix the source, or add a second condition — and that prerequisite belongs in the estimate.

> [!warning] A manually-maintained field has no guarantee at all
> Where some tenants get the flag through a manual process, the field's correctness depends on someone remembering. That is fine for a report and not fine for an access-control decision.

**The template is worth stealing too.** Each candidate solution written as **Why? / Why not? / Decision / Implementation**, with the decision left explicitly as `tbd` until taken. Forcing a "why not" section per option is what surfaced the data-quality problem — an evaluation that only lists advantages will always pick the cheapest-looking option.

Related: [[A nullable column with no constraint becomes an NPE in a downstream service]] — the same theme: an existing column's real contents are not its declared meaning.

Source: [[Evaluation of final solution including implementation needs]] (AI, Confluence).

## Related

- [[A nullable column with no constraint becomes an NPE in a downstream service]]

%% ai-graph-start %%

**Related notes:**
- [[Idempotency guards keyed on object presence break when hydration materializes the object]]
- [[A read filtered on a value no writer produces fails by returning empty]]
- [[Test the decisions that override the story description, not the description]]
- [[Implementation is the best reviewer a design doc gets]]
- [[A nullable column with no constraint becomes an NPE in a downstream service]]

%% ai-graph-end %%