---
ai_hash: f52733b6c4f146b4
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-29
entities: []
source: session 2026-08-29 task-store migration
status: seedling
tags:
- sqlalchemy
- url-encoding
- gotcha
- python
title: SQLAlchemy URL.create encodes passwords correctly; hand-encoding with quote_plus
  can corrupt them
type: gotcha
---

# SQLAlchemy URL.create encodes passwords correctly; hand-encoding with quote_plus can corrupt them

When building a SQLAlchemy database URL, prefer `sqlalchemy.URL.create(...)` with raw `username`/`password`/`query` values over hand-assembling the string — it percent-encodes each part with the *correct* codec for its position.

The gotcha with hand-encoding: SQLAlchemy decodes the **netloc password** with `urllib.parse.unquote` (percent-only), **not** `unquote_plus`. So if you encode the password with `quote_plus`, a literal space becomes `+`, which `unquote` leaves as a literal `+` — silently corrupting the password (a `+` in the input is fine because `quote_plus` maps it to `%2B`; only spaces break). Query values have their own decoding rules too. Delegating to `URL.create` sidesteps all of this.

```python
# fragile:
f"postgresql+asyncpg://{quote_plus(user)}:{quote_plus(pw)}@/{db}?host={quote_plus(sock)}"
# robust:
URL.create("postgresql+asyncpg", username=user, password=pw, database=db, query={"host": sock})
```

Verify a round-trip with `engine.url.password == pw` and log safely with `engine.url.render_as_string(hide_password=True)`.

Related: [[Cloud Run to Cloud SQL via Auth-proxy unix socket with asyncpg]].

## Related

- [[Cloud Run to Cloud SQL via Auth-proxy unix socket with asyncpg]]

%% ai-graph-start %%

**Related notes:**
- [[Cloud Run to Cloud SQL via Auth-proxy unix socket with asyncpg]]
- [[node-postgres percent-decodes the connection-string password]]
- [[Cloud SQL Python Connector async use create_async_connector inside the loop]]
- [[Local Cloud SQL admin without psql use the Python connector + asyncpg]]
- [[Cloud Run mounts the Cloud SQL cloudsql socket only into the ingress container, not sidecars]]

%% ai-graph-end %%