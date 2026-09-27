---
title: "ClamAV definition updates restart clamd, so scheduled 503s are expected not broken"
created: 2026-09-27
type: observation
status: seedling
source: "Confluence: Part B - luz-antivirus Analysis (2025-12-18)"
tags: [clamav, luz-antivirus, kubernetes, readiness-probe, monitoring, kepler]
---

# ClamAV definition updates restart clamd, so scheduled 503s are expected not broken

ClamAV's `freshclam` downloads new virus definitions on a schedule, and loading them requires **restarting the `clamd` daemon**. While it restarts, the clamd socket is gone, so any health check hitting it fails. Measured on `luz-antivirus` in production: **12–20 seconds per restart (avg ~15s), roughly 10–12 times per day per pod.**

That is normal operation, not a fault. With 4–8 pods behind an HPA the updates stagger naturally, so overall service availability holds.

The part that surprised: **the probe configuration decided how loud this looked.** With `readinessProbe.periodSeconds: 2` and `failureThreshold: 1`, a 15-second blip produces `15 ÷ 2 ≈ 7–11` logged 503s — every single one. Multiply by ~11 updates and 8 pods and you get the 1,000+ 503s/day that dominated the error dashboard.

Two takeaways:

- **A dependency that self-restarts needs a probe tuned to its restart time**, or it reports itself as broken on schedule. `failureThreshold: 3` here absorbs the blip while still catching a real outage within ~6s.
- **Probe frequency is an error-rate multiplier.** Before concluding a service is flaky, check whether you are just sampling a short outage very often.

Separate, genuinely-bad finding in the same service: 6 × HTTP 504 `SocketTimeoutException` at exactly **300 s** on **37–75 MB** files — a hard scan-duration ceiling that needs a size limit or an async scan path, not probe tuning.

## Related

- [[Error volume and error severity are independent, so triage by impact not by count]]

## Related

- [[Error volume and error severity are independent, so triage by impact not by count]]
