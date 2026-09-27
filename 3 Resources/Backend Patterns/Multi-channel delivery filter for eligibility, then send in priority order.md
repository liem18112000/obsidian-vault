---
title: "Multi-channel delivery: filter for eligibility, then send in priority order"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: OneAPI Architecture overview (LUZ)"
tags: [fallback, routing, notifications, retry, design-pattern, confluence-distilled]
---

# Multi-channel delivery: filter for eligibility, then send in priority order

When a message can go out by several channels — ePost, email, SMS, physical letter — resist writing one routine that decides and sends. Split it into **filter** then **send**, and the fallback behaviour falls out for free.

The design, for a delivery where the sender left the channel preference empty or set it to `AUTO`:

**Filter step.** Validate the document's metadata and files against **each channel's rules**. Output: the list of channels this particular document is *eligible* for. A 40 MB PDF is not eligible for SMS; a recipient with no verified postal address is not eligible for physical letter.

**Send step.** Walk the eligible channels **in a pre-defined order**, stopping at the first success. If all fail, the delivery fails.

**What the separation buys:**

- **Eligibility is a pure function.** Given the document and recipient, the eligible set is computable with no side effects — so it is trivially testable, and you can show a sender *why* a channel was excluded without attempting a send.
- **Ordering becomes policy, not code.** The sequence is configuration ("the pre-defined channels order"), so changing priority is not a code change in the sending logic.
- **Failure has two distinct meanings.** *Not eligible* is a permanent, explainable condition. *Eligible but failed* is transient and worth retrying. A single combined routine collapses these into one error and usually retries the un-retryable.

> [!tip] The generalisable shape
> Any "try these options until one works" problem benefits from the same split: **compute the candidate set, then iterate it.** Payment methods, storage backends, notification transports, LLM providers. The version that filters and sends in one loop always ends up with eligibility checks tangled into the retry logic.

> [!warning] First-success ordering hides systematic failures
> If channel #1 quietly fails for a whole class of recipients, everything silently lands on channel #2 and the system looks healthy — until the bill for channel #2 arrives, or #2 degrades too. Record **which channel actually delivered** and alert on shifts in the distribution, not just on total failures. Pair with [[Record origin and origin_href so a downstream row traces back to its cause]], which is what makes that attribution possible after the fact.

Source: [[OneAPI Architecture overview]] (LUZ, Confluence).

## Related

- [[Record origin and origin_href so a downstream row traces back to its cause]]
