---
title: "Evaluating Knowledge-Gathering Agent - V2"
created: 2026-09-07
updated: 2026-09-15
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49730683068/Evaluating+Knowledge-Gathering+Agent+-+V2
confluence_id: "49730683068"
confluence_path: "Team Kepler > AI-First Framework — Mission Team: Receive > Testing Agents > Agent Loop 1 - Knowledge Gathering - v2"
tags: [confluence, ai-agents]
---

# Evaluating Knowledge-Gathering Agent - V2

*Confluence source · Team Kepler › AI-First Framework — Mission Team: Receive › Testing Agents › Agent Loop 1 - Knowledge Gathering - v2 · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49730683068/Evaluating+Knowledge-Gathering+Agent+-+V2) · updated 2026-09-15*

## Overview

### **Purpose.**

The reports are about *building* capability into the Testing Agent.

This one is about **measuring** the first agent — the **Knowledge-Gathering Agent (KGA)** — so every future change to it can be judged against a **repeatable score** instead of a hand-wave.

It answers one question: *given a seed ticket, how do we know the pack the KGA assembled is good?*

It organizes the answer with a **three-layer evaluation framework** — measure the *artifact*, the *process* that produced it, and its *downstream effect* — and fills each layer with metrics from two established sources:

1.  **Google ADK Evaluation** — Google's Agent Development Kit ships an agent-eval harness whose metrics score two things a tool-using agent does: the **trajectory** (which tools it called, in what order) and the **final response** (is it correct, grounded, safe). These are the right lens for **Layer 2** — the KGA's *agentic control flow*: the `gather → refine → approve` tool sequence and the fan-out tier order.

2.  **RAGAS (Retrieval-Augmented Generation Assessment)** — the de-facto metric set for RAG systems, split into **retrieval** metrics (did we fetch the right context?) and **generation** metrics (is the answer faithful to what we fetched?). These are the right lens for **Layer 1** — the KGA's *pack content*: the memory self-seed, the Atlassian search, the `search_memory` tool, and the refine "understanding" over the pack.

### Sources

- [Google ADK — Why evaluate agents](https://google.github.io/adk-docs/evaluate/) · [Evaluation criteria reference](https://adk.dev/evaluate/criteria/)

- `adk-python` [v1.22.1](https://github.com/google/adk-python/blob/v1.22.1/src/google/adk/evaluation/eval_metrics.py) `eval_metrics.py` [(PrebuiltMetrics enum)](https://github.com/google/adk-python/blob/v1.22.1/src/google/adk/evaluation/eval_metrics.py)

- `adk-docs` [evaluate/index.md](https://github.com/google/adk-docs/blob/main/docs/evaluate/index.md)

- [RAGAS — List of available metrics](https://docs.ragas.io/en/stable/concepts/metrics/available_metrics/) · [Agentic / tool-use metrics](https://docs.ragas.io/en/stable/concepts/metrics/available_metrics/agents/)

- [Testing with ADK — Agent evaluations (Google Cloud, Medium)](https://medium.com/google-cloud/testing-with-agent-development-kit-agent-evaluations-76a9eec27965)

- [Evaluate Amazon Bedrock Agents with Ragas (AWS)](https://aws.amazon.com/blogs/machine-learning/evaluate-amazon-bedrock-agents-with-ragas-and-llm-as-a-judge/)

- [RAG Evaluation: Metrics, Tools, and the Context Gap (Atlan)](https://atlan.com/know/how-to-evaluate-rag-systems-explained/)

![[image-20260915-044853.png]]

## The gap and one principle

**The framework**

A pack can *look* excellent and still be useless downstream, so we measure it on three layers:

|  |  |  |  |
|----|----|----|----|
| Layer | What it measures | KGA question | Framework |
| **Layer 1 — Artifact** | the pack itself | right nodes, no junk; understanding grounded + on-task? | **RAGAS** retrieval + generation |
| **Layer 2 — Process** | how it got there | right tools, right order; goal achieved? | **ADK** trajectory + tool-use |
| **Layer 3 — Downstream** | the real signal | does this pack corrupt or enable the plan built on it? | noise/topic drift + the TPD handoff |

### Where we started.

- The KGA had **zero automated quality evaluation.**

- There were unit tests for wiring (`tests/`), but nothing scored *the pack a gather produces*.

- Regressions were caught only by a human noticing a bad run.

- Both are **quality** failures a metric would catch; neither is a crash a unit test catches.

### The gap

We couldn't answer "did this change make gathers *better* or *worse*?"

- No golden dataset

- no retrieval score

- no groundedness check

- no trajectory assertion

So every tuning decision on `_MAX_TERMS`, `max_web`, the explore-loop `_FOCUS_CAP`, or a new tier was **flown blind**.

### The principle.

> **Score the pack, not the vibes.**
>
> A gather agent is a *retrieval* system (Layer 1) with an *agentic* controller (Layer 2) whose output only matters by what it *enables* (Layer 3).
>
> Anchor every score to a small, human-curated **golden set** of seed → expected-pack pairs, and keep a few deliberately-bad **canary seeds** that must always score low — if a canary ever scores high, the eval pipeline is broken, not the agent.

## The three-layer framework (the organizing spine)

The framework comes from the spec: **a plan can look excellent and still catch no bugs — so measure the artifact, the process that produced it, and what it does in the field.** Applied to the KGA, whose "artifact" is a *pack*:

- **Layer 1 — Artifact quality (the pack).**

  - Everything about the output itself: did retrieval fetch the nodes a human QA would call relevant and no junk (**Context Precision / Recall / Entities**), and is the restated understanding grounded in those nodes with nothing invented and on-task (**Faithfulness / Response Relevancy / fabrication rubrics**).

  - This is where RAGAS lives, and it carries the heaviest PQS weight because our real incidents were bleed + hallucination.

- **Layer 2 — Agent process (how it got there).**

  - The controller's behaviour, not its output:

    - did it call the right tools in the right order (`gather → refine → approve`),

    - fire the right fan-out tiers,

    - hit the dev-panel it needs;

    - did the run achieve its goal;

    - is it consistent across N runs.

  - This is where ADK lives.

- **Layer 3 — Downstream effectiveness (the real signal).**

  - A pack is an *intermediate* artifact — its true quality is whether it *enables* a good test plan and doesn't *corrupt* it.

  - Layer 3 is the hardest to measure directly, so today it is a set of proxies:

    - **Noise Sensitivity** (does one irrelevant node change the understanding — the bleed, at the output level),

    - **Topic Adherence** (does the explore loop drift off-seed), and, ultimately, the **plan quality** the sibling TPD report scores on the pack this agent hands off.

Every metric below is tagged with its layer.

The composite PQS spans Layers 1–2 (Layer 3's downstream signal is scored by the TPD report, not folded into the PQS number).

> [!note]- Concepts & terms
>
> - **Pack / context pack** — everything a gather assembles for one `context_id`: the distilled **Notes**, the **link inventory** (`LinkRecord`s), declared **gaps**, and the `RunLog`. Persisted to the GCS memory bank.
>
> - **Trajectory** — the ordered sequence of tool/skill invocations an agent makes for one task. For the KGA: the MCP tool sequence (`gather_knowledge → refine → … → approve`) and, one level down, the internal tier order and crawl fetch-kind order.
>
> - **Golden set / evalset** — a small, human-curated set of `(seed, expected_pack, reference_understanding)` triples. The ground truth every metric scores against. ADK calls one file an **EvalSet**, one seed an **EvalCase**, one turn an **Invocation**.
>
> - **Canary seed** — a deliberately-degraded golden seed (a bled pack, a thin container) whose score *must* stay low. A canary scoring high means the metric or judge is broken, not the agent (from the HTML spec's calibration protocol, §11).
>
> - **Retrieval surface** (KGA) — everything that *fetches context*: the frontier crawl (`gather/crawl/crawl.py`), the memory self-seed and Atlassian search (`gather/explore/*`), and the `search_memory` / `get_note` read tools.
>
> - **Generation surface** (KGA) — everything that *writes prose from context*: the per-node distilled synopsis (`common.llm` distiller), the hypothesized terms, and — most important — the refine **understanding** brief.
>
> - **Reference-based vs reference-free** — a metric that needs a human-written ground truth vs one that scores from the run alone (faithfulness needs only answer+context, not a reference).
>
> - **LLM-as-judge** — a metric computed by prompting a judge model. In this system the judge is **provider-sourced**. Non-deterministic → sample N times, expect variance.
>
> - **Deterministic metric** — computed by code, no LLM (set overlap, exact trajectory match, substring recall). Cheap, stable, PR-gate-able.
>

## What "good" means for a gather agent

The KGA is not a chatbot; its "answer" is a **pack**. A good pack has five properties; each maps to a metric family and sits in one of the three layers:

|  |  |  |  |  |
|----|----|----|----|----|
| Property | Layer | Plain meaning | Failure we've actually seen | Metric family |
| **Complete** | 1 | it found the tickets/pages/repos a human QA would call relevant | 0-links crawl on a thin ticket (LUZ-159312: 1 node) | RAGAS **Context Recall**, **Entities Recall** |
| **Focused** | 1 | it did *not* drag in unrelated material | memory-bleed: billing seed pulled the ZIP-import cluster | RAGAS **Context Precision** (+ hard-negative gate) |
| **Grounded** | 1 | the understanding cites the pack; nothing invented | ungrounded external-LLM lead promoted | RAGAS **Faithfulness**, `cites_only_real_ids` / `no_invented_urls` rubrics |
| **On-task** | 1 | it answered *this* ticket, not a neighbour | refine restated the wrong domain | RAGAS **Response Relevancy** |
| **Well-driven** | 2 | the controller took the right tool path | skipped the dev-panel; a tier ran that shouldn't | ADK **tool_trajectory_avg_score**, Tool Call Accuracy |
| **Non-corrupting** | 3 | irrelevant context didn't change the answer, and the loop stayed on-seed | bleed changed the understanding; explore loop drifted to a 43-node wrong domain | **Noise Sensitivity**, **Topic Adherence** |

## The KGA as an evaluation target

- Before choosing metrics, pin down which stages are **deterministic** (trajectory metrics fit; expect exact scores) vs **stochastic** (LLM-judged; expect variance and sample).

- This matters because ADK's `tool_trajectory_avg_score` defaults to a hard **1.0** — only legitimate on the deterministic parts.

- Code paths below are the **current by-agent layout** (`gather/` owns crawl + explore; `refine/` owns the interrogation).

|  |  |  |  |
|----|----|----|----|
| Stage | Deterministic? | Layer | Notes |
| Seed probe | ✅ | 2 | one `get_issue` read |
| Hypothesize terms | ❌ LLM | 1 | always-on when Vertex configured |
| Memory self-seed | ✅ | 1 | pure index read, no LLM |
| Atlassian search | ✅ | 1 | no LLM |
| External leads → grounding gate | ❌ LLM → ✅ gate | 1 | leads stochastic; grounding deterministic |
| Frontier crawl | ✅ | 1 | bounded BFS; the executor |
| Per-node distill | ❌ LLM (or heuristic) | 1 | synopsis |
| Explore loop | ⚠️ mostly det. | 3 | focus-derivation is token-based, no LLM |
| Refine understanding | ❌ LLM | 1 | the headline "answer" |

**Two retrieval sub-systems, one report.**

- RAGAS was built for a single retriever→generator.

- The KGA has **two** retrievers feeding one generator:

  - the **crawl** (traverses the seed's neighborhood)

  - the **memory** (self-seed / search / `search_memory` over the GCS index).

- Evaluate them **separately** — the memory retriever is where the bleed happens, and it deserves its own precision/recall — then evaluate the **combined** pack that feeds refine.

## Layer 1 — Artifact quality (the pack) · RAGAS

The pack is the artifact. Layer 1 scores its *content* — retrieval on the left, generation on the right — and composes them into the PQS.

### Retrieval metrics — score the pack's node set

|  |  |  |  |
|----|----|----|----|
| Metric | Definition | Kind | KGA mapping |
| **Context Precision** | fraction of retrieved contexts that are actually relevant | **Det.** (set overlap) | of the pack's notes, how many are genuinely about *this* ticket. **Catches memory-bleed.** |
| **Context Recall** | fraction of the relevant contexts retrieved | **Det.** (set overlap) | of the human's known-relevant nodes, how many the gather found. **Catches the 0-links false-negative.** |
| **Hard-negative leak gate** | did any `must_not_retrieve` node appear? | **Det.** | the bleed guard — *any* appearance is a hard fail, scored 0. |
| **Context Entities Recall** | of the key entities in the ground truth, how many appear in retrieved text | **Det.** (substring) | did the pack surface the right LUZ keys, components, endpoints, repos? |

### Generation metrics — score the understanding

|  |  |  |  |
|----|----|----|----|
| Metric | Definition | Kind | KGA mapping |
| **Faithfulness** | fraction of claims in the answer supported by the retrieved context | **Judge** (RAGAS) | is every statement in the understanding traceable to a pack note? Same intent as ADK `hallucinations_v1`. |
| **Response Relevancy** | does the answer directly address the question | **Judge** (RAGAS) | does the understanding address *this ticket's* AC, not a neighbour's? |
| `cites_only_real_ids` | every Jira key named in the understanding belongs to a pack node | **Det.** (regex + set) | fabrication guard (output level); shipped in `metrics/rubrics.py`. |
| `no_invented_urls` | every URL in the understanding also appears in the pack text | **Det.** (regex) | fabricated-link guard; shipped in `metrics/rubrics.py`. |
| `names_the_ac` / `declares_gaps_honestly` | semantic rubrics: real AC named, gaps acknowledged | **Judge** (semantic) | declared as data (`SEMANTIC_RUBRICS`), run through an injected `judge(q, text) -> bool`. |

### The composite — Pack Quality Score (PQS)

For dashboards, one weighted mean, weighting **groundedness and precision highest** because our real incidents were bleed + hallucination, not missing recall:

```
PQS = 0.30·faithfulness + 0.25·ctx_precision + 0.20·ctx_recall + 0.15·relevancy + 0.10·trajectory
```

At **runtime** (`engine.py::evaluate_pack(bank, context_id, case=None)` — note: **no** `trajectory=` **kwarg**) the `PQSComponents` are filled from the persisted pack:

- `faithfulness` = the two fabrication rubrics both passing (`cites_only_real_ids ∧ no_invented_urls`, as `0.0/1.0`);

- `ctx_precision` / `ctx_recall` from `node_overlap`;

- `relevancy` from `entities_recall`;

- `trajectory` = **fixed** `1.0` — the tier trajectory needs the gather reply, which only the offline harness and the ADK-native runner have, so the runtime agent leaves it neutral and the harness scores it separately.

**Always emit the components** next to the number: a 0.04 PQS drop could be all faithfulness or all recall, and you need to know which surface regressed to act.

![[image-20260915-043446.png]]

## Layer 2 — Agent process · Google ADK Evaluation

Layer 2 scores the controller, not the pack. Since the ADK-native cutover the KGA **is** an ADK agent, so we can use ADK's own eval harness directly — the `eval/` subpackage — as well as our deterministic trajectory metric.

### The metric catalog

|  |  |  |  |
|----|----|----|----|
| Config key | Measures | Judge | Applies to KGA? |
| `tool_trajectory_avg_score` | exact match of the tool-call sequence (EXACT / IN_ORDER / ANY_ORDER) | deterministic | **YES, high value** — the tier + fetch order; catches the dev-panel/parent regression |
| `response_match_score` | ROUGE-1 word overlap vs reference | deterministic | LIMITED — only the deterministic count line |
| `final_response_match_v2` | LLM-judged **semantic** match to the reference | LLM | YES for refine (paraphrastic; beats ROUGE) |
| `hallucinations_v1` | splits the response into sentences, checks each is grounded | LLM | **YES, critical** — the refine understanding groundedness (Layer-1 cross-check) |
| `rubric_based_final_response_quality_v1` | LLM-judged quality against custom rubrics | LLM | YES — our `SEMANTIC_RUBRICS` map onto it (§10.3) |
| `safety_v1` | harmlessness | LLM / Vertex | LOW — read-only over internal Atlassian; a cheap floor |

As built, `eval/config.py` declares the ADK criteria: `PACK_METRICS` = `pqs_score` (threshold 0.70, our PQS as a custom metric) + `hard_negative_leak` (threshold 1.0, the bleed gate as a custom metric); the judged tier is `JUDGED_METRICS = (hallucinations_v1, rubric_based_final_response_quality_v1, final_response_match_v2)` at `JUDGED_THRESHOLD = 0.70`.

![[image-20260915-043657.png]]

## Layer 3 — Downstream effectiveness

A pack is an intermediate artifact; its real value is what it *enables* and what it *doesn't corrupt*. Layer 3 is the hardest to score directly, so it's a set of proxies today.

### **Noise Sensitivity — does one irrelevant node corrupt the answer?**

This is the **memory-bleed metric at the output level.**

- Precision (Layer 1) catches the irrelevant node being *retrieved*;

- noise sensitivity catches whether it actually *changed the understanding* — exactly the bleed incident.

- `metrics/noise.py::drift_score(baseline, perturbed, injected_terms)` returns a typed `NoiseScore` — the fraction of injected terms that surfaced in `perturbed` but not `baseline` (`0.0` = noise ignored).

- The harness runs it as a **causal** experiment: gather+refine a clean pack, inject the hard-negative into the seed's `issuelinks` so it enters a noisy pack, re-gather+refine, and measure the drift.

- Toggle a de-bias knob and watch the injected terms stop leaking.

### **Topic Adherence — does the explore loop stay on-seed?**

- The loop derives round-N focus from round N-1's node titles, which can **drift** into the memory gravity well.

- `metrics/topic.py::adherence(round_terms, seed_terms)` scores the fraction of a round's focus terms still on the seed's topic (deterministic token-overlap);

- `adherence_curve(rounds, seed_terms)` plots it per round — a monotone decline is drift; a cliff at round K says cap the loop at K.

- The payoff: the loop knobs become **tunable against a number** instead of by eye.

### **The plan handoff — the truest downstream signal.**

The pack exists to be turned into a test plan.

- Its ultimate quality is the TPD plan built on it.

- A high-recall but bled pack that produces a confidently-wrong plan is a Layer-1 pass and a Layer-3 failure; that is why the two reports are one program. (Human-acceptance of the pack — kept vs edited vs discarded — is the other Layer-3 proxy, not yet instrumented.)

- Layer 3 is **not** folded into the PQS number (it needs the downstream artifact), but its two shipped proxies gate the explore loop and the bleed guard.

![[image-20260915-043752.png]]

## The mapping — KGA stage × metric × layer

The table to implement against. "Det." = deterministic/cheap (PR gate). "Judge" = LLM-judged (nightly).

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| KGA stage / artifact | Layer | Primary metric(s) | Framework | Kind | Ground truth |
| Outer tool sequence `gather→refine→approve` | 2 | `tool_trajectory_avg_score` (IN_ORDER) | ADK | Det. | expected tool list |
| Inner tier order | 2 | trajectory + Tool Call Accuracy | ADK/RAGAS | Det. | expected tier list per seed shape |
| Crawl fetch calls (dev-panel, remote-links) | 2 | Tool Call Accuracy | RAGAS | Det. | expected fetch kinds |
| **Memory retriever** node set | 1 | Context Precision / Recall + hard-neg gate | RAGAS | Det. | golden relevant-id set |
| Memory retriever entity coverage | 1 | Context Entities Recall | RAGAS | Det. | golden entity list |
| **Combined pack** node set | 1 | Context Precision / Recall | RAGAS | Det. | golden relevant-id set |
| **Refine understanding** groundedness | 1 | Faithfulness / `hallucinations_v1` / `cites_only_real_ids` | RAGAS/ADK | Judge + Det. | — |
| Refine understanding correctness | 1 | `final_response_match_v2` | ADK | Judge | reference understanding |
| Refine understanding on-task | 1 | Response Relevancy | RAGAS | Judge | — |
| `summarize_gather` count line | 2 | `response_match_score` (ROUGE) | ADK | Det. | expected counts |
| Bleed at the output level | 3 | **Noise Sensitivity** (causal) | RAGAS | Det. | hard-negative terms |
| Explore-loop rounds | 3 | **Topic Adherence** curve | RAGAS | Det. | seed topic ref |
| The plan built on the pack | 3 | TPS (sibling report) | testing | Judge | golden plan |
| Whole run | 2 | Agent Goal Accuracy (approved, on-domain) | RAGAS | Judge | binary label |

## Judge calibration & canary seeds

- The LLM-judged tier (Faithfulness, `hallucinations_v1`, the semantic rubrics) is trustworthy only once the provider-sourced judge is **calibrated against humans**, and the deliberately-bad **canary seeds** (`golden/canary/`) are the cheap drift guard between recalibrations.

- The full protocol — Cohen's kappa, the `judge-human kappa >= human-human kappa - 0.1` gate, why a low human-human kappa is a rubric-wording bug (fix the rubric, not the judge), and the shipped canary implementation — is its own page: [https://axonivy.atlassian.net/wiki/x/MwCYlQs](https://axonivy.atlassian.net/wiki/x/MwCYlQs) .

- In short: keep a dimension **judged** only if the judge agrees with a human about as well as two humans agree with each other; otherwise keep it **deterministic** (which is why most of the PQS is set-overlap + regex). Pin the judge model (`judge_model_id()`) and re-calibrate on any model/prompt change.

## What the metrics would have caught (retro-fit to real incidents)

Each past KGA incident maps to a metric that would have flagged it **before** a human noticed — the strongest argument for building this:

|  |  |  |  |
|----|----|----|----|
| Incident (memory note) | Layer | Metric that flags it | How |
| **memory-bleed → wrong ticket** | 1 + 3 | Context Precision ↓, hard-neg leak, Noise Sensitivity ↑, Faithfulness ↓ | the billing seed's `must_not_retrieve_ids` lists the ZIP-import nodes; any appearance fails the gate, and the drifted understanding fails faithfulness + noise. |
| **0-links false-negative** (thin → 1 node) | 1 | Context Recall ↓ | the thin seed's `relevant_node_ids` has the parent + siblings; a 1-node crawl scores near-zero recall. |
| **crawler misses parent + dev panel** | 2 | `tool_trajectory_avg_score` ↓, Tool Call Accuracy ↓ | expected trajectory includes the dev-status + parent fetch; dropping them fails the order match. |
| **interrogation asks nothing** ("high confidence") | 2 | Agent Goal Accuracy = 0, Response Relevancy ↓ | an empty interrogation on a rich seed = goal not achieved; the "understanding" is generic. |
| **ungrounded lead promoted** | 1 | Faithfulness ↓, `cites_only_real_ids` fail | an ungrounded lead introduces unsupported claims / invented ids in the understanding. |
| **explore-loop drift** | 3 | Topic Adherence ↓ | round-over-round focus compared to the seed topic reference. |
