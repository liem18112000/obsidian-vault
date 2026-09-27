---
title: "create_note.py --link comma-splits titles and silently breaks wikilinks"
created: 2026-09-27
type: gotcha
status: seedling
source: "session 2026-09-27 Confluence distillation"
tags: [obsidian, wikilinks, zettelkasten, tooling, gotcha]
---

# create_note.py --link comma-splits titles and silently breaks wikilinks

`create_note.py --link "<title>"` splits the value on commas. Any note title containing a comma is therefore shredded into several bogus `## Related` entries, each pointing at nothing:

```bash
--link "Connection count, not tenant count, sizes a multi-tenant Postgres cluster"
```

produces

```markdown
## Related

- [[Connection count]]
- [[not tenant count]]
- [[sizes a multi-tenant Postgres cluster]]
```

instead of one link to the real note. Obsidian renders unresolved wikilinks in a muted colour rather than erroring, so the note *looks* linked and the connection silently does not exist — which defeats the point, since connectivity is most of a Zettelkasten's value.

**Workarounds:**

- **Avoid commas in note titles** you intend to link to. This is the cheap fix and titles are usually improvable anyway — `Connection count not tenant count sizes a multi-tenant Postgres cluster` reads badly, so prefer a rephrasing that needs no comma.
- **Pass `--link` once per link** rather than relying on a delimiter, and keep each value comma-free.
- **Write the `## Related` section by hand** in the piped body when a title genuinely needs a comma.

> [!tip] Verify links after any bulk note creation
> Index every `.md` basename in the vault, then walk each note's `[[targets]]` and assert each one exists:
> ```js
> for (const m of text.matchAll(/(?<!!)\[\[([^\]|#]+)/g))
>   if (!titles.has(m[1].trim())) broken.push(m[1].trim());
> ```
> Note the `(?<!!)` — it excludes `![[embeds]]`, which point at attachments rather than notes and need checking against a different set.

**A second, related breakage from the same session:** links written by hand from memory of a *source* document's filename drifted from what the importer actually wrote to disk. The export folder produced `Proposal- eArchived architecture direction…` while the vault importer's sanitiser produced `Proposal eArchived architecture direction…`. Two different sanitisers, two different filenames, silently unresolved links.

**The fix that handles both:** repair by **normalised fuzzy match** — lowercase, collapse every non-alphanumeric run to a single space, and look the target up in an index of real titles. That matches across punctuation differences without needing to know which sanitiser ran. For the comma-split case, try rejoining consecutive `## Related` entries with `", "` and check whether the joined form matches a real title.
