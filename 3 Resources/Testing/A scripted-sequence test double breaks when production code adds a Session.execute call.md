---
ai_hash: 30b63f0396662fd0
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-20
entities: []
source: session 2026-08-20, leo-customer360 commit 4da1868
status: seedling
tags:
- testing
- test-doubles
- sqlalchemy
- mocking
- gotcha
title: A scripted-sequence test double breaks when production code adds a Session.execute
  call
type: lesson
---

# A scripted-sequence test double breaks when production code adds a Session.execute call

A test double that scripts a **fixed sequence of return values** for `Session.execute(...)` (one canned result per call, in order) is silently coupled to the exact number and order of `execute` calls the production code makes. Adding **any** new `Session.execute` statement to that code path — even an unrelated side-effect like setting a session variable — consumes one scripted result out of position and shifts every subsequent assertion.

**Symptom cluster** (all from one added statement): off-by-one wrong results, count mismatches (`assert 4 == 3`), and `KeyError` on parameter dicts that are now read against the wrong call. In `customer360-api` this broke 11 tests at once when a leading `set_config` was added to login provisioning.

**Two takeaways:**
1. Keep genuinely out-of-band SQL off the ORM session so it never enters the scripted sequence — e.g. run it on the raw connection (see [[Set Postgres RLS session GUC via the raw DBAPI connection, not Session.execute]]).
2. Prefer test doubles that match on the SQL/statement rather than blindly popping the next scripted result — positional scripts are brittle to any new statement.

Root cause was found via `git log`/blame on the SQL sequence: the assertions had been stable across two prior refactors and only the commit that inserted the extra `Session.execute` broke them — evidence the code regressed, not the tests.

## Related

- [[Set Postgres RLS session GUC via the raw DBAPI connection, not Session.execute]]

%% ai-graph-start %%

**Related notes:**
- [[Set Postgres RLS session GUC via the raw DBAPI connection, not Session.execute]]
- [[Measure non-idempotent integration tests on clean state - 409 on re-run is an isolation defect]]
- [[test-agent-v2 executor tests share memory-bank state and fail by test order]]
- [[Never pipe a dbmate migration file through a raw psql replay — its down section is destructive]]
- [[Shape-keyed test mocks break when production query shapes change]]

%% ai-graph-end %%