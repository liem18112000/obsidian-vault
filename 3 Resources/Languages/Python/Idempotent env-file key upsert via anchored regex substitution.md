---
ai_hash: 4da2b6d3c84f02e8
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-07-11
entities: []
source: virtual-avatar session 2026-07-11, scripts/set_dev_session_token.py
status: seedling
tags:
- python
- regex
- dotenv
- env-file
- idempotent
title: Idempotent env-file key upsert via anchored regex substitution
type: technique
---

# Idempotent env-file key upsert via anchored regex substitution

When a script needs to set/update a single `KEY=value` line in a `.env`-style file without disturbing the other lines (comments, unrelated vars), the simplest idempotent approach is an anchored regex substitution with a fallback append:

```python
import re
text = path.read_text()
new_text, count = re.subn(r"(?m)^KEY=.*$", "KEY=newvalue", text)
if count == 0:
    new_text = text.rstrip("\n") + "\nKEY=newvalue\n"
path.write_text(new_text)
```

The `(?m)^KEY=.*$` pattern anchors to line start/end (multiline mode), so it only replaces an existing `KEY=...` line in place — preserving line order, comments, and every other variable. If the key is not present yet, `re.subn` returns a count of 0, so you fall back to appending a fresh line.

This is safer than blindly overwriting the whole file (which would wipe other required variables) and safer than naive string `.replace()` (which could accidentally match a value substring elsewhere in the file). Useful for any small dev-utility script that needs to inject/update one `.env` entry — e.g. seeding a placeholder `SESSION_TOKEN=dev-token` for local runs without touching `GCP_PROJECT_ID` etc.

## Related

- [[Dataclass field defaults reading env vars are evaluated at import time, not instantiation]]

%% ai-graph-start %%

**Related notes:**
- [[Grep-audit env vars against code before pruning .env files]]
- [[Dataclass field defaults reading env vars are evaluated at import time, not instantiation]]

%% ai-graph-end %%