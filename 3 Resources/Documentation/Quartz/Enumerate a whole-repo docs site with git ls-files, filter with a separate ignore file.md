---
title: "Enumerate a whole-repo docs site with git ls-files, filter with a separate ignore file"
created: 2026-09-05
type: lesson
status: seedling
source: "session 2026-09-05 (leo-customer360 whole-repo Quartz plan)"
tags: [quartz, docs-as-code, github-actions, gitignore, static-site, gotcha]
---

# Enumerate a whole-repo docs site with git ls-files, filter with a separate ignore file

When building a static docs site (Quartz etc.) from **all** markdown in a repo rather than one `docs/` folder, enumerate with `git ls-files "*.md"` instead of a filesystem walk.

## Why
`git ls-files` returns only tracked files and **already honors `.gitignore`**, so the scan never descends into `node_modules/`, `dist/`, `.venv/`, etc. A raw fs-walk would have to re-list every heavy tree in the exclusion file.

## Two-layer exclusion
- `.gitignore` (implicit, via git ls-files) removes vendor/build/tooling trees.
- A separate **`.documentignore`** (gitignore syntax, parsed with the `ignore` npm package) carries only the *docs-specific* opinions: hide agent files (`CLAUDE.md`), boilerplate (`CHANGELOG.md`, `LICENSE.md`), per-package scaffolding READMEs, or the plans themselves. Keeps that file short and meaningful.
- Per-file opt-out is better done with `draft: true` frontmatter (Quartz `RemoveDrafts()`), not a new ignore line.

## Collector pattern
A ~40-line Node script: enumerate → filter through `ignore()` → **mirror survivors into `content/<same relative path>`** (structure preserved, so URLs match repo paths and folder-listing pages generate) → inject frontmatter (title from first H1/filename, `source`, tags) where missing → auto-generate a root `index.md`. Never mutate source files; write only into the throwaway `content/`.

## Gotcha — wikilink collisions
Across a whole repo, dozens of files are named `README.md`, so `[[README]]` cannot resolve uniquely. Set `CrawlLinks({ markdownLinkResolution: "absolute" })` and prefer path-style links (`[[customer360-api/README]]`).

Related: [[claude-api]]
