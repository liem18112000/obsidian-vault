---
title: "NamedTemporaryFile breaks Windows tests that reopen the path"
created: 2026-09-27
type: lesson
status: seedling
source: "leo-customer360 SSO session fix, 2026-09-27"
tags: [python, pytest, windows, tempfile, gotcha]
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
