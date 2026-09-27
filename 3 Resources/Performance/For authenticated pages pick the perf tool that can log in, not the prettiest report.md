---
title: "For authenticated pages pick the perf tool that can log in, not the prettiest report"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: eLetter Performance Investigation Report (Helios)"
tags: [performance, playwright, lighthouse, cdp, web-vitals, sso, confluence-distilled]
---

# For authenticated pages pick the perf tool that can log in, not the prettiest report

The usual web-performance tools — Lighthouse, WebPageTest — are built around **public, unauthenticated** pages. The moment the page you actually care about sits behind Keycloak, an SSO redirect, and an account-selection step, most of them stop being usable and you are left with **Playwright driving Chrome DevTools Protocol**.

How the four compared for measuring a real logged-in inbox:

| Tool | Why it wins | Why it fails here |
|---|---|---|
| **Playwright + CDP** ⭐ | Drives the full auth flow (Keycloak, account selection); real production-shaped data; CI-ready; exposes `PerformanceResourceTiming` and precise CDP network timestamps | No built-in HTML report; wall-clock affected by local machine load |
| **Lighthouse** | Built into DevTools; Core Web Vitals (LCP, INP, CLS); nice reports; throttling emulation | **Cannot authenticate** — no Keycloak/SSO; synthetic data, not the real inbox; dev-server numbers unrepresentative |
| **WebPageTest** | Filmstrip, multi-location, multi-device, full waterfall | Needs a **public URL** (no localhost); SSO is hard; paid tiers; awkward in CI |
| **Chrome DevTools (manual)** | Zero setup; waterfall, timeline, JS flame chart | Not reproducible or automated; varies run to run; no trend over time |

**The selection rule:** for an authenticated application, pick the tool that can *become a logged-in user* first, and accept worse reporting. Pretty Core Web Vitals scores computed against a login screen measure the login screen.

> [!tip] Add `Server-Timing` before you need it
> The noted weakness of the Playwright+CDP approach is that it *"cannot separate Next.js from upstream without `Server-Timing`"*. TTFB is one number covering your server-side rendering **and** every backend it called. Emitting `Server-Timing` headers — one entry per meaningful span — splits that number at zero measurement cost and turns "TTFB is 1.4 s" into an attribution. It is much easier to add proactively than during an incident.

> [!warning] Two measurement traps in this comparison
> **Dev servers lie.** Next.js in dev compiles on demand and skips production optimisations; numbers from a dev server are not a baseline for anything.
> **Wall-clock from your laptop includes your laptop.** A local Playwright run competes with everything else on the machine. Run repeatedly, report the median, and prefer a quiet CI runner when comparing across days.

Related: [[Playwright for UI E2E, k6 for load split by specialization not overlap]] — Playwright earning a third job here, as the only tool that can log in.

Source: [[eLetter Performance - Investigation Report Load Times and Optimization Recommendations]] (Helios, Confluence).

## Related

- [[Playwright for UI E2E, k6 for load split by specialization not overlap]]
