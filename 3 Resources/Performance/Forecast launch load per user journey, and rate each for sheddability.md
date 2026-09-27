---
title: "Forecast launch load per user journey, and rate each for sheddability"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: AI-1538 Prepare for more volume on Analyze API (AI)"
tags: [capacity-planning, load, user-journeys, launch, load-shedding, confluence-distilled]
---

# Forecast launch load per user journey, and rate each for sheddability

Forecasting load by scaling up last month's request count tells you nothing about *which* service breaks. Enumerate the **user journeys** instead, trace each one to the services it touches, and the forecast becomes a per-component work list.

The setup: a partner app with **2M users** and **200,000 sessions/day (peak 300,000)** was about to embed the product on a known date. Rather than one aggregate number, the analysis tabulated:

| Column | Why it is there |
|---|---|
| **ID** (`J0`, `J1`, …) | A stable handle so the journey can be referenced in tickets and re-estimated later |
| **User journey** | "New user is created", … — in product language, not endpoint names |
| **Path to the API** | How this journey actually reaches the service under study — *or that it does not* |
| **Release-peak impact estimate** | LOW / … per journey, so effort goes where the load is |
| **Components affected** | Which systems inherit that load |
| **Issues / Questions** | Open unknowns, named rather than assumed |

**Why journeys beat aggregate extrapolation:**

- **It finds the paths you forgot.** `J0` turned out to reach the pipeline **via a different API entirely** — load the team would have attributed to the wrong service.
- **It makes the estimate arguable.** A reviewer can disagree with one journey's rating. Nobody can meaningfully disagree with "traffic will be 3× higher".
- **It maps directly to mitigation.** Each row names its components, so the output is a list of things to fix, not a number to worry about.

> [!tip] Rate each journey for *sheddability*, not just volume
> The most useful annotation in that table was not the size of the load but its **criticality**: *"real-time user record ingestion can be switched off and should be non-blocking"*. Knowing which journeys can be degraded, delayed or disabled under peak is what turns a capacity plan into a launch-day runbook. Volume tells you what will hurt; sheddability tells you what you can drop when it does.

> [!warning] Write down the questions, and check the data before trusting it
> The table carries an open question with its own answer half-found: *"Do we get a User record for new users? (checking data — yes we do, but data looks wrong, does not match the other system's numbers)"*. Capacity work is built on assumptions about volumes; recording which ones are **unverified** is what stops a plan being confidently wrong. A discrepancy between two systems' counts is a finding, not a rounding issue.

Related: [[Split page load into server, render and interaction before optimising]] — the same instinct, applied after the fact rather than before.

Source: [[AI-1538 Prepare for more volume on Analyze API]] (AI, Confluence).

## Related

- [[Split page load into server, render and interaction before optimising]]
