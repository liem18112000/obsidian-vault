---
title: "Expose intermediate results of an async job, not just the final output"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: Analyze API Explained (AI)"
tags: [async, jobs, api-design, partial-results, document-processing, confluence-distilled]
---

# Expose intermediate results of an async job, not just the final output

An async job API usually offers two states to the caller: *not done* and *here is everything*. When a job is a **collection of tasks**, that wastes the results that finished first.

The refinement:

> Document analysis is performed by an asynchronous job triggered by the user providing configuration and input data. The final result can be fetched when the job has finished, **but intermediate results can be accessed as soon as they are available.**

**Why exposing partial results matters:**

- **The UI can start rendering.** A document whose OCR text is ready but whose entity extraction is still running can already be displayed and searched. All-or-nothing forces the user to wait for the slowest task.
- **Downstream work can start earlier.** A consumer that only needs one of the tasks' outputs is not blocked by the others.
- **Failures become partial rather than total.** If one task fails, the results that succeeded are still retrievable — the job does not collapse to "error".

**The shape that makes it possible.** A job is *a collection of tasks* with a generated unique identifier and an explicit lifecycle of states. The caller supplies input data, **specifies which tasks to perform**, and passes configuration options controlling behaviour. Because tasks are named and independently addressable, "which parts are ready?" is a meaningful question — in a monolithic job it is not.

> [!tip] Let the caller choose the tasks
> Being able to request a subset of the available tasks is what keeps the API from becoming slower for everyone as capabilities are added. A caller that wants only the text layer should not pay for entity extraction — and a task-per-capability design gives you that for free.

> [!warning] Partial results need a completeness signal, not just data
> If a client can read intermediate output, it must be able to distinguish *"this task finished and produced nothing"* from *"this task has not run yet"*. That means per-task status alongside per-task results — otherwise consumers will treat an empty in-progress result as a final empty answer, which is the silent-wrong-answer failure mode again.

Related: [[Persist raw third-party results before mapping them to your domain shape]] · [[Score async API designs on crash recovery and multi-instance, not latency]].

Source: [[Analyze API Explained]] (AI, Confluence).

## Related

- [[Score async API designs on crash recovery and multi-instance, not latency]]
