---
ai_hash: bcd67004dbe30206
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Scan to book booking exception (Helios)'
status: seedling
tags:
- integration
- error-handling
- exceptions
- api-contracts
- confluence-distilled
title: Enumerate a dependency's exception surface and decide each one before integrating
type: lesson
---

# Enumerate a dependency's exception surface and decide each one before integrating

Before integrating with a service, enumerate **every exception it can throw** and decide, per exception, what your caller does with it. Writing that list is a design activity, not documentation — most of the decisions are product decisions, and they are invisible until the list exists.

The artefact is plain: for one endpoint, the exceptions traced through the call tree with the message each produces.

```
POST /luz-accounting/api/.../companies/{companyId}/business-cases?isBooking=true

BusinessCaseService.handleBusinessCase
  businessCaseTemplateService.findEntityById     → NotFoundException
  validateBusinessCaseInput
    ValidatorHelper.validate                     → ValidationException
    validateBusinessCase
      validateBusinessCaseOperators              → ValidationException
        "Business case with OR operator should choose one snippet…"
```

with an explicit column: *decide which exception to handle in the calling app*.

**What the exercise surfaces:**

- **Which failures are the user's fault and which are yours.** A `ValidationException` about choosing a snippet is actionable by the user; a `NotFoundException` on a template id is a data or configuration problem they cannot fix, and should not be shown as though they can.
- **Which messages are safe to display.** Upstream exception text is written for developers. Some of it is a usable user message, most is not, and some leaks internals. Deciding per exception is how you avoid the default of rendering `e.getMessage()` everywhere.
- **Which failures you deliberately do not handle.** An explicit "not handled — falls through to the generic error" is a decision. Silence is not.

> [!tip] Trace the call tree, not just the signature
> The valuable part is that the exceptions are listed **with the path that produces them** — two different `ValidationException`s from two different validators need different handling, and a method signature saying `throws ValidationException` tells you nothing about which. Walking the call tree is what turns one exception type into the several distinct user-facing situations it actually represents.

> [!warning] This list goes stale silently
> A new validation added upstream produces a new exception your caller has never seen, and the default path swallows it into a generic error. The list is a snapshot unless the boundary is tested — pair it with tests that assert the caller's behaviour for each enumerated case, so an unhandled addition shows up as a gap rather than as a vague error in production.

Related: [[Throw error codes not sentences; localise at the request boundary]] — what the upstream service should have done to make this easier.

Source: [[Scan to book - booking exception]] (Helios, Confluence).

## Related

- [[Throw error codes not sentences; localise at the request boundary]]

%% ai-graph-start %%

**Related notes:**
- [[Throw error codes not sentences; localise at the request boundary]]
- [[Handle error exception]]
- [[Scan to book - booking exception]]
- [[Re-wrapping a 5xx as 4xx defeats status-based retry]]
- [[Mutually exclusive API parameters should be rejected, not resolved by precedence]]

%% ai-graph-end %%