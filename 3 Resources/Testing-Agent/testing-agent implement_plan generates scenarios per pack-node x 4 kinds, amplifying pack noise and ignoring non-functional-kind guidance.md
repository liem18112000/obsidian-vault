---
title: "testing-agent implement_plan generates scenarios per pack-node x 4 kinds, amplifying pack noise and ignoring non-functional-kind guidance"
created: 2026-09-15
type: lesson
status: seedling
source: "testing-agent run run-87933300, 2026-09-15"
tags: [testing-agent, implement_plan, scenarios, noise, precision, gotcha, mcp-timeout]
---

# testing-agent implement_plan generates scenarios per pack-node x 4 kinds, amplifying pack noise and ignoring non-functional-kind guidance

When `implement_plan` (testing-agent) generates scenarios via the P4 assured loop, it emits **one scenario group per PACK NODE × the 4 default kinds** (happy / negative / boundary / error). Two consequences bit the LUZ-158230 run (run-87933300):

1. **Pack noise is amplified into scenario noise.** The pack had `precision=0.00` (complete-but-noisy, recall=1.0) — 41 nodes, ~26 of them irrelevant (sibling LUZ tickets, attachment `.png`/`.jpg` images, the spec PDFs *as targets*, our own [Agentic Framework] backlog stories, "dev test-suite results" pages, Self-check/Cross-check). The generator dutifully produced 4 scenarios for EACH → **164 total**, ~104 of which are noise. Low retrieval precision therefore has a direct downstream cost even though it "doesn't block." Curate to the nodes whose `covers:` is the real feature.

2. **Non-functional-kind guidance is ignored.** Even after answering the case-design round with an explicit full non-functional suite (security, performance, concurrency, i18n, accessibility, migration), the generated kind histogram was ONLY happy/negative/boundary/error. The extra kinds silently did not materialize (matches the known scenario-generator silent-fallback / define-answer-not-persisted limitations).

3. **Quality split within a group:** the *happy-path* variants for genuinely-relevant [eArchive] stories are rich and specific (real transfer.zip / Post Health / healthData-verbatim / folder-merge / notification-once oracles), but the *negative/boundary/error* variants frequently fall back to generic boilerplate ("invalid input → 4xx, no side effects").

Also: **the client MCP call aborts on idle-timeout (~44 min silent) but the server checkpoints to GCS**, so `get_scenarios` still returns the full set afterward — treat the client abort as cosmetic, then fetch scenarios. (get_scenarios came back 644KB / 164 scenarios.)

Fix for cleaner output: use a tighter, higher-precision pack (fresh context, focused seed/grounding) rather than post-hoc `exclude=` (a no-op on a converged context).

## Related
[[Testing-agent refine loses confidence when source-of-truth PDF attachments are undistilled gaps]]
[[LUZ-158230 is the ePost eArchive transfer.zip document-import feature]]

## Related

- [[Testing-agent refine loses confidence when source-of-truth PDF attachments are undistilled gaps]]
- [[LUZ-158230 is the ePost eArchive transfer.zip document-import feature]]
