---
ai_hash: c730d46ba4aca670
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49755127836'
confluence_path: 'Team Kepler > AI-First Framework — Mission Team: Receive > Testing
  Agents > Agent Loop 3 - Test-Plan Definition'
created: 2026-09-15
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- ai-agents
title: Evaluating the Test-Plan-Definition Agent
type: source
updated: 2026-09-15
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49755127836/Evaluating+the+Test-Plan-Definition+Agent
---

# Evaluating the Test-Plan-Definition Agent

*Confluence source · Team Kepler › AI-First Framework — Mission Team: Receive › Testing Agents › Agent Loop 3 - Test-Plan Definition · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49755127836/Evaluating+the+Test-Plan-Definition+Agent) · updated 2026-09-15*

## **Purpose.**

- The sibling report [Evaluating Knowledge-Gathering Agent - V2](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49730683068/Evaluating+Knowledge-Gathering+Agent+-+V2) answers *"given a seed ticket, how do we know the **pack** the KGA assembled is good?"*

- This one is its downstream twin: *given an approved insight pack, how do we know the **test plan** the second agent — the **Test-Plan-Definition Agent (TPD)** — defined and implemented is good?*

- Same goal, different artifact. Every future change to the TPD (the define interrogation, the coverage matrix, the Claude-on-Vertex scenario/step generators, the prose brief, the self-learning hooks) should be judged against a **repeatable score** instead of a hand-wave — so a regression is caught by a number, not a human eyeballing a `.feature` file.

It uses the same **three-layer evaluation framework** as the KGA report — measure the *artifact*, the *process* that produced it, and its *downstream effect* — and fills each layer from two sources:

1.  **Google ADK Evaluation** (**Layer 2 — process**) — the same agent-eval harness the KGA report adopts, reused because the TPD is *also* an ADK agent. ADK scores the **trajectory** (`define_plan → approve_plan → implement_plan`, and the inner `methodology → scope → metrics` round order) and the **final response** (is the brief correct, grounded, safe). RAGAS's **generation** metrics fold in here as the same-intent cross-check for "is the plan brief grounded in the pack?".

2.  **Test-suite adequacy & quality** (**Layer 1 — artifact**, and **Layer 3 — downstream**) — the body of software-testing science that answers *"is this a good test suite?"*: **coverage adequacy** (AC traceability, equivalence-partition + boundary coverage — ISTQB / ISO-IEC-IEEE 29119), **oracle strength** (does a scenario assert an end-state or just a status code?), and **BDD/Gherkin quality** — all measurable on the artifact (Layer 1); plus **fault-detection adequacy** (mutation score — Jia & Harman; PIT on the JVM) which needs the suite to actually *run*, so it is the **Layer-3** downstream signal, gated on the execution stage.

![[tpd-evaluation-adk-testsuite.png]]

## The framework, the gap, the principle

### **The framework.**

A suite can *look* comprehensive and still catch no bugs, so we measure it on three layers

|  |  |  |  |
|----|----|----|----|
| Layer | What it measures | TPD question | Lens |
| **Layer 1 — Artifact** | the plan + suite | scope grounded; every behaviour + partition covered; oracles that discriminate? | **test-suite adequacy** |
| **Layer 2 — Process** | how it got there | right `define→approve→implement` path + rounds; goal achieved? | **ADK** trajectory + tool-use |
| **Layer 3 — Downstream** | the real signal | would the suite actually *kill* a bug in the running system? | **mutation score** (gated on execution) |

### **Where we are.**

There was **zero automated quality evaluation of the plan** — unit tests for wiring, but nothing scored *the plan a define produces* or *the suite an implement produces*. Every real TPD defect was found by a human comparing artifacts by hand:

- the **define brief that ignores both the answers and the pack** (it auto-scoped LUZ-159312 to the *excluded* nodes and said "nothing out of scope" — the inverse of the approved understanding);

- the **silent heuristic fallback** that shipped 3 of 6+ behaviours when the LLM JSON truncated (looked full, was half-empty);

- the **execution-depth gap** — a codegraph-grounded plan with real *structure* but placeholder oracles, benchmarked against a human-authored, *executed* plan (~83 cases, real endpoints/states) that was far stronger;

- the **empty interrogation** ("high confidence", 0 questions) on a rich pack.

Every one is a **quality** failure a metric catches; none is a crash a unit test catches.

### **The gap**

We can now answer *"did this change make the plan better or worse?"* on the artifact and the process; the Layer-3 downstream signal (real mutation) stays blocked on the execution stage and runs as a proxy until then.

### **The principle.**

> **Grade the plan by what it would catch, not by how much it produced.**
>
> - A test-plan agent is a *judgement* engine (the brief) feeding a *generative* engine (the scenarios/steps/data).
>
> - A big suite that misses the real failure modes is worse than a small one that catches them — so score **coverage of behaviours and faults**, not scenario count.
>
> - Anchor every score to a small human-curated golden set of pack → reference-plan pairs — and where one exists, to a **human-authored, executed** plan as the ground truth.
>
> Keep deliberately-bad **canary packs** that must always score low.

## The three-layer framework (the organizing spine)

Same spine as the KGA report, applied to a *plan + suite*:

- **Layer 1 — Artifact quality (the plan + suite).** Everything about the output itself, all measurable **without running anything**: is the brief's **scope** grounded in the pack (no invented or excluded nodes); does the scenario set **cover** every in-scope behaviour and its negative/boundary/error partitions with real **traceability**; would a scenario **discriminate** — a real end-state oracle, not `assert 200`; are the artifacts **real** (no placeholder leak) and the Gherkin **well-formed**. This is test-suite adequacy science and it carries the heaviest TPS weight.

- **Layer 2 — Agent process (how it got there).** The controller: the `define→approve→implement` trajectory, the `methodology→scope→metrics` round order, interrogation yield, goal accuracy. ADK's lens.

- **Layer 3 — Downstream effectiveness (the real signal).** Whether the suite actually *kills bugs* in the running system: **mutation score** (inject faults, count kills), defect-detection rate vs baseline, and human acceptance (cases kept vs edited vs deleted). All need the suite to *run* → **gated on the execution stage** (Pillar 2). Until then, Layer 3 is a proxy: oracle strength + fault-class coverage stand in for mutation.

Every metric below is tagged with its layer. The TPS composite spans Layers 1–2; the `fault_detection` term is the Layer-1 **proxy** today (oracle + fault-class coverage) and becomes the Layer-3 **real mutation** score once the execution stage lands.

> [!note]- Concepts & terms (glossary)
>
> - **Plan / TestPlan** — the Stage-A artifact for one `context_id`: structured `methodology`, `scope`, `out_of_scope`, `metrics` (what "passed" means), `confidence`, `source_refs`, `status`, plus a human-facing **brief**. Persisted to the bank at `test-plan/<ctx>/plan.json` + `plan-brief.md`.
>
> - **Suite** — the Stage-B artifacts: `TestData`, `TestScenario`s, `TestStep`s, and the exported Gherkin `.feature`. Persisted at `test-plan/<ctx>/{scenarios,steps,test-data}.json`.
>
> - **Coverage matrix** — per in-scope behaviour, the scenario **kinds** the agent generates: `_FULL_MATRIX = ["happy","negative","boundary","error"]` (the eval-side partition model in `plan_engine.py`), downgraded to happy-only when a metric string literally says "happy only". The equivalence-partitioning + boundary-value-analysis surface.
>
> - **Behaviour / AC** — one acceptance-criterion proxy in the pack: a **grounded note** (`pack.grounded`). Scenarios are generated one-per-behaviour × the matrix, each carrying `source_refs=[note.id]` back to the behaviour it covers. When the golden case omits `behaviours`, the engine derives them from `pack.grounded` with the full partition matrix.
>
> - **Traceability** — the bidirectional link AC↔scenario. Forward: does every in-scope AC have a scenario? Backward: does every scenario cite a real pack node? `coverage_scores` computes it as `1 − untraceable/total` (a scenario is untraceable when none of its `source_refs` is in the real pack id set).
>
> - **Oracle** — what a scenario *asserts*. A **strong** oracle verifies a concrete end-state; a **weak** one asserts only a status code / "accepted". Oracle strength is the difference between a test that catches a regression and one that always passes.
>
> - **Golden set / plan-evalset** — a human-curated set of `(pack, reference_plan)` cases; the ground truth every metric scores against. The star anchor is a **human-authored, executed** plan (LUZ-156281).
>
> - **Canary pack** — a deliberately-bad golden pack (a bled brief, a thin pack) whose TPS *must* stay low; a canary scoring high means the metric or judge is broken, not the agent (§11 calibration).
>
> - **Mutation score** — fraction of injected faults (mutants) the suite would **kill**. The gold standard for fault-detection adequacy; needs runnable tests → the Layer-3 signal, gated on the execution stage.
>
> - **Deterministic vs judged** — deterministic metrics are computed by code over ids/sets/structure → cheap, stable, PR-gate-able. Judged metrics prompt the **provider-sourced** LLM (faithfulness, oracle depth) → non-deterministic, sampled, nightly.
>

## The TPD as an evaluation target

The TPD is **two engines** in series, and they demand different metrics:

- a **judgement engine** (Stage A / define) — an interrogation (`methodology → scope → metrics`) that distills human calls into a structured plan. Output: *decisions* + a *brief*. Score it for groundedness, scope correctness, honest confidence.

- a **generation engine** (Stage B / implement) — expansion of the confirmed plan into a suite. Output: *scenarios/steps/data*. Score it for coverage, traceability, oracle strength, and (eventually) fault detection.

Pin down deterministic vs stochastic stages — ADK's `tool_trajectory_avg_score` defaults to a hard **1.0**, only legitimate on the deterministic skeleton:

|  |  |  |  |  |
|----|----|----|----|----|
| Stage | Deterministic? | Engine | Layer | Notes |
| Round order `methodology→scope→metrics` | ✅ | Judgement | 2 | fixed dependency order |
| Question generation per round | ⚠️ LLM-or-heuristic | Judgement | 2 | falls back to heuristic on empty — *which path?* |
| Decision distillation | ✅ | Judgement | 2 | human→decision(high); self→assumption(low) |
| Plan assembly (scope/method/metrics) | ✅ | Judgement | 1 | from decision rounds |
| Confidence | ✅ | Judgement | 1 | opens→low; assumption→medium; else high |
| Prose brief | ⚠️ LLM-or-heuristic | Judgement | 1 | the headline "answer" of Stage A |
| Test-data generation | ⚠️ | Generation | 1 | placeholders `<generated>`/`<expected>` in heuristic |
| Scenario generation | ⚠️ LLM-or-heuristic | Generation | 1 | matrix per behaviour; **silent fallback risk** |
| Step generation | ⚠️ | Generation | 1 | Given/When/Then |
| Gherkin export | ✅ | Generation | 1 | tags `@kind @methodology` |
| Provenance edges | ✅ | Generation | 1 | scenario→insight edges (traceability) |

## Layer 1 — Artifact quality (the plan + suite) · test-suite adequacy

- This is the heart of the report — the lens the KGA did not need.

- A test-plan agent must be judged the way any test suite is judged.

- Five families, all measurable **without running anything**, feeding the TPS.

### Brief groundedness — is the scope true to the pack?

The brief is the known-broken surface (it once auto-scoped to the *excluded* nodes). Score it two ways, then average:

|  |  |  |  |
|----|----|----|----|
| Metric | Definition | Kind | TPD mapping |
| **Scope Precision / Recall** | overlap of `plan.scope` with the golden in-scope ids | **Det.** (set overlap) | reuses `node_overlap.retrieval_scores(scoped, in_scope, must_not_scope)` — **no separate** `scope_overlap.py`. |
| `must_not_scope` **leak gate** | did the scope include any excluded id? | **Det.** | the define-brief-scoping bug guard — *any* appearance is a hard fail (`scope.leaked`). |
| **Brief fabrication rubrics** | brief cites only real ids / no invented URLs | **Det.** (regex + set) | reuses `rubrics.cites_only_real_ids` / `no_invented_urls` over the brief. |
| **Faithfulness /** `hallucinations_v1` | fraction of brief claims supported by the pack | **Judge** | RAGAS/ADK second opinion; provider-sourced. |
| **Response Relevancy** | does the brief address *this pack's* subject | **Judge** | RAGAS. |

### Coverage adequacy — did the suite cover the behaviours and their partitions?

|  |  |  |  |
|----|----|----|----|
| Metric | Definition | Kind | TPD mapping |
| **AC-Coverage Recall** | of the in-scope behaviours, how many have ≥1 scenario tracing to them | **Det.** | grounded notes appearing in some scenario's `source_refs`. **Catches the silent-fallback drop** (3 of 6 → 0.5). |
| **Coverage-Matrix Completeness** | of the required partitions per behaviour, how many are present | **Det.** | for each behaviour, are `happy/negative/boundary/error` scenarios present? Scores `_FULL_MATRIX` vs what shipped. |
| **Traceability** | every scenario resolves to a real pack node | **Det.** | `1 − untraceable/total` (a scenario is untraceable when no `source_ref` is a real pack id). |

### Oracle strength — would a scenario discriminate?

A test that asserts only `2xx` kills almost no mutant; a test that asserts a real end-state kills many. `metrics/oracle.py::oracle_strength(steps, pass_criteria)` classifies each step's `expected`:

- **strong (1.0)** — names a concrete, resolvable end-state: an enum-shaped token (`[A-Z][A-Z0-9_]{3,}`, e.g. `CREDIT_CARD_CHARGED_PENDING`) or a match against the plan's `pass_criteria`;

- **weak (0.0)** — asserts only a status code / "accepted" / "succeeds" / "is processed" (the `_WEAK` list);

- **medium (0.5)** — anything in between.

It returns a typed `OracleScore(score, distribution, weak)` — the mean weight, the strong/medium/weak counts, and the weak assertions to fix. Measurable *without running anything* and a strong predictor of mutation score; it directly encodes the **execution-depth gap**. [https://axonivy.atlassian.net/wiki/x/YICalQs](https://axonivy.atlassian.net/wiki/x/YICalQs)

### Executability / validity — are the artifacts real or placeholders?

- `metrics/placeholders.py::placeholder_scan(scenarios, steps, test_data, *, detail)` is the validity gate.

- It scans for `<generated>` / `<expected>` / `<test-tenant>` / `PASS_METRIC` and infers **provenance** (heuristic if a kind-suffix title is present, else llm).

- On a `detail=True` run, a leaked token **or** heuristic provenance fails it — that is the silent-fallback catch (a `detail` run that shipped heuristic artifacts).

- Returns a typed `PlaceholderReport(passed, leaked_tokens, provenance)`.

### BDD / Gherkin quality — is the `.feature` well-formed?

- `metrics/gherkin_lint.py::gherkin_lint(feature_text)` returns a typed `GherkinReport(passed, scenarios, tagged, issues)`:

- it asserts a `Feature:` header, that every `Scenario:` carries **≥2 tags** (`@kind @methodology`), and that Given/When/Then steps are present. (It is scored in the judged/quality tier; `evaluate_plan` leaves `PlanReport.gherkin` `None` unless wired in — the lint runs in `test_eval_tpd_*` over the exported feature.)

- Redundancy — `(source_ref, kind)` uniqueness first (the heuristic guarantees it; the LLM path can duplicate), semantic near-duplicate detection second — is the minimality companion.

![[image-20260915-061619.png]]

### The composite — Test-Plan Score (TPS)

One weighted mean for dashboards (`metrics/tps.py`), weighting the **silent-failure** surfaces highest:

```
TPS = 0.30·fault_detection      # oracle+fault-class proxy now; real mutation once executable (Layer 3)
    + 0.25·brief_groundedness   # (brief_ok + scope.precision) / 2
    + 0.20·coverage             # ac_recall × matrix_completeness
    + 0.15·oracle_strength      # mean oracle weight over the steps
    + 0.10·trajectory           # fixed 1.0 at runtime (the harness scores the real trace)
```

As built, `evaluate_plan` fills `TPSComponents`:

- `fault_detection = fault.coverage` (the fault-class proxy),

- `brief_groundedness`,

- `coverage`,

- `oracle_strength = orc.score`,

- `trajectory = 1.0` (neutral — the tier trace lives in the offline harness / ADK runner, not the runtime path).

**Always emit the components** next to the number: a 0.04 TPS drop could be all oracle-strength or all coverage.

While mutation is a proxy, `fault_detection` and `oracle_strength` measure the same thing at different fidelities — re-split once real mutation lands (§4.7 / Layer 3).

### Fault-detection *proxy* (fault-class coverage) — the Layer-1 stand-in for Layer-3 mutation

- `metrics/mutation.py::fault_class_coverage(scenarios, behaviours)` is the honest proxy until the execution stage exists.

- Each behaviour lists its **known fault classes** in the golden set (e.g. "retry fires on the wrong `failCount` day", "QR fallback wrongly removed for a COMPANY tenant");

- a class counts as *aimed-at* when some **non-happy** scenario `source_refs` the behaviour.

- Returns `FaultClassScore(coverage, covered, missing)`.

- It is a checklist stand-in for mutation — measurable now, and the term real mutation replaces in Layer 3.

## Layer 2 — Agent process · Google ADK Evaluation

Layer 2 scores the controller, not the plan. Since the ADK-native cutover the TPD **is** an ADK agent, so ADK's own harness (the `eval/` subpackage, §8.2) applies directly, alongside our deterministic trajectory metric.

### The metrics that apply to the TPD

|  |  |  |  |
|----|----|----|----|
| ADK config key | Measures | TPD mapping | Value |
| `tool_trajectory_avg_score` | exact match of the tool-call sequence | outer `define_plan → approve_plan → implement_plan`; inner `methodology → scope → metrics` | **HIGH** — deterministic, regression-prone |
| `hallucinations_v1` | each response sentence grounded in the context | is every claim in the **brief** traceable to a pack note? | **CRITICAL** (Layer-1 cross-check) |
| `final_response_match_v2` | LLM-judged semantic match to a reference | brief vs a **reference brief** (paraphrastic — beats ROUGE) | HIGH for the brief |
| `response_match_score` | ROUGE-1 overlap vs reference | only the deterministic summary line | LIMITED |
| `rubric_based_final_response_quality_v1` | LLM-judged quality against custom rubrics | the TPD-specific rubrics in §8.3 | **HIGH, later** |

As built, `eval/config.py` declares `PLAN_METRICS` = `tps_score` (threshold 0.70, our TPS as a custom metric) + `must_not_scope_leak` (threshold 1.0, the scope-leak gate as a custom metric), and the shared judged tier (`JUDGED_METRICS` at 0.70). Interrogation Yield (≥1 open question on a rich pack) and Agent Goal Accuracy (confirmed, on-scope, non-empty suite) are the Layer-2 binary labels.

### Deterministic trajectory

Our own `metrics/trajectory.py::trajectory_score` (agent-agnostic, shared with the KGA) scores the outer tool sequence and the inner round order `in_order` from the harness-derived trace. This is the deterministic PR gate on the TPD control flow — a reordered round turns it red.

![[image-20260915-062023.png]]

## Layer 3 — Downstream effectiveness (gated on execution)

This is the truest layer and the one that separates a *plausible* suite from an *effective* one — and it is the one that isn't fully built, because it needs the suite to *run*.

- **Mutation Score (the gold standard).** Inject small faults (mutants) into the system under test and measure the fraction the suite **kills**. Established since Jia & Harman; on the JVM (the luz stack is Java) the tool is **PIT/pitest**. Requires the scenarios to *run* — the execution stage from `RESEARCH-test-executor-agent.md` Pillar 2. **Not available today.** When it is, mutation score becomes the `fault_detection` term (replacing the §4.7 proxy) and the TPS's heaviest.

- **Defect-detection rate** — bugs found when the plan is executed vs a baseline plan. Also execution-gated.

- **Human acceptance** — of the generated cases, how many a QA lead keeps as-is vs edits vs deletes. A cheap downstream proxy that needs no execution but needs a human review loop; not yet instrumented.

- **Escaped defects** — production bugs in areas the plan claimed to cover. The ultimate signal, longest loop.

Until the execution stage lands, Layer 3 is represented by the **oracle strength + fault-class coverage proxy** (§4.3 / §4.7) — measurable without running anything and a strong *predictor* of mutation score. The report says so at every step: the `fault_detection` TPS term is a proxy today, and the execution stage is the clean seam between this report and its sibling — the KGA scores retrieval, this one scores planning, both hand off to the execution pillar.

![[image-20260915-062013.png]]

## The mapping — TPD stage × metric × layer (core deliverable)

"Det." = deterministic (PR gate). "Judge" = LLM-judged (nightly). "Exec" = needs the execution stage (gated).

|  |  |  |  |  |
|----|----|----|----|----|
| TPD stage / artifact | Layer | Primary metric(s) | Kind | Ground truth |
| Outer `define→approve→implement` | 2 | `tool_trajectory_avg_score` (IN_ORDER) | Det. | expected tool list |
| Inner rounds `methodology→scope→metrics` | 2 | `tool_trajectory_avg_score` | Det. | expected round order |
| Interrogation yield | 2 | questions raised / pack-richness + Goal Accuracy | Det.+Judge | ≥1 open question on a rich pack |
| Plan **scope** | 1 | **Scope Precision/Recall** + `must_not_scope` gate | Det. | golden in-scope + excluded ids |
| Plan **brief** groundedness | 1 | fabrication rubrics (Det.) + Faithfulness / `hallucinations_v1` (Judge) | Det.+Judge | — |
| Plan **brief** correctness | 1 | `final_response_match_v2` | Judge | reference brief |
| Plan **confidence** honesty | 1 | rubric: confidence matches open-gap count | Det. | — |
| **Scenarios: AC coverage** | 1 | **AC-Coverage Recall** | Det. | golden behaviours |
| **Scenarios: partitions** | 1 | **Coverage-Matrix Completeness** | Det. | golden partitions per behaviour |
| **Scenarios: precision** | 1 | scope-based; `must_not_scope` gate | Det. | golden in/excluded ids |
| Scenarios: traceability | 1 | `1 − untraceable/total` | Det. | pack id set |
| Steps: oracle | 1 | **Oracle Strength** distribution | Det.+Judge | golden pass-criteria |
| Steps: concreteness | 3 | **execution-depth**: real endpoint/state vs placeholder | Judge | reference executed plan |
| Test-data | 1 | placeholder-leak + provenance | Det. | — |
| Gherkin `.feature` | 1 | BDD/Gherkin lint (tags, structure) | Det. | — |
| **Whole suite: fault detection** | 1→3 | fault-class coverage (proxy now) → **Mutation Score** (real) | Det. → Exec | golden fault classes |
| Whole run | 2 | Agent Goal Accuracy | Judge | binary label |

## What the metrics would have caught (retro-fit to real incidents)

|  |  |  |  |
|----|----|----|----|
| Incident (from project history) | Layer | Metric that flags it | How |
| **Define brief ignores answers AND the pack** | 1 | Scope Precision ↓, `must_not_scope` leak, Faithfulness ↓ | the golden pack lists the excluded siblings in `must_not_scope_ids`; scoping them is an instant hard fail; the brief citing them fails faithfulness. |
| **Silent heuristic fallback** (LLM JSON truncated → 3 of 6+ behaviours) | 1 | AC-Coverage Recall ↓, provenance = heuristic | 6 golden behaviours, 3 in `source_refs` → recall 0.5; the shipped titles are kind-suffix strings → provenance "heuristic" contradicts a `detail` run. |
| **Execution-depth gap** (placeholder oracles vs the human plan's real states) | 1 → 3 | Oracle Strength ↓, exec-depth rubric ↓, (later) Mutation Score ↓ | steps asserting `2xx`/`PASS_METRIC` score weak/medium; the executed plan asserts `CREDIT_CARD_CHARGED_PENDING` → strong; fault-class coverage shows the QR-COMPANY class untested. |
| **Empty interrogation** ("high confidence", 0 questions) | 2 | Interrogation Yield ↓, Goal Accuracy = 0 | a rich golden pack expects ≥1 open question; zero is a false-positive smell. |
| **Happy-only silent narrowing** | 1 | Coverage-Matrix Completeness ↓ | the metric checks *actual* partition presence per behaviour, not the `"happy only"` substring. |
| **Placeholder leak on a** `detail` **run** | 1 | placeholder-leak = fail | deterministic regex; a `detail=True` run that leaks a placeholder has silently fallen back to the heuristic. |

## Judge calibration & canary packs

The judged tier (RAGAS brief Faithfulness / Response Relevancy, `hallucinations_v1`, the LLM oracle-depth classifier) is trustworthy only once the provider-sourced judge is **calibrated against humans**, and the deliberately-bad **canary packs** (`golden_plans/canary/`) are the cheap drift guard between recalibrations. The full protocol — Cohen's kappa, the `judge-human >= human-human - 0.1` gate, and the shipped canary implementation — is its own page: [Judge Calibration and Canary Seeds](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49754406963/Judge+Calibration+and+Canary+Seeds) .

In short: keep a dimension **judged** only if the judge agrees with a human about as well as two humans agree with each other; otherwise keep it **deterministic** (which is why most of the TPS is set-overlap + partition presence). Pin the judge model and re-calibrate on any model/prompt change.

%% ai-graph-start %%

**Related notes:**
- [[Agent Loop 3 - Test-Plan Definition]]
- [[Evaluating Knowledge-Gathering Agent - V2]]
- [[Test-Plan Definition Agent]]
- [[Test oracle - what a scenario asserts, and why its strength decides everything]]
- [[Agent Loop 4 - Test-Plan Execution]]

%% ai-graph-end %%