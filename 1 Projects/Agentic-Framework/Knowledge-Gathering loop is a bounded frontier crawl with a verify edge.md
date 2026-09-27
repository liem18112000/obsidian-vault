---
ai_hash: f3fd0ce12052e6da
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-27
entities:
- Knowledge-Gathering loop
- bounded frontier crawl
- verify edge
- agent
- Seed
- Fetch
- Extract links
- Classify
- follow
- record-only
- Expand frontier
- visited set
- Distill to a markdown note
- Persist
- scraper
- Bounded
- frontier empty
- depth reached
- max_nodes
- max_seconds
- N consecutive "dry" rounds
- link-rich graph
- Dedup
- canonical URL
- cycles
- Testing-Agent
- transient failure
- memory
- Nothing dropped silently
- load-bearing design rule
- out-of-scope link
- broken link
- unreachable node
- declared gap
- omission
- flagged row
- absent one
- Distill-then-persist per node
- transcript
- curated facts
- markdown note
- provenance frontmatter
- links table
- run-log line
- inputs
- sources
- output
- confidence
- LUZ-159671
- GCS markdown memory bank
- A remote A2A agent
- connectors
- MCP
source: session 2026-08-27 — test-agent proposal
status: seedling
tags:
- agentic
- crawl
- loop-spec
- luz-159671
title: Knowledge-Gathering loop is a bounded frontier crawl with a verify edge
type: model
---

# Knowledge-Gathering loop is a bounded frontier crawl with a verify edge

The **Knowledge-Gathering** loop of an agent that has to "read everything linked from a seed" is best modeled as a **bounded frontier crawl**, not a single fetch:

`Seed → Fetch → Extract links → Classify (follow vs record-only) → Expand frontier (dedup via a `visited` set) → Distill to a markdown note → Persist → Verify edge → (loop | return)`.

Key properties that make it correct rather than just a scraper:

- **Bounded.** Terminate when ANY holds: frontier empty · depth reached · `max_nodes` · `max_seconds` · N consecutive "dry" rounds (nothing new discovered). Without this it never converges on a link-rich graph.
- **Dedup with a visited set** keyed on the *canonical* URL, so cycles (A links B links A) do not loop forever and re-fetch.
- **Verify edge** (borrowed from the loop-spec of the parent Testing-Agent story): before returning, cheaply re-check suspect/broken links so a transient failure does not poison memory — re-check, do not blindly re-crawl.
- **Nothing dropped silently** — the load-bearing design rule. An out-of-scope link is *recorded* (not followed); a broken link is *flagged*; an unreachable node becomes a *declared gap*, never an omission. A gap is a flagged row, not an absent one.
- **Distill-then-persist per node** turns a transcript into *curated facts* — one markdown note per node with provenance frontmatter + a links table, plus a run-log line (inputs/sources/output/confidence).

Context: designed for the LUZ-159671 Testing-Agent first slice (Atlassian → GCS markdown memory bank).

## Related

- [[A remote A2A agent needs its own connectors because MCP is client-side]]

%% ai-graph-start %%

**Related notes:**
- [[A link-following crawl pulls in graph-adjacent but topically-tangential nodes]]
- [[Converge an exploration loop on marginal yield (zero new items), not a fixed iteration count]]
- [[Ground-then-refine gathering grounds, refinement interprets and confirms]]
- [[Extracting every link from Jira ADF and Confluence storage]]
- [[A remote A2A agent needs its own connectors because MCP is client-side]]

**Relations:**
- Knowledge-Gathering loop — *is a* — bounded frontier crawl
- Knowledge-Gathering loop — *has* — verify edge
- agent — *uses* — Knowledge-Gathering loop
- Knowledge-Gathering loop — *modeled as* — bounded frontier crawl
- Knowledge-Gathering loop — *starts with* — Seed
- Knowledge-Gathering loop — *includes step* — Fetch
- Knowledge-Gathering loop — *includes step* — Extract links
- Knowledge-Gathering loop — *includes step* — Classify
- Knowledge-Gathering loop — *includes step* — Expand frontier
- Knowledge-Gathering loop — *includes step* — Distill to a markdown note
- Knowledge-Gathering loop — *includes step* — Persist
- Expand frontier — *uses* — visited set
- Classify — *has option* — follow
- Classify — *has option* — record-only
- Knowledge-Gathering loop — *has property* — Bounded
- Bounded — *distinguishes from* — scraper
- Bounded — *terminates on* — frontier empty
- Bounded — *terminates on* — depth reached
- Bounded — *terminates on* — max_nodes
- Bounded — *terminates on* — max_seconds
- Bounded — *terminates on* — N consecutive "dry" rounds
- Bounded — *enables convergence on* — link-rich graph
- Knowledge-Gathering loop — *uses* — Dedup
- Dedup — *uses* — visited set
- visited set — *keyed on* — canonical URL
- Dedup — *prevents* — cycles
- verify edge — *borrowed from* — Testing-Agent
- verify edge — *re-checks* — broken link
- verify edge — *prevents* — transient failure
- transient failure — *poisons* — memory
- Knowledge-Gathering loop — *follows rule* — Nothing dropped silently
- Nothing dropped silently — *is a* — load-bearing design rule
- out-of-scope link — *is* — recorded
- out-of-scope link — *is not* — followed
- broken link — *is* — flagged
- unreachable node — *becomes* — declared gap
- declared gap — *is not* — omission
- declared gap — *is a* — flagged row
- declared gap — *is not* — absent one
- Distill-then-persist per node — *transforms* — transcript
- Distill-then-persist per node — *produces* — curated facts
- curated facts — *are* — markdown note
- markdown note — *has* — provenance frontmatter
- markdown note — *has* — links table
- markdown note — *has* — run-log line
- run-log line — *includes* — inputs
- run-log line — *includes* — sources
- run-log line — *includes* — output
- run-log line — *includes* — confidence
- Knowledge-Gathering loop — *designed for* — LUZ-159671
- LUZ-159671 — *is a* — Testing-Agent
- Testing-Agent — *uses* — GCS markdown memory bank
- Knowledge-Gathering loop — *related to* — A remote A2A agent
- A remote A2A agent — *needs* — connectors
- MCP — *is* — client-side

%% ai-graph-end %%