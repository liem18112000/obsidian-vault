---
ai_hash: 6218ac5a655ea8c8
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: Distilled from the Confluence export, 2026-09-27
status: evergreen
tags:
- confluence
- moc
- distilled
- kepler
title: Confluence Export — What I Learned
type: moc
---

# Confluence Export — What I Learned

The digested half of the Confluence import: **59 atomic notes** mined from the 167 source pages in [[Confluence Export — Index]].

> [!tip] Source vs distilled
> [[Confluence Export — Index]] holds the **pages as written** — full prose, screenshots, tables, links back to the live wiki.
> This page holds **one idea per note**, rewritten to stand alone and filed by PARA. When a note cites a page, its `source:` names it.

## Tamper-evident audit logs

*The tension between integrity and throughput, and how Merkle checkpoints resolve it.*

- [[A hash chain proves integrity but not authorship, so a database admin can silently rebuild it]] — `argument`
- [[A hash-chained audit log cannot be written in parallel]] — `concept`
- [[A Merkle tree proves one item belongs to a set without revealing or transferring the set]] — `concept`
- [[Hash chains order by linkage, not by time, so backdated entries still verify]] — `argument`
- [[LUZ Audit spent 5 database operations per log entry, ~47ms, from chain maintenance]] — `observation`
- [[LUZ Audit went from 23 to 778 logs per second by dropping the REST hop to MongoDB]] — `observation`
- [[Merkle checkpoints restore parallel writes to a hash-chained audit log]] — `model`
- [[Per-record signatures prove authenticity but not completeness, so deletion and reordering go undetected]] — `argument`
- [[RFC 3161 timestamps outsource the time claim to a party the attacker does not control]] — `term`

## Running services reliably

*What actually broke in production, and the probe/shutdown mechanics behind it.*

- [[A rolling deploy drops in-flight requests unless preStop outlives endpoint propagation]] — `lesson`
- [[An exec-cat readiness probe reports Ready before the server can serve]] — `lesson`
- [[ClamAV definition updates restart clamd, so scheduled 503s are expected not broken]] — `observation`
- [[Error volume and error severity are independent, so triage by impact not by count]] — `lesson`
- [[Every production FAILED_TO_STORE traced back to a rolling deploy, not to load]] — `observation`

## MongoDB query behaviour

*Where indexes stop helping, and the three ways one gets silently ignored.*

- [[A MongoDB text index matches stemmed words, not substrings]] — `concept`
- [[A string index is silently skipped when its collation differs from the query's]] — `lesson`
- [[An index only helps an aggregation before the first group, unwind, or lookup]] — `concept`
- [[Casting a field inside $match makes it un-indexable - $toString on _id cost 16 seconds]] — `lesson`

## Performance & capacity

*Cloud Run scaling limits, and asking who consumes a result before optimising it.*

- [[Check who consumes a result before optimising it - eArchive counted 128k docs for a boolean]] — `lesson`
- [[Cloud Run concurrency capacity is an upper bound CPU rarely lets you reach]] — `lesson`
- [[Cold starts appear as a p95 spike during the scale-up ramp, not in steady state]] — `lesson`
- [[Hitting Cloud Run maxScale turns latency into compounding errors, and 2xx throughput falls]] — `lesson`

## Agent & LLM architecture

*Harness engineering, multi-agent trade-offs, swarm coordination, skills as assets.*

- [[A code knowledge graph answers impact questions that embedding search cannot]] — `concept`
- [[A complete skill has five layers - intent, knowledge, execution, verification, evolution]] — `model`
- [[A prompt is a temporary instruction, a skill is an encapsulated capability]] — `concept`
- [[A shared mutable context beats a message bus for sequentially orchestrated agents]] — `argument`
- [[Agent equals model plus harness, and the harness is the engineering discipline]] — `term`
- [[Few-shot cost grows linearly while accuracy flattens, so escalate examples in steps]] — `lesson`
- [[Incremental graph builds hash-skip unchanged files and re-parse only their dependents]] — `lesson`
- [[LLM text watermarking biases free token choices with a keyed g-function]] — `concept`
- [[Multi-agent systems trade a single capable agent for specialisation, isolation and parallelism]] — `concept`
- [[Prompt, context, and harness engineering nest rather than replace each other]] — `model`
- [[ReAct beats plan-then-execute when the environment can surprise the agent]] — `argument`
- [[Stigmergy coordinates through traces left in the environment, not messages between agents]] — `term`
- [[Swarm intelligence gets global behaviour from local rules and no central controller]] — `concept`
- [[Thinking tokens are billed as output, so effort level is a cost lever]] — `lesson`

## Knowledge systems

*Why knowledge loses meaning when moved, and how layered context should compose.*

- [[Intent composes up the chain, specification stays a leaf property]] — `model`
- [[Knowledge loses meaning when copied out of the position where it was learned]] — `argument`
- [[One chain filename at every altitude lets a reader orient by walking to the root]] — `model`

## Testing

*Oracles decide whether a suite catches anything; matrix ordering decides how fast you learn.*

- [[A test oracle is what decides pass or fail, and without one a test is just a script]] — `term`
- [[Oracle strength can be graded statically from the expected-result text]] — `howto`
- [[Oracle strength, not coverage, decides whether a suite catches regressions]] — `argument`
- [[Order a test matrix by feedback latency - smoke, sync rejections, volume last]] — `lesson`

## Security & API design

*Fail-open authorization, status-code semantics, and honest error reporting.*

- [[A null-guarded tenant check fails open, so a renamed path parameter disables isolation]] — `lesson`
- [[A pagination token is an opaque cursor, and it must carry the filter it was issued under]] — `concept`
- [[CQRS splits read and write models architecturally, CQS only splits methods]] — `concept`
- [[Returning 401 for a permission failure causes infinite login loops]] — `lesson`
- [[Silently-ignored input needs a visible reason field, or it looks like data loss]] — `lesson`

## Finance & domain logic

*SAP export mechanics, VAT rounding, and ordering via queue priority.*

- [[Express an ordering requirement as queue priority, not as a synchronous wait]] — `lesson`
- [[Rounding per line makes net plus VAT miss gross, so the VAT line absorbs the difference]] — `lesson`
- [[SAP export defaults come from the default-sap config service so accounting codes change without a deploy|SAP export defaults come from the /default-sap config service so accounting codes change without a deploy]] — `lesson`
- [[SAP in luz_finance is a manual CSV export, not a live integration]] — `observation`

## Tooling

*Lessons from building the import itself.*

- [[JS regex dot excludes carriage return, so (.)$ silently fails on CRLF lines|JS regex dot excludes carriage return, so (.*)$ silently fails on CRLF lines]] — `lesson`

## Other

- [[Claude Code Bash tool collapses backslashes even inside quoted heredocs]] — *Claude Code*
- [[Blocking on CompletableFuture.get in a custom pool recreates the bottleneck]] — *Concurrency*
- [[Keyword classifiers assign topics by lexicon size unless you normalize]] — *Information Retrieval*
- [Pandoc gfm-raw_html silently replaces complex tables with TABLE](<3 Resources/Pandoc/Pandoc gfm-raw_html silently replaces complex tables with [TABLE].md>) — *Pandoc*
- [[Playwright for UI E2E, k6 for load split by specialization not overlap|Playwright for UI E2E, k6 for load: split by specialization not overlap]] — *Testing*
- [[LUZ-158230 ePost ZIP Import Test Fixture Matrix (Confluence)]] — *testing-agent/LUZ-158230*

---

## Not distilled

Some source pages are records rather than knowledge, and are kept in the mirror without atomic notes:

- **23 `Case Report: DocumentId …` pages** — per-document evidence for one incident; the finding is in [[Every production FAILED_TO_STORE traced back to a rolling deploy, not to load]].
- **Sprint retrospectives and joint reviews** — point-in-time team records.
- **Test-execution evidence pages** — UAT run screenshots tied to specific sprints.
- **Xray / test-case library templates** — procedural forms, not findings.

%% ai-graph-start %%

**Related notes:**
- [[Confluence-Distillation]]
- [[Investigation Stories - Audit Logs Current Implementation]]
- [[Confluence Export — Index]]
- [[Luz Audit System - Performance Optimization Proposal]]
- [[Solution - Enhanced Chain-Signature Hybrid]]

%% ai-graph-end %%