---
ai_hash: 56426b0840fec343
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-07
entities: []
source: luz_docs_import review diagram 2026-08-07
status: seedling
tags:
- mermaid
- markdown
- diagrams
- gotcha
title: Mermaid node labels with parentheses or special chars must be wrapped in double
  quotes
type: gotcha
---

# Mermaid node labels with parentheses or special chars must be wrapped in double quotes

In Mermaid flowchart node labels, characters that the parser also uses as shape/syntax tokens must be escaped by wrapping the whole label in double quotes: `D["set createJob (status UPLOADED)"]`. An unquoted `(` inside `[...]` makes Mermaid think a new shape is starting and throws e.g. `Parse error ... got 'PS'` (PS = paren-start; likewise 'SQS' for '['). 

Trigger chars include parentheses `()`, square/curly brackets, and often `/` combos. Safe fix: quote any label containing punctuation. `<br/>` line breaks and plain `/` are usually tolerated unquoted, but quoting is the reliable default.

Gotcha within the gotcha: the parser stops at the FIRST offending node, so after quoting one you may hit the next. Scan ALL nodes for unescaped special chars in one pass rather than fixing one-at-a-time. Applies to Mermaid rendered in Claude artifacts and in Obsidian/GitHub Markdown alike.

Related: [[luz_docs_import]].

%% ai-graph-start %%

**Related notes:**
- [[LLM-generated mermaid sequenceDiagrams die on semicolons and reserved-word aliases]]
- [[Mermaid Flowchart - Multi-word Labels and Decision Branches]]
- [[Artifacts render mermaid natively — never add a mermaid CDN script (CSP blocks it)]]
- [[Validate mermaid diagrams headlessly with mermaid.parse under jsdom]]
- [[Unquoted YAML frontmatter description breaks on colon-space in SKILL.md]]

%% ai-graph-end %%