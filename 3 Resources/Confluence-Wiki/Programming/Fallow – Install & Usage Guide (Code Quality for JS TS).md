---
title: "Fallow – Install & Usage Guide (Code Quality for JS/TS)"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/49471750190/Fallow+Install+Usage+Guide+Code+Quality+for+JS+TS
space: "TS"
topic: programming
relevance: 0.823
depth: 2.92
updated: 2026-06-03
attachments: 3
tags:
  - confluence
  - programming
  - space/ts
---

# Fallow – Install & Usage Guide (Code Quality for JS/TS)

> [!info] Imported from Confluence
> Space **TS** · updated 2026-06-03 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/49471750190/Fallow+Install+Usage+Guide+Code+Quality+for+JS+TS)
> Relevance 0.823 · topic `programming`

**Fallow** is a Rust-native, deterministic codebase-intelligence tool for JavaScript & TypeScript. It reports code quality, changed-code (PR) risk, dead code, duplication, complexity hotspots, dependency hygiene, and architecture-boundary issues — in milliseconds, with no AI inside the analyzer. This page is the team guide for installing and using it on our front-end repos (`luz_next` and `luz_admin_ui`) and wiring it into AI coding agents.

<div hasbody="true" macro-id="164f5d3d-def8-462e-9140-c250fc387912" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

**TL;DR** — Run `npx fallow` from a repo root. Nothing to install, zero config. Optionally install the fallow *agent skill* with one command (`npx skills add fallow-rs/fallow-skills`) so Claude / Cursor can drive it for you.

</div>

</div>

## 1. What it is (and isn't)

Fallow analyses the repository as a graph, not file-by-file. It answers: what changed, what got riskier, what should I review, what should I refactor, what can be safely removed. It complements — does not replace — `tsc` and ESLint.

- **Use it for:** dead code (unused files/exports/deps), duplication, complexity hotspots, dependency hygiene, circular dependencies, architecture boundaries, PR risk gating.

- **Don't use it for:** type checking (use `tsc`), style/formatting (ESLint/Prettier), runtime debugging, or verified security scanning.

- **Limitation:** syntactic analysis only (no type-checker), which is what makes it fast and deterministic.

<div class="panel conf-macro output-block" hasbody="true" macro-id="" macro-name="panel" style="background-color: #EAE6FF;border-color: #998DD9;border-width: 1px;">

<div class="panelContent" style="background-color: #EAE6FF;">

**Licensing.** Fallow's static analysis (everything on this page) is **MIT and free**, usable in commercial/proprietary work. Only the optional **Fallow Runtime** layer (production runtime-coverage / hot-path features, activated via `fallow license activate`) is a separate paid product — we are not using it.

</div>

</div>

## 2. Prerequisites

Node **22.x** / npm **10.x** (already our standard — see each repo's `.nvmrc`). Fallow ships prebuilt binaries from the public npm registry, so our private `@epost` Artifact Registry token is not involved.

## 3. Install

Two options. Start with option A to try it; use option B once you want it pinned and available to the agent every time.

### Option A — Run ad hoc (zero install)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="bf5908d2-76f3-4f32-a5c1-52e746a87d8b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
cd {path-to-your-project}

EXAMPLE:

cd C:\work\repo\front-end\luz_next
npx fallow            # first run fetches the binary (a few seconds)
```

</div>

</div>

### Option B — Pin as a dev dependency (recommended for daily use)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="411a0ed5-853d-4935-ab71-34ec69ac683f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
npm install --save-dev fallow
```

</div>

</div>

This installs three binaries into `node_modules/.bin`: `fallow` (CLI), `fallow-lsp` (editor server), and `fallow-mcp` (the MCP server used by agents).

<div hasbody="true" macro-id="81bb6a07-7e6a-4634-8f9d-4f9de4be4f1a" macro-name="note">

<span class="aui-icon aui-icon-small aui-iconfont-warning confluence-information-macro-icon"> </span>

<div>

The only command that writes to your files is `fallow fix`, and it requires `--yes` to apply. Every other command is read-only, so exploring is safe.

</div>

</div>

## 4. How to use — common commands

All commands are read-only unless noted. Add `--format json --quiet` when you want machine-readable output.

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th><p>Goal</p></th>
<th><p>Command</p></th>
</tr>
&#10;<tr>
<td><p>Full analysis (cleanup + duplication + health)</p></td>
<td><p><code>npx fallow</code></p></td>
</tr>
<tr>
<td><p>Analysis on new code changed</p></td>
<td><p><code>npx fallow --changed --summary</code> OR</p>
<p><code>npx fallow --changed</code> OR</p>
<p><code>npx fallow --diff main</code></p></td>
</tr>
<tr>
<td><p>Health score (0–100 with grade)</p></td>
<td><p><code>npx fallow health --score</code></p></td>
</tr>
<tr>
<td><p>Find dead code</p></td>
<td><p><code>npx fallow dead-code</code></p></td>
</tr>
<tr>
<td><p>Find duplication</p></td>
<td><p><code>npx fallow dupes</code></p></td>
</tr>
<tr>
<td><p>Complexity hotspots + refactor targets</p></td>
<td><p><code>npx fallow health --hotspots --targets</code></p></td>
</tr>
<tr>
<td><p>PR risk gate (what your change introduced)</p></td>
<td><p><code>npx fallow audit --base master</code></p></td>
</tr>
<tr>
<td><p>Preview auto-fixes (no changes)</p></td>
<td><p><code>npx fallow fix --dry-run</code></p></td>
</tr>
<tr>
<td><p>Apply auto-fixes (writes files)</p></td>
<td><p><code>npx fallow fix --yes</code></p></td>
</tr>
<tr>
<td><p>Why is X flagged?</p></td>
<td><p><code>npx fallow dead-code --trace src/file.ts:symbolName</code></p></td>
</tr>
</tbody>
</table>

</div>

<div hasbody="true" macro-id="f04fefed-8319-42a5-a3a7-1a48c059357f" macro-name="note">

<span class="aui-icon aui-icon-small aui-iconfont-warning confluence-information-macro-icon"> </span>

<div>

**Our default branch is** `master`**, not** `main`**.** Always use `--base master` / `--changed-since origin/master`.

</div>

</div>

## 5. Using Fallow with an AI agent

With an AI coding agent (Claude or Cursor) you don't run fallow yourself — you ask in plain language, and the agent runs fallow, reads the results, and offers to fix them. You only need the `fallow` binary available (section 3). Installing the optional **agent skill** (step 2 below) just teaches the agent to use fallow correctly; without it, you can still tell the agent to run the commands from section 4.

### What it looks like

<div hasbody="true" macro-id="abba27d2-8a6a-464c-83c9-404c1794eade" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

**You:** "run fallow to audit changed code"

**Agent:** runs `fallow audit --changed-since origin/master --format json --quiet`, then replies — "Your change introduces 2 issues: an unused export `formatLabel` in `src/utils.ts:42`, and a duplicated block shared with `src/format.ts`. Want me to remove the unused export?"

**You:** "Yes — show me the change first."

**Agent:** runs `fallow fix --dry-run`, shows the diff; on your OK runs `fallow fix --yes`, re-runs the audit, and runs the tests to confirm nothing broke.

</div>

</div>

### How to do it

1.  **Check fallow runs in the repo:**`npx fallow --version` (or `npm i -D fallow`).

2.  **(Optional) Install the agent skill once** — see the next section.

3.  **Ask your agent in plain language.** Useful prompts:

    - "Run a fallow audit on my changes vs `master` and summarize the risks."

    - "Find unused exports and dependencies; remove the safe ones, but show me a dry run first."

    - "Why does fallow think `<symbol>` is unused?" — the agent traces it for you.

4.  **Review before applying.** The agent should always `fix --dry-run` first and run the tests after.

### Install the agent skill (optional, per developer — not committed to the repo)

One command. Run it in a terminal (PowerShell); it installs the skill into your user profile (not the repo) and asks which editor to set up:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="7be70a45-f23d-48f0-820a-5bb148122c2e" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
npx skills add fallow-rs/fallow-skills
```

</div>

</div>

Select the agent you want to install and take some more steps until you finish.


![[49471750190-Termius_plOXcneV9U-20260603-072749.png]]

![[49471750190-tQmvcigY00-20260603-072947.png]]

![[49471750190-Termius_JCUZyvul7J-20260603-073244.png]]



Restart your agent afterwards so it picks up the skill. Nothing is added to the repository, and each developer manages their own copy.

<div id="expander-2042709018" class="expand-container conf-macro output-block" hasbody="true" macro-id="cbab06dd-1045-4d78-a9a0-a04a8229865d" macro-name="expand">

<div id="expander-control-2042709018" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Optional: clean up an entire repo in one pass (paste this to your agent)</span>

</div>

<div id="expander-content-2042709018" class="expand-content expand-hidden">

<div hasbody="true" macro-id="2d2c27d4-44f6-4149-8522-a5a4075c35b0" macro-name="tip">

<span class="aui-icon aui-icon-small aui-iconfont-approve confluence-information-macro-icon"> </span>

<div>

Adopt fallow in this repo. Run `npx fallow`, then `fallow dead-code`, `dupes`, and `health` (use `--format json`). Fix high-confidence issues first (unresolved imports, unlisted/unused dependencies, unused files), then for each remaining finding either fix it in code, model it in `.fallowrc.json`, or add a narrow inline suppression with a reason. Re-run after each batch until clean. Run the test suite after any auto-fix. Report code changes, config changes, and exceptions added.

</div>

</div>

</div>

</div>

<div id="expander-961661569" class="expand-container conf-macro output-block" hasbody="true" macro-id="c42572c4-d69e-4637-863e-464dc57c75dc" macro-name="expand">

<div id="expander-control-961661569" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Optional: auto-block the agent's own commits when quality fails (Claude Code CLI)</span>

</div>

<div id="expander-content-961661569" class="expand-content expand-hidden">

Installs a hook so the agent's `git commit` / `git push` is blocked when `fallow audit` returns `fail`, handing the findings back so it fixes them before retrying:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="be3d3696-f511-4148-9897-cd7bb17ba84d" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
npx fallow hooks install --target agent
```

</div>

</div>

For Cursor/Copilot there's no hook — add this to your agent instructions: "Before any commit, run `fallow audit --changed-since origin/master --format json --quiet`; if the verdict is `fail`, fix the findings first."

</div>

</div>

## References

- Docs: <a href="https://docs.fallow.tools" class="external-link" rel="nofollow">https://docs.fallow.tools</a>

- Source (MIT): <a href="https://github.com/fallow-rs/fallow" class="external-link" rel="nofollow">github.com/fallow-rs/fallow</a>

- Agent skills (MIT): <a href="https://github.com/fallow-rs/fallow-skills" class="external-link" rel="nofollow">github.com/fallow-rs/fallow-skills</a>
