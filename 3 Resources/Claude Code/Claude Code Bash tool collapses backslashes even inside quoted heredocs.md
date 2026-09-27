---
ai_hash: f16adecfead0243a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: session 2026-09-27 Confluence export
status: seedling
tags:
- claude-code
- bash
- shell
- escaping
- gotcha
- tooling
title: Claude Code Bash tool collapses backslashes even inside quoted heredocs
type: gotcha
---

# Claude Code Bash tool collapses backslashes even inside quoted heredocs

Backslashes in code passed to the Bash tool get collapsed one level before the interpreter sees them — and this happens **even inside a single-quoted heredoc**, which normally guarantees literal passthrough. The failure is silent: the code usually still parses, it just behaves wrong.

Two ways it bit me in one session:

**1. Silent no-op.** A JS escape helper written inline:

```bash
node -e 'const esc = s => s.replace(/([\[\]|])/g, "\\$1"); ...'
```

Node received `"\$1"`, not `"\\$1"`. JS treats `\$` as an unknown escape and drops the backslash, so the replacement became just `$1` — the function returned its input unchanged. No error, no warning; 167 rows of output were quietly unescaped.

**2. Syntax error.** The same source moved into a quoted heredoc, which should be literal:

```bash
cat > script.js <<'JS'
const esc = s => s.replace(/[\[\]|]/g, c => '\\' + c);
JS
```

The file on disk contained `'\' + c` — an unterminated string. Node threw `SyntaxError: Invalid or unexpected token`.

**The rule:** do not author code containing backslashes through the Bash tool. This covers regex escapes (`\s`, `\b`, `\[`), escaped quotes, and string literals for path separators or newlines.

**What to do instead:**

- Use the **Write tool** for any file whose content contains backslashes. This is the documented fallback for "Bash genuinely cannot do the job," and this is exactly that case.
- If you must stay inline, write backslash-free code. Character classes avoid escapes (`/[|]/` not `/\|/`), `.split(x).join(y)` avoids regex entirely, and `String.fromCharCode(92)` produces a literal backslash with no escaping at all.
- Python heredocs need `r"""..."""` raw strings for the same reason — a non-raw string additionally emits `SyntaxWarning: invalid escape sequence` before misbehaving.

**Detection:** after generating a file through Bash, grep for the backslashes you expect (`grep -c` on the literal). Zero hits where you expected many means the collapse happened. Type-2 failures announce themselves; type-1 failures do not, so verify output rather than trusting exit code 0.

Both this and the pandoc `[TABLE]` placeholder bug are silent-corruption failures that sail past a naive "did it exit 0" check — see the related note below.

## Related

- [[Pandoc gfm-raw_html silently replaces complex tables with [TABLE]|Pandoc gfm-raw_html silently replaces complex tables with [TABLE]]]

%% ai-graph-start %%

**Related notes:**
- [[Apostrophe inside bash ${varmessage} breaks the parser]]
- [[Bash collapses backslashes before PowerShell stdin, breaking Windows-path JSON]]
- [[ssh 'bash -s' flattens args, so empty middle args shift positionals]]
- [[PowerShell here-string @'...'@ silently corrupts git commit messages in the Bash tool]]
- [[Unquoted YAML frontmatter description breaks on colon-space in SKILL.md]]

%% ai-graph-end %%