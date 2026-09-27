---
title: "GCS object names cap at 1024 chars — hash URL-derived keys"
created: 2026-09-13
type: lesson
status: seedling
source: "session 2026-09-13 LUZ-156281 demo"
tags: [gcs, gcp, object-storage, gotcha, test-agent-v2]
---

# GCS object names cap at 1024 chars — hash URL-derived keys

Google Cloud Storage limits an object *name* to 1024 characters (UTF-8 bytes). Any code that derives a GCS object key from untrusted/variable-length input — a URL, a user string, an external-web page address — must length-bound the key, or an over-long input yields a 400 `invalid` upload error at write time.

**Failure seen:** test-agent-v2 KGA `gather_knowledge` crawled an external-web lead (`https://sequencediagram.org/index.html?initialData=C4...`) and built the note key `memory/notes/external-web/<sanitized-url>.json`. The sanitizer only replaced non-alphanumerics with `_` (no cap), so the key hit 1122 chars → GCS 400 → the *entire gather aborted* (one bad lead kills the whole crawl).

**Fix (root cause, one shared helper):** cap the slug and append a short hash of the full input so distinct long inputs stay distinct and the slug is deterministic (so the `.json`/`.md` pair and later reads map to the same key):

```python
_SLUG_MAX = 200
def _slug(s):
    out = re.sub(r"[^A-Za-z0-9._-]+", "_", s).strip("_")
    if len(out) <= _SLUG_MAX:
        return out
    return f"{out[:_SLUG_MAX-13]}-{hashlib.sha1(s.encode()).hexdigest()[:12]}"
```

**General lesson:** never derive a storage key straight from a URL/user string without bounding its length; truncate-plus-hash keeps it readable, unique, and deterministic. Determinism matters when the same id must round-trip (write then read) or map to a sibling file (`.json` + `.md`).

## Related

- [[test-agent-v2 image built only from pyproject + src + main.py]]
