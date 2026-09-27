---
title: "Fallow analyses a JS-TS repo as a graph and exposes it to agents over MCP"
created: 2026-09-27
type: reference
status: seedling
source: "Confluence: Fallow Install and Usage Guide (TS)"
tags: [static-analysis, javascript, typescript, mcp, dead-code, tooling, confluence-distilled]
---

# Fallow analyses a JS-TS repo as a graph and exposes it to agents over MCP

**Fallow** is a Rust-native static-analysis tool for JavaScript/TypeScript that analyses a repo **as a graph rather than file-by-file**, answering questions linters structurally cannot: what changed, what got riskier, what should I review, what can be safely deleted.

```bash
npx fallow                      # zero install, zero config, from a repo root
npm install --save-dev fallow   # pins it; adds fallow, fallow-lsp, fallow-mcp
```

**What it covers:** dead code (unused files, exports, dependencies), duplication, complexity hotspots, dependency hygiene, circular dependencies, architecture-boundary violations, and PR risk gating.

**What it explicitly does not:** type checking (`tsc`), style and formatting (ESLint/Prettier), runtime debugging, verified security scanning. It **complements** those rather than replacing them.

> [!tip] The limitation *is* the feature
> Fallow is **syntactic only — it does not run a type checker**, and that is precisely what makes it fast (milliseconds) and **deterministic**. Worth internalising as a general principle: when evaluating an analysis tool, ask what it deliberately gave up, because that answer usually explains both its speed and the class of questions it can honestly answer. A tool that claims full type awareness *and* millisecond whole-repo analysis is claiming something expensive.

**Why it matters for agent-assisted work.** It ships `fallow-mcp`, an MCP server, plus an agent skill:

```bash
npx skills add fallow-rs/fallow-skills
```

so Claude or Cursor can run the analysis itself. That turns "which files does this change actually put at risk?" into something the agent can *query* rather than infer from reading — a graph answer instead of a guess, and deterministic enough to be worth trusting.

**Safety and licensing, both worth knowing before adoption:**

- **Read-only by default.** The only command that writes is `fallow fix`, and it requires `--yes`. Everything else is safe to explore with.
- **The static analysis is MIT and free**, usable in commercial and proprietary work. Only the optional **Fallow Runtime** layer (production runtime-coverage / hot-path features, activated via `fallow license activate`) is a separate paid product.
- Binaries come prebuilt from the **public npm registry**, so a private registry token is not involved — one fewer auth path to configure.

Requires Node 22.x / npm 10.x.

Related: [[A coding-agent prompt needs codebase anchors and stated house style]] — a tool like this is one way an agent gets those anchors without being told them.

Source: [[Fallow – Install & Usage Guide (Code Quality for JS TS)]] (TS, Confluence).

## Related

- [[A coding-agent prompt needs codebase anchors and stated house style]]
