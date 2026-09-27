---
title: "Playwright for UI E2E, k6 for load: split by specialization not overlap"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: Testing Tool Comparison Playwright vs k6 (LUZ)"
tags: [testing, playwright, k6, e2e, load-testing, tooling, confluence-distilled]
---

# Playwright for UI E2E, k6 for load: split by specialization not overlap

Playwright and k6 both *can* drive a browser and both *can* issue load, so teams waste time arguing which one to standardise on. The resolution is to split by what each is specialised for and accept the overlap is nobody's strong suit:

- **Playwright → functional E2E UI testing.** Simulates real user browser actions, supports parallel runs, produces detailed reports and snapshots, and ships a **test generator/recorder** that writes scripts from a click-through.
- **k6 → load and performance testing.** Built for many iterations and load scenarios against backend infrastructure; cloud-based load runs are licensed.

The deciding factor was not capability but **maintenance cost per team-hour**. Playwright's recorder meant UI scripts could be produced and repaired quickly; k6 has no native recorder (k6 Studio exists but was not the team's path), so every UI script is hand-written. k6's own UI-testing module was described as the "unwanted sibling" to its load-testing strength.

> [!warning] Both are brittle the same way
> Each breaks when element IDs or tags change. Choosing Playwright does not buy you stable selectors — it buys you a faster way to regenerate broken ones. The mitigation is the same either way: split into small test cases so a selector change invalidates one test, not a suite.

Other limits worth carrying: Playwright can time out against a slow server, and it is not the tool for heavy load. Both run headless on a server; Playwright also runs against local, dev, and staging.

**The transferable idea:** when two tools overlap, do not pick the one with the larger feature union. Pick per *job* based on which tool treats that job as its primary concern, and let each own its lane. A tool's secondary feature is usually maintained as a secondary feature.

Source: [[Testing Tool Comparison Playwright vs. k6]] (LUZ, Confluence).

## Related

- [[Testing Tool Comparison Playwright vs. k6]]
