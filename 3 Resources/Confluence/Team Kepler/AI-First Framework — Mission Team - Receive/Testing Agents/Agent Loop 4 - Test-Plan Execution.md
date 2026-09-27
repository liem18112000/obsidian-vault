---
ai_hash: 70cfa239ed4cb128
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49780031515'
confluence_path: 'Team Kepler > AI-First Framework — Mission Team: Receive > Testing
  Agents'
created: 2026-09-23
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- ai-agents
title: 'Agent Loop 4: Test-Plan Execution'
type: source
updated: 2026-09-23
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49780031515/Agent+Loop+4+Test-Plan+Execution
---

# Agent Loop 4: Test-Plan Execution

*Confluence source · Team Kepler › AI-First Framework — Mission Team: Receive › Testing Agents · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49780031515/Agent+Loop+4+Test-Plan+Execution) · updated 2026-09-23*

## ⓪ THE GAP — leaf never run 🍂✗ (red dashed box, top-left)

Today the hunt-chain go `gather → refine → define → implement_plan → get_scenarios` and **STOP**. `implement_plan` carve a Gherkin `.feature` leaf "for the execution stage" — and **that stage no exist.** So nothing run, nothing self-fix, and every downstream quality number is a GUESS (a *proxy*) standing in for what only a real run can tell. **EXEC close that gap.** 🔴

------------------------------------------------------------------------

## ① THE NEW ROBOT — EXEC, same shape as the other three 🟧 (big orange box)

EXEC is the **4th A2A agent**, brother of KGA / TPD / TEV, keyed by the same `context_id`, registered on the same MCP gateway. **No new datastore, no new protocol** — it reuse every seam already there. Two rules keep it safe:

- **Off the request path.** A full run is heavy (heavier than the three Vertex calls that once tripped `ERROR_TIMEOUT`). So `run_suite` KICK OFF a job, say `in_progress`, and the client re-poll — same multi-turn dance `implement_plan` already do. 🕒

- **Deterministic first, robot only to heal.** Robot find a step ONCE → freeze it to a hard locator → big-brain wake up ONLY when a step break. Fast, repeatable. 🧊

------------------------------------------------------------------------

## ② THE INNER LOOP — run → measure → (heal) → triage 🔁 (blue/green/yellow boxes)

One `run_suite(context_id, env)` call drive a small bounded loop, checkpointed to GCS so a Cloud Run kill **resume**, not restart:

- **0 · RESOLVE ENV** 🔵 — pick the target cave (JEV Choice), health-probe the `base_url`, upsert its row.

- **1 · RUN** 🔵 (inside the 🔒 **red dashed sandbox**) — Playwright + `behave` run the `.feature` unchanged; step-binding via Playwright MCP a11y refs. *(The sandbox is the ONLY robot holding live creds — the trust boundary, see ⚠️.)*

- **2 · MEASURE** 🟢 (on PASS) — coverage delta, flakiness ×5, schema/status/auth conformance (Schemathesis, not `assert 200`), mutation kills.

- **3 · TRIAGE** 🟡 (on FAIL) — sort the fail: **Actual Bug** → record + stop 🔴 · **UI change** → HEAL · **Flaky/Env** → quarantine ×5 ⬜ (JEV Choice, LLM only on the hard tail).

- **HEAL step** 🟠 — propose a locator/wait/data patch, surface it through the client's **human Yes/No gate** — *never a silent retarget* (a silent one hide a real regression). Accepted → re-run the patched step, **bounded by MAX_HEALS**, loop back to RUN.

- **4 · PERSIST + REPORT** 🟢 — `run.json` + traces → GCS; one run row → Postgres.

## The stone ledger 🪨 (green + grey boxes, bottom-left)

- **Shared Cloud SQL** (`common.db.get_engine`) — the SAME Postgres that already holds the task store + ADK sessions + pgvector. EXEC add **TWO new tables**: `exec_environment` (one row per cave ever tested — name, base_url, revision, `creds_ref` = a Secret Manager *path*, never the secret) and `exec_run` (one row per run — summary, signals, triage, `trace_uri`). **Reuse the Postgres — no new datastore** (a redeploy once wiped loose GCS state; the ledger must survive). 🟢

- **GCS** ⬜ — the heavy rocks (run.json, traces, heal patches) + a `trace_uri` pointer. Same GCS-heavy / SQL-light split the rest of the system use.

## Many caves — 2nd cave IS the oracle 🗺️ (light-blue strip, bottom)

EXEC run the SAME leaf across **many caves**: `dev` · `staging` · `rev:abc123` (a Cloud Run revision) · `tenant:42`. New cave → new row, on first sight, no pre-register. With **≥2 runs** on different caves you get **differential mode**: same input, two caves, diff the answers — any divergence is a regression, and **consensus IS the oracle** (no ground-truth needed). *That* is the payoff of storing every cave: the second cave judge the first. 🔀

## JEV — fast typed brain in front of big-brain 🟣 (purple box, right)

Every EXEC decision (*bug or UI change? accept this heal? is it flaky? which cave?*) is a typed judgment — exactly what the **JEV** `DecisionProvider` port is for (`choice` · `score` · `noul`, ~150ms, ~free). The cascade: JEV answer first; **conf ≥ θ → typed verdict (~90% of calls, fast path)**; else **fall through to the existing LLM judge (~10% hard tail)**. `TPD_DECISION_BACKEND` unset → JEV returns None and every caller keep its LLM path → worst case = today. **Default OFF.** 🧠⚡

## ⑥ FEED THE SCORE-ROBOT — retire the guesses 🟢 (green box, bottom-right)

TEV score the pipeline today **without running it**, so its heavy terms are GUESSES waiting for exactly this run. EXEC hand back the real signals:

- `fault_detection` **0.30** (the heaviest weight) → **REAL mutation score** (mutmut / PIT / Stryker: mutants *killed*, not *aimed at*).

- `coverage` **0.20** → **executed** coverage delta (lines the run actually hit).

- **+ conformance + flakiness ×5** — brand new signals.

**The TPS weights DON'T change — only the INPUTS get real** (proxy → run). New tool `evaluate_run(context_id)` sit beside `evaluate_pack` / `evaluate_plan`; benchmarks now compare *execution-grounded* runs across caves. And **the assured loop close** — its MEASURE step was a rubric stub; now it gate on `builds ∧ passes×5 ∧ raises coverage ∧ kills mutant`.

## ⚠️ SAFETY — this robot hold the KEYS 🔒 (red dashed sandbox)

Unlike read-only KGA/TPD/TEV, EXEC **hold test-env creds and drive live systems** — the biggest security delta in the whole cave. Guard it: **run in a sandbox** · store a secret *reference* (Secret Manager path), **never the value** in Postgres or GCS · **allow-list egress** per run (the sandbox reach only the target host). This is the primary review surface. 🛡️

![[image-20260923-132541.png]]

### Rock color meaning 🎨

- 🟧 **orange** = the NEW EXEC robot + the run flow / heal path

- 🔵 **blue** = deterministic run steps (resolve · run) · 🟢 **green** = real signals / measure / persist / the ledger / TEV

- 🟡 **yellow** = the triage decision · ⬜ **grey** = GCS · flaky-quarantine

- 🟣 **purple** = JEV typed brain · 🔴 **red** = the gap / actual-bug / 🔒 the sandbox trust boundary

- 🟦 **light-blue dashed** = DESIGN-ONLY status · light-blue = the many caves

------------------------------------------------------------------------

### One grunt takeaway

**Leaf finally RUN. 4th robot grab the** `.feature`**, run it in a locked sandbox against a chosen cave, watch with real oracle-eyes, human-approve each heal, sort each fail, write every cave + run to the shared stone (2 new tables), and hand the score-robot REAL numbers so it stop guessing.** Off the hunt-path, resumable, human owns the gate, keys locked away. **But robot only**

%% ai-graph-start %%

**Related notes:**
- [[Test Executor Agent - Closing the Testing Pipeline Gap]]
- [[Agent Loop 3 - Test-Plan Definition]]
- [[Test-Plan Definition Agent]]
- [[Flow View - V2]]
- [[Sub Agentic Loop 1.2 - GCP Service Exploration]]

%% ai-graph-end %%