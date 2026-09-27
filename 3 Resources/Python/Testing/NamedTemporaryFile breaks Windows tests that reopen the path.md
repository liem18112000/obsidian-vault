---
ai_hash: da671e8ca41a54f7
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: leo-customer360 SSO session fix, 2026-09-27
status: seedling
tags:
- python
- pytest
- windows
- tempfile
- gotcha
title: NamedTemporaryFile breaks Windows tests that reopen the path
type: lesson
---

# NamedTemporaryFile breaks Windows tests that reopen the path

`tempfile.NamedTemporaryFile()` keeps its file handle open for the life of the context manager. On Windows that handle is exclusive, so any code under test that *reopens the path* — the common "write my output to the configured file" pattern — fails with `PermissionError: [Errno 13] Permission denied`.

On Linux the same test passes, because POSIX permits a second open on an already-open file. So the bug is invisible in CI and only bites developers on Windows, which makes it easy to misattribute to whatever change surfaced it.

```python
# Fails on Windows when main() reopens ENV_FILE to write into it
with tempfile.NamedTemporaryFile() as env_file:
    os.environ["ENV_FILE"] = env_file.name

# Works on both: a directory, and a path inside it that nothing holds open
with tempfile.TemporaryDirectory() as tmp_dir:
    os.environ["ENV_FILE"] = os.path.join(tmp_dir, ".env")
```

Rule of thumb: use `NamedTemporaryFile` only when *you* do all the I/O through the returned handle. The moment a path is handed to other code, use `TemporaryDirectory` and build a path inside it.

%% ai-graph-start %%

**Related notes:**
- [[Windows Python resolves a leading-slash path to C-colon-tmp, not Git Bash tmp]]
- [[Git Bash mktemp paths are unreadable by Windows python; pipe via stdin instead of a temp-file path]]
- [[Python venv layout Scripts on Windows vs bin on POSIX breaks Linux-authored test runners]]

%% ai-graph-end %%