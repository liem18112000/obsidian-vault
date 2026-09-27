---
title: "Test Executor Agent: Closing the Testing Pipeline Gap"
created: 2026-09-24
updated: 2026-09-24
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49782063122/Test+Executor+Agent+Closing+the+Testing+Pipeline+Gap
confluence_id: "49782063122"
confluence_path: "Team Kepler > AI-First Framework — Mission Team: Receive > Testing Agents > Agent Loop 4: Test-Plan Execution"
tags: [confluence, ai-agents]
---

# Test Executor Agent: Closing the Testing Pipeline Gap

*Confluence source · Team Kepler › AI-First Framework — Mission Team: Receive › Testing Agents › Agent Loop 4: Test-Plan Execution · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49782063122/Test+Executor+Agent+Closing+the+Testing+Pipeline+Gap) · updated 2026-09-24*

## Overview

- Today the pipeline is `gather → refine → approve → [evaluate_pack] → define_plan → approve_plan → implement_plan → get_scenarios → [evaluate_plan]`. `implement_plan` exports a Gherkin `.feature` "for the downstream Test execution stage" — **and that stage does not exist.**

- The pipeline generates scenarios (now *scored* by the TPD assured loop), then **stops**: nothing is ever run, nothing self-heals, and every downstream quality number is a **deterministic proxy** (`oracle_strength`, `fault_class_coverage`) standing in for signals only a real run can produce.

- The Test Executor closes that gap. It is a **4th A2A agent**,

  - sibling to KGA/TPD/TEV, keyed by the **same** `context_id`

  - runs the persisted `.feature` against a **selected target environment**;

  - wraps every step in a **layered oracle** (schema/status/auth conformance, not `assert 200`)

  - **self-heals** flaky locators/waits under a human gate

  - **triages** each failure (Bug / Heal / Flaky / Env)

  - records **each tested environment and each run to Postgres**

  - hands TEV the real coverage / flakiness×5 / mutation / conformance signals that let it retire its proxies.

- The pipeline stays **linear and human-gated on the outside**

- the Executor adds a bounded **run → measure → (heal) → triage** inner loop.

## **Purpose**

Design the **fourth main A2A agent** of the Testing Agent — the **Test Executor (EXEC)** — the missing **Pillar-2** stage that turns the Gherkin `.feature` the Test-Plan Definition (TPD) agent exports into a *real, self-correcting, multi-environment test run*, then feeds its execution signals back into the Test-Evaluation (TEV) scores.

### Primary sources (selection)

- Playwright Test Agents [https://playwright.dev/docs/test-agents](https://playwright.dev/docs/test-agents)

- Playwright MCP [https://github.com/microsoft/playwright-mcp](https://github.com/microsoft/playwright-mcp)

- browser-use [https://github.com/browser-use/browser-use](https://github.com/browser-use/browser-use)

- Skyvern [https://github.com/Skyvern-AI/skyvern](https://github.com/Skyvern-AI/skyvern)

- Healenium [https://github.com/healenium/healenium](https://github.com/healenium/healenium)

- Schemathesis [https://github.com/schemathesis/schemathesis](https://github.com/schemathesis/schemathesis)

- RESTler [https://github.com/microsoft/restler-fuzzer](https://github.com/microsoft/restler-fuzzer)

- Hypothesis [https://github.com/HypothesisWorks/hypothesis](https://github.com/HypothesisWorks/hypothesis)

- Metamorphic testing survey [https://dl.acm.org/doi/10.1145/3143561](https://dl.acm.org/doi/10.1145/3143561)

- Meta ACH [https://arxiv.org/abs/2501.12862](https://arxiv.org/abs/2501.12862)

### 1. Main architecture

The Executor is **the same shape as the three existing agents** and reuses every seam the pipeline already has — that is the whole point: no new datastore, no new protocol, no new gateway.

**The five load-bearing decisions:**

1.  **A 4th A2A agent, not a mode of TEV.** It holds credentials and drives live systems — a trust boundary the read-only KGA/TPD/TEV never crossed. Keep it a separate deployable so its blast radius, IAM, and network egress are isolated. It registers on the gateway *identically* to the others (`EXEC_A2A_URL` + `register_exec(mcp, exec_session)`), keyed by `context_id`.

2.  **The runner lives in a sandbox the agent** ***kicks off and polls*****, never in the request path.** A full test run is far heavier than the three serial Vertex calls that once blew the Cloud Run liveness/request timeout (memory: *implement serial Vertex calls → Cloud Run timeout*). The A2A tool starts a job, returns `[state: in_progress]`, and the client re-polls — exactly the multi-turn contract `implement_plan` already uses.

3.  **Deterministic-first, agent-only-to-heal.** The winning production pattern (Octomind's "AI doesn't belong in test runtime"): **agent discovers a step once → compile it to a deterministic locator → the LLM re-engages only when a step fails.** The steady-state run is fast and repeatable; the model is a *healer*, not an interpreter, on every step.

4.  **Every oracle upgrade rides the OpenAPI surface the services already expose.** `assert 200` becomes "conforms to the contract" for free — no new spec authoring (§3.2).

5.  **State reuses what exists.** Loop/run state → **GCS** under `context_id` (resume-not-restart, the discipline the TPD assured loop already uses). Tested-environment + run metadata → the **shared Cloud SQL Postgres** that already backs the task store and ADK sessions (§4). Typed decisions (triage, heal-accept, flakiness) → the **JEV** `DecisionProvider` **port** already added beside `ModelProvider` (§5).

![[image-20260924-022431.png]]

**A2A Agent Card — proposed tools** (registered on the gateway exactly as KGA/TPD/TEV tools are):

|  |  |  |
|----|----|----|
| Tool | Shape | Role |
| `run_suite(context_id, env=None)` | multi-turn (`in_progress`→`done`) | Run the persisted `.feature` against the selected/chosen environment; kick-off + poll. |
| `heal_step(context_id, step_id)` | single | Replay one failing step, propose a locator/wait/data patch (human-gated). |
| `triage_run(context_id)` | single | Classify each failure → Bug / Heal / Flaky / Environment. |
| `get_run_report(context_id, run_id=None)` | read-only | The persisted run report (per-env, per-scenario, oracle results). |
| `list_environments(context_id)` | read-only | The environments seen for this context and their last run state. |

**Pipeline seam** — the Executor slots in *after* `get_scenarios`, optional and read-model-friendly like `evaluate_plan`:

![[image-20260924-022541.png]]

## 2. Detailed flow

- A single `run_suite(context_id, env)` call drives the bounded inner loop above.

- Everything is checkpointed to GCS under `context_id` so a Cloud Run kill **resumes** at the last completed scenario.

### **Key properties of the flow:**

- **Discover-once, compile, heal-on-fail.**

  - Step binding resolves against the *current* a11y tree; the resolved `ref` is cached deterministically.

  - The LLM only re-engages on a `FAIL` that triage labels *UI-change*.

- **Self-heal is human-gated.**

  - Every locator/wait/data patch a healer proposes is surfaced through the client's existing **Yes/No gate** — a silent retarget can mask a real regression.

  - This is the same gate discipline the pipeline already enforces at `approve` / `approve_plan`.

- **Flakiness is a verdict, not a guess.**

  - A step that oscillates is re-run ×5; JEV `noul("this test is flaky")` + the run history decide *quarantine* (still runs, no longer blocks) vs *real intermittent bug*.

- **Bounded.**

  - `MAX_HEALS` per run and a per-scenario timeout keep the loop finite; exhaustion → record as unresolved and move on, never spin.

- **Resumable.**

  - Each completed scenario is checkpointed; re-invoking `run_suite(context_id)` after a kill continues from the last checkpoint (resume-not-restart).

![[image-20260924-022633.png]]

## Multi-environment execution + the per-environment database

## **The requirement**

The Executor must run the same `.feature` across **multiple environments** (local / dev / staging / a specific Cloud Run revision / a customer tenant) and **dynamically store each tested environment's info to a database** — so runs are comparable across environments and over time, and so **differential testing** (§3.2) has two sides to diff.

**The lazy-but-correct store: reuse the Postgres that is already there.** `common/db.py::get_engine()` is a single async SQLAlchemy pool that dials Cloud SQL via the Python Connector (`TASK_DB_URL` / `DB_*`), and it **already backs** the A2A `DatabaseTaskStore`, the ADK `DatabaseSessionService`, pgvector memory, and the prompt store. The tested-environment DB is **two new tables on that same engine** — not a new datastore (memory: *Task store on Cloud SQL*; handoff constraint: reuse the existing Postgres).

> [!note]- Detail DDL
>
>
>
> ```
> -- environment registry: one row per (context_id, environment) ever tested
> CREATE TABLE exec_environment (
>   id            uuid PRIMARY KEY,
>   context_id    text NOT NULL,
>   name          text NOT NULL,             -- 'dev' | 'staging' | 'rev:abc123' | 'tenant:42'
>   base_url      text NOT NULL,
>   kind          text,                       -- cloud-run | gke | local | tenant
>   revision      text,                       -- image tag / git sha / Cloud Run revision
>   creds_ref     text,                       -- Secret Manager path — NEVER the secret value
>   health        jsonb,                      -- last probe: {ok, status, latency_ms, checked_at}
>   first_seen    timestamptz DEFAULT now(),
>   last_seen     timestamptz DEFAULT now(),
>   UNIQUE (context_id, name)
> );
>
> -- run ledger: one row per run_suite invocation against one environment
> CREATE TABLE exec_run (
>   id             uuid PRIMARY KEY,
>   context_id     text NOT NULL,
>   environment_id uuid REFERENCES exec_environment(id),
>   started_at     timestamptz DEFAULT now(),
>   finished_at    timestamptz,
>   state          text,                       -- in_progress | done | failed
>   summary        jsonb,                      -- {passed, failed, healed, quarantined, flaky}
>   signals        jsonb,                      -- coverage_delta, flakiness, conformance, mutation_kills
>   triage         jsonb,                      -- per-scenario Bug/Heal/Flaky/Env verdicts
>   trace_uri      text                        -- gs://…/context_id/run_id/  (heavy artifacts stay in GCS)
> );
> ```
>
>
>

### **How the AI runs across environments.**

1.  **Choose the target.** The client may pass `env` explicitly, or the agent selects one with a **JEV** `Choice` over the `exec_environment` rows (§5) — e.g. "which env is healthiest / most representative for this scenario set?" — falling back to the LLM only on low confidence.

2.  **Register / refresh.** Health-probe `base_url`; **upsert** the `exec_environment` row (`last_seen`, `health`). New environment → new row, *dynamically*, on first sight — the DB grows as environments are tested, no pre-registration.

3.  **Run + record.** Execute the suite; write one `exec_run` row with the run summary, the real **signals** (§6), and per-scenario triage. Heavy artifacts (traces, screenshots) stay in **GCS**; Postgres holds the queryable metadata and a `trace_uri` pointer — the same GCS-heavy / SQL-light split the rest of the system uses.

4.  **Differential across envs.** With ≥2 `exec_run` rows for the same `context_id` on different environments/revisions, the oracle stack runs **differential mode** (§3.2): same input, two envs, diff the responses — any divergence is a regression, and *consensus is the oracle* (no ground truth needed). This is the concrete payoff of storing every tested environment: the second environment *is* the oracle for the first.

![[image-20260924-022949.png]]

### **Why not a new datastore.**

A Cloud Run redeploy once wiped in-flight GCS memory-bank state (memory: *Test-plan scenario-generator gotchas*); the task store moved to Cloud SQL precisely so run/task state survives. Putting the env ledger anywhere else re-opens a durability gap the system already closed. One engine, one pool, two tables.

## JEV judge & evaluation

The Executor makes a stream of **typed decisions** at runtime — *is this failure a real bug or a UI change? should this heal be accepted? is this test flaky? which environment do I target?* Those are **exactly** what the **JEV** `DecisionProvider` port (added beside `ModelProvider` — `common/adk/providers/decision.py`) is for. See `RESEARCH-jev-in-test-agent-v2.md`.

### **The port (already in-tree, default OFF).**

`DecisionProvider` exposes three primitives returning a typed `Verdict(value, probs, confidence)`:

|  |  |  |
|----|----|----|
| Primitive | Signature | Executor use |
| **Choice** | `choice(state, options, instructions) → Verdict` | Pick the **target environment** from the registry; route a failure to a triage bucket. |
| **Score** | `score(state, instructions, levels) → Verdict` | Grade **run health** / heal-risk on an ordinal scale (0–1 via `score01`). |
| **Noul** | `noul(state, statement) → Verdict` | Yes/no judgments: "*this failure is a product bug*", "*this test is flaky*", "*this heal preserves intent*". `value` is a bool, `probs` its distribution. |

### **The cascade (the important part).**

JEV **fronts** the LLM, it never replaces it — strictly additive:

> [!note]- Code details
>
>
>
> ```
> v = decision.noul(trace_state, "this failure is a real product bug")   # JEV: ~150ms, ~free
> if v.confidence >= THRESHOLD:      # ~90% of calls — fast typed path
>     verdict = v.value
> else:
>     verdict = llm_triage(trace_state)   # ~10% hard tail — existing LLM judge, unchanged
> ```
>
>
>

`get_decision_provider()` returns `None` when `TPD_DECISION_BACKEND` is unset → **every call site keeps its existing LLM path**. Worst case = today's behaviour. `TEV_NOUL_THRESHOLD` (0.5 default) is the semantic accept cut; calibrate the confidence gate against TEV's golden sets before trusting the fast path.

### **Where JEV lands in the Executor:**

|  |  |  |
|----|----|----|
| Site | Primitive | Replaces |
| **Triage classifier** (Bug / Heal / Flaky / Env) | Choice / Noul | an LLM call per failure over the trace |
| **Heal-accept** ("*patch preserves scenario intent*") | Noul | LLM judgment before surfacing the Yes/No gate |
| **Flakiness** ("*non-deterministic, not a real fail*") | Noul + run-history | a heuristic threshold |
| **Env selection / run-health** | Choice / Score | static "use dev" |

![[image-20260924-025022.png]]

### **Invariant kept (I8)**

The **deterministic** scorers stay LLM-free and JEV-free. JEV only touches the Executor's **runtime judgments** and TEV's **opt-in judged tiers** — never the deterministic product path (`evaluate_pack` / `evaluate_plan` / `metrics/*`). The Executor's real signals (§6) are *measurements*, not judgments; JEV classifies what to *do* with a failure, not whether a number is correct.

## How it extends the Test-Evaluation (TEV) stage

TEV today scores the pipeline output **without ever running it**, so several of its terms are honest **proxies** waiting for exactly this agent. The Executor's job on the TEV side is to **retire the proxies**.

### **The proxies TEV currently ships** (`src/test_evaluation`):

- `TPS_WEIGHTS` = `fault_detection 0.30 · brief_groundedness 0.25 · coverage 0.20 · oracle_strength 0.15 · trajectory 0.10` (`metrics/tps.py`).

- `metrics/oracle.py::oracle_strength` — "*deterministic proxy for mutation*": grades each step's `expected` string strong/medium/weak **without running anything** (`RESEARCH-test-oracle.md`).

- `metrics/mutation.py::fault_class_coverage` — "*fault-class-coverage PROXY (real mutation is gated on the execution stage)*": counts whether a fault class was *aimed at*, not whether a test *kills* it.

### **What the Executor feeds back:**

|  |  |  |
|----|----|----|
| TEV term / metric | Today (proxy) | With the Executor (real) |
| `fault_detection` (0.30 — the heaviest weight) | `oracle_strength` + `fault_class_coverage`, both static | **real mutation score** (mutmut / PIT / Stryker run against the suite) — mutants *killed*, not *aimed at* |
| `coverage` (0.20) | AC-coverage recall + matrix completeness, from the plan text | **executed coverage delta** — lines/branches the run actually hit |
| oracle strength (0.15) | classifier over `expected` strings | **stays** as its own signal, now *cross-checked* against which oracles actually caught injected faults |
| *(new)* schema/status/auth conformance | — | Schemathesis battery pass-rate per scenario |
| *(new)* flakiness×5 | — | non-determinism rate from the run ledger |

### **Mechanically, minimal surface change on TEV:**

- **A new metric module** `metrics/execution.py` (or the real body of `metrics/mutation.py`) reads the `exec_run.signals` row and returns real mutation / coverage-delta / conformance / flakiness scores.

- `fault_detection` **swaps its source**: when an `exec_run` exists for the `context_id`, the composite uses the real mutation score; otherwise it falls back to today's proxy. Same weight, better input — the TPS formula is unchanged, only the term's provenance improves.

- **New TEV bridge tool** `evaluate_run(context_id)` alongside `evaluate_pack` / `evaluate_plan`, and the existing `benchmark_run` / `compare_benchmarks` now compare **execution-grounded** runs across environments (join on `exec_run.environment_id`).

- **The assured loop closes.** The TPD assured loop's ②–③ *MEASURE* step is today stubbed by a judge-rubric score (`RESEARCH-tpd-assured-generation.md`, "*honest gap*"). The Executor supplies the real `builds ∧ passes×5 ∧ raises coverage ∧ kills mutant` gate — the generation loop stops trusting a proxy and starts gating on a run.

![[image-20260924-025321.png]]

## Reference — tooling landscape (compact)

|  |  |  |
|----|----|----|
| Layer | Pick | Why |
| Runner | **Playwright** + `playwright-bdd` / `behave` | runs the exported Gherkin unchanged |
| Step binding | **Playwright MCP** | a11y `ref`s, Claude-native, deterministic-mappable |
| UI discovery / heal | **Playwright Test Agents** (`--loop=claude`), **Healenium** | Planner/Generator/Healer; DOM-fingerprint heal |
| Vision fallback | browser-use · Skyvern · Midscene | canvas / cross-origin where a11y fails |
| API oracle | **Schemathesis** (spec = generator *and* oracle) | conformance battery for free off OpenAPI |
| Stateful / security | **RESTler** · RestTestGen | producer/consumer sequences + security checkers |
| Property data | **Hypothesis** (via Schemathesis) | boundary/malformed inputs, shrunk reproducers |
| Mutation | **mutmut** / **PIT** / **Stryker** | the *quality* number for TEV `fault_detection` |
| Visual oracle | Applitools Eyes | perceptual baseline for appearance regressions |
| Contracts | **Pact** | consumer-driven contracts where codegraph shows cross-service use |
| Reference loops | SWE-agent / Aider / OpenHands | the run→observe→fix agentic template |

## Caveats & source hygiene

- **New trust boundary.** Unlike the read-only KGA/TPD/TEV, the Executor holds test-env credentials and makes live calls / drives a browser — the biggest security delta of this agent. Sandbox it; store secret *references*, not values; allow-list egress per run. This is the primary review surface.

- **Maintenance flags.** **Dredd** (archived Nov 2024) and **Spring Cloud Contract** (archived) — prefer **Schemathesis** and **Pact**. **jqwik** in maintenance mode. Octomind "discontinued for new customers" is single-sourced/unverified. Qodo Cover "no longer maintained (~2025)" — vendor/fork, don't depend on upstream.

- **Vendor claims** (mabl "80–99% heal", Skyvern WebBench 64.4%) are self-reported — directionally credible, not independently verified here.

- **JEV calibration** is per-model and early-access — calibrate the cascade threshold on TEV goldens before trusting the fast path; re-check on any JEV model update. Data residency (JEV region vs `europe-west6`) before sending customer state.

- Star counts / versions are point-in-time (≈ Sept 2026).

### 9.
