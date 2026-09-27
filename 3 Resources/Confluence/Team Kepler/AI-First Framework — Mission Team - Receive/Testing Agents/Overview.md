---
title: "Overview"
created: 2026-09-07
updated: 2026-09-07
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49730682947/Overview
confluence_id: "49730682947"
confluence_path: "Team Kepler > AI-First Framework — Mission Team: Receive > Testing Agents"
tags: [confluence, ai-agents]
---

# Overview

*Confluence source · Team Kepler › AI-First Framework — Mission Team: Receive › Testing Agents · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49730682947/Overview) · updated 2026-09-07*

## Manual Test Flow - Non-AI

- **Non-AI = human do ALL test. No robot. Human brain only. Slow. Tired. But work.**

### The hunt (steps, in order)

1.  **Gather knowledge** 🔍**.**

    1.  Human read Jira.

    2.  Human read Confluence.

    3.  Human dig code.

    4.  Human read log.

2.  **Refine knowledge.** ❓

    1.  Human confused?

    2.  Human go ask other human. Ask BA. Ask dev. "What this mean?"

3.  **Define test plan.** 🗺️

    1.  Human decide: How test? (API, screen, whole thing.) What test? (which feature.)

    2.  What "pass" mean?

4.  **Say what "pass" mean.** 🎯

    1.  Human write rule: this = good, this = bad.

    2.  How much cover enough?

5.  **Build test plan.** 🧱

    1.  Human make fake data.

    2.  Human write test case — happy path, angry path.

    3.  Human write step 1, 2, 3. *(All by hand. No auto-generate.)*

6.  **Run test.** 🏃

    1.  Human set up machine.

    2.  Human click / run test (by hand OR script).

    3.  Human clean up mess.

    4.  Human write down what happen.

7.  **Big question: ALL TEST PASS?** ⚖️

    - **YES** ✅ → **Write completion report.** Document result. Close ticket. Hunt done. Eat. Sleep. 🦣

    - **NO** ❌ → go fix road (next).

8.  **Fix road (when test fail):** 🔧

    - **Triage** — sort bug: tech problem or business problem?

    - **Raise defect ticket.** Write down history.

    - **Notify dev.** Poke dev. "You break. Fix."

    - **Dev fix bug.**

    - **Retest when fix done.** Loop back to step 6. 🔁🪨

![[image-20260907-030630.png]]

### What NON-AI is MISSING (the robot stuff)

- ❌ **No Agent Memory**

  - Human no shared robot memory.

  - Human forget.

  - Human repeat mistake.

- ❌ **No Test / Ops / Report AI Agent**

  - No robot helper watch, report, or think.

- ❌ **No auto "collect insight"**

  - Flow no learn by self. Only human learn (maybe).

- ❌ **No AI Q&A refine** — no smart robot to explain fast.

**Net:** same hunt, but human carry ALL rock. More slow. More miss. More tired. Robot help = less rock.

## AI-Assisted Test Flow - Agentic QA/QC

- **Robot AND human hunt test together.**

- **Human throw rock, robot help, robot remember, robot learn. Full team.** 🤖

### The hunt (steps, left to right)

1.  **Gather knowledge.** 🔍

    1.  Read Jira.

    2.  Read Confluence.

    3.  Robot memory + code-graph + log.

    4.  Grab all rock.

2.  **Refine knowledge.** ❓

    1.  Robot ask human question.

    2.  Human answer.

    3.  Human say "robot, you understand right?"

3.  **Make test plan.** 🗺️ *(check with robot)*

    1.  Decide: How test? (API, whole thing, screen.)

    2.  What test? (which feature.)

    3.  What "pass" mean?

    4.  Side rock → **Test Evaluation define**: what count as pass? how much cover enough?

4.  **Build test plan.** 🧱

    1.  Make fake data + fake account.

    2.  Write case — happy path, angry path.

    3.  Write step 1, 2, 3.

5.  **Run test.** 🏃 on machine (local, dev…)

    1.  Set up machine.

    2.  Run case (hand OR auto). Clean mess.

    3.  Write down what happen.

    4.  Side rock → **Test Evaluation support**: watch how test run.

------------------------------------------------------------------------

### BIG QUESTION rock: ALL TEST PASS? ⚖️

- **YES** ✅ → **Write completion report.** 🎉 Sort result. Close ticket. Hunt done. Eat mammoth. 🍖

- **NO** ❌ → go **fix road** (below).

#### Fix road (test fail) 🔧

- **Triage** — sort bug: tech problem or business problem?

- **Triage support** — raise defect ticket. Write history.

- **Send ticket → poke dev (notify/trigger) → dev FIX BUG** (+ fix support) **→ retest when fix → LOOP back.** 🔁

![[image-20260907-035528.png]]

### The robot helpers 🤖 (down + side)

- **Test AI Robot**, **Ops AI Robot** (watch/monitor), **Report AI Robot** (make report). Three robot help hunt.

- **After AI → collect insight → loop.** Robot LEARN after hunt. Get smart. Loop back. 🧠

- **Polaris Memory Bank** — big shared robot brain. All robot read + write here. Robot no forget. 🗄️

------------------------------------------------------------------------

### One grunt takeaway

**full-flow = same test hunt as non-ai, BUT robot added.**

- Robot help every step, robot remember in brain, robot learn after. One robot (quality check) already built ✅.

- Smarter brain (pgvector) still just plan 📐.

- Human carry LESS rock now. 🪨👍
