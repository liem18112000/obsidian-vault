---
ai_hash: 8a1f36a287e804dc
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-08
entities: []
source: test-agent-v2 eval suite hang, session 2026-09-08
status: seedling
tags:
- pytest
- dotenv
- fixtures
- offline-tests
- gotcha
- faulthandler
title: load_dotenv() in package __init__ leaks .env into pytest, breaking offline
  tests
type: lesson
---

# load_dotenv() in package __init__ leaks .env into pytest, breaking offline tests

A package that calls `load_dotenv()` at import time (e.g. in `__init__.py`, to bootstrap a deployed service) will **also** load a local `.env` into `os.environ` during a pytest run — the vars land at *collection* time, when pytest imports the package. Any "offline" test that gates behaviour on an env var (here `vertex_config()` reading `VERTEX_PROJECT/LOCATION/MODEL`) then silently takes the *configured* branch and makes a **real network call** — which manifests as a mysterious hang (an SSL `recv`/`do_handshake` that never returns), not a clean failure.

**Tell-tale:** tests pass in CI / a clean shell (no `.env`) but hang locally (where `.env` exists), or hang only for some cases. `faulthandler.dump_traceback_later(N, exit=True)` before the call dumps the stuck stack and reveals the exact network call + the app frames above it — the fastest way to pin a hang.

**Fix:** a **session-scoped, autouse** fixture in `conftest.py` that pops the offending env vars (restoring them after). Session scope is REQUIRED when the thing that triggers the network call is set up in a **module- or session-scoped fixture** — a function-scoped clear (or `monkeypatch.delenv`, which is function-only) runs *after* the higher-scoped fixture and is too late. Tests that genuinely want the configured path set the vars via their own function-scoped `monkeypatch.setenv`, which applies within (and reverts after) their test.

Rule of thumb: `load_dotenv()` belongs behind a runtime entrypoint, not at import; if it must be at import, neutralise it in tests. And a hanging "offline" test almost always means an un-mocked real client reached the network.

## Related

- [[Dead-code refcount scans flag intentional seams as unused; vet before deleting]]

%% ai-graph-start %%

**Related notes:**
- [[Module-level load_dotenv lets unit tests hit real cloud credentials]]
- [[Dataclass field defaults reading env vars are evaluated at import time, not instantiation]]
- [[Path(__file__).parent breaks when a module is moved to a deeper directory]]
- [[pytest imports all test modules before applying -m deselection]]
- [[Test module-load env decisions with vi.resetModules plus dynamic import per case]]

%% ai-graph-end %%