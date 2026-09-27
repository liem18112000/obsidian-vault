---
title: "Error volume and error severity are independent, so triage by impact not by count"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: Service Error Analysis Report - FAILED_TO_STORE on Production (2025-12-18)"
tags: [sre, observability, triage, monitoring, reliability]
---

# Error volume and error severity are independent, so triage by impact not by count

The same 30-day production review logged **1,000+ HTTP 503s from `luz-antivirus` and 25 `FAILED_TO_STORE`s from `luz-jsonstore`**. The 503s were 40× more numerous and entirely harmless; the 25 were data loss.

The 503s came from ClamAV doing its job — see [[ClamAV definition updates restart clamd, so scheduled 503s are expected not broken]]. No user request failed; they were health-check responses during a 15-second daemon restart, absorbed by the other pods behind the HPA. The 25 `FAILED_TO_STORE`s each meant a document that did not get stored.

Two habits fall out of this:

- **Rank by blast radius, not by log volume.** A dashboard sorted by count puts the harmless thing first and buries the incident. Ask "what did the user lose?" before "how many lines?".
- **Expected-but-noisy errors need suppressing at the source, or they mask real ones.** Here the noise was partly manufactured by probe settings (`periodSeconds: 2`, `failureThreshold: 1`), which turned one 15-second event into 7–11 logged failures. Raising `failureThreshold` to 3 cuts the volume without hiding a genuine outage.

The trap this guards against: after weeks of ignoring a 1,000-strong error class because "that one's normal", the eye stops reading that part of the log — and a real failure in the same class goes unnoticed.

## Related

- [[ClamAV definition updates restart clamd, so scheduled 503s are expected not broken]]
- [[Every production FAILED_TO_STORE traced back to a rolling deploy, not to load]]

## Related

- [[ClamAV definition updates restart clamd, so scheduled 503s are expected not broken]]
