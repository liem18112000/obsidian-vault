---
title: "Safely strip Python comments + collapse docstrings with tokenize + ast"
created: 2026-09-08
type: howto
status: seedling
source: "session 2026-09-08 test-agent-v2 cleanup"
tags: [python, tokenize, ast, refactoring, codemod]
---

# Safely strip Python comments + collapse docstrings with tokenize + ast

To bulk-strip `#` comments and collapse docstrings across a Python codebase WITHOUT corrupting code, use two stdlib tools — not regex:

- **tokenize** to remove comments. `tokenize.generate_tokens` yields a `COMMENT` token type that is distinct from `STRING`, so a `#` inside a string literal or an f-string is never mistaken for a comment. Skip COMMENT tokens whose text lacks `noqa` (those are functional ruff/flake8 directives — keep them). Rebuild line-by-line: record `{row: start_col}` for comments to drop, then for each physical line truncate at that col + rstrip; if the remainder is empty (a full-line comment) drop the line, else keep the code (inline comment).
- **ast** to collapse docstrings. Walk the tree; a docstring is `node.body[0]` being an `ast.Expr` whose `.value` is an `ast.Constant` of type `str`, for `Module`/`ClassDef`/`FunctionDef`/`AsyncFunctionDef`. Its `lineno..end_lineno` gives the exact span and `col_offset` the indent. Replace those lines with one line: `<indent>"""<first non-empty content line>"""`. Edit spans **bottom-up** (sort by start line descending) so earlier line numbers stay valid.

Guardrails: run `ast.parse(result)` per file before writing (never persist a file that stopped parsing); do a comment-strip pass and a docstring pass separately, re-reading between them (line numbers shift); squeeze 3+ blank lines; then `ruff check --fix` for import-order/blank-line normalization; then run the test suite (the transform is comment/docstring-only so behaviour must be identical). Everything committed first = git revert is the safety net.

Gotcha: `@mcp.tool()` / some frameworks use a functions docstring as the tool DESCRIPTION — collapsing shortens those descriptions (cosmetic, not breaking). Agent `description=`/`instructions=` passed as string ARGS are not docstrings, so they are untouched. Verified on test-agent-v2 (254 files, zero logic change).
