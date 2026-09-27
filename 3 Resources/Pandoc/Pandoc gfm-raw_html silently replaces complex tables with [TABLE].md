---
ai_hash: 510cd2b4b75181da
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: session 2026-09-27 Confluence export
status: seedling
tags:
- pandoc
- markdown
- html
- conversion
- gotcha
- data-loss
title: Pandoc gfm-raw_html silently replaces complex tables with [TABLE]
type: gotcha
---

# Pandoc gfm-raw_html silently replaces complex tables with [TABLE]

Disabling pandoc's `raw_html` extension on a Markdown writer (`-t gfm-raw_html`) causes **silent content loss** for any table GFM cannot express. Pandoc emits the literal placeholder `[TABLE]` and the rows are simply gone — no warning, no non-zero exit.

GFM pipe tables only support a flat grid of inline content. Anything richer trips the fallback:

- merged cells (`colspan` / `rowspan`)
- block content inside a cell (lists, nested tables, multiple paragraphs)
- an empty or missing header row

Normally pandoc handles these by falling back to **raw HTML** — it passes the `<table>` through untouched. Subtracting `raw_html` removes that escape hatch, so pandoc has nothing left to emit but the placeholder.

```bash
pandoc -f html -t gfm-raw_html page.html   # complex table -> "[TABLE]"
pandoc -f html -t gfm          page.html   # complex table -> <table>...</table>
```

I hit this exporting 167 Confluence pages: 41 of them lost 128 tables, and the failure was invisible because every page still produced plausible-looking markdown. The tell was pages whose body collapsed to ~59 characters.

**Detection is a one-liner** — worth running after any bulk HTML-to-markdown conversion:

```bash
grep -l "^\[TABLE\]$" */index.md | wc -l
```

The lesson generalises past tables: choosing a *lossy subtractive* format string trades fidelity for tidiness, and pandoc expresses that loss as a placeholder rather than an error. If you want tidy output, keep `raw_html` on and strip the unwanted markup yourself with a post-pass over the HTML — that way anything you did not explicitly decide to drop survives.

## Related

- [[Export Confluence to markdown via body.view HTML]]
- [[not body.storage]]

%% ai-graph-start %%

**Related notes:**
- [[Export Confluence to markdown via body.view HTML, not body.storage]]
- [[Pandoc Markdown to DOCX full-width tables, justified body, per-section page breaks]]

%% ai-graph-end %%