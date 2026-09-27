---
ai_hash: 4f4368cc063e43aa
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-27
entities:
- Bitbucket Cloud API
- Jira
- Confluence
- Host
- Auth
- Raw file content
- https://api.bitbucket.org/2.0
- '*.atlassian.net'
- Basic auth
- username
- app password
- repo/workspace access token
- Bearer
- Jira email
- API-token pair
- GET /2.0/repositories/{ws}/{repo}/src/{ref}/{path}
- resp.text
- 'Accept: application/json'
- Bitbucket
- Atlassian
- Design pattern
- multi-service read-only client
- config
- retry core
- _request(url, auth)
- _get(path)
- BaseClient
- JiraMixin
- ConfluenceMixin
- BitbucketMixin
- AtlassianClient
- atlassian/ (package)
- kga
- kga.atlassian.asyncio.sleep
- kga.atlassian.base.asyncio.sleep
- asyncio
- base.py
- LUZ-159671
- test-agent
- Extracting every link from Jira ADF and Confluence storage
source: session 2026-08-27 — kga atlassian package
status: seedling
tags:
- bitbucket
- atlassian
- api
- mixin
- python
title: 'Bitbucket Cloud API differs from Jira/Confluence: host, auth, raw src'
type: lesson
---

# Bitbucket Cloud API differs from Jira/Confluence: host, auth, raw src

Bitbucket is Atlassian but its Cloud REST API is a **different realm** from Jira/Confluence — three gotchas:
- **Host:** `https://api.bitbucket.org/2.0` (not the `*.atlassian.net` site base).
- **Auth:** Basic auth with **username + app password** (or a repo/workspace access token as Bearer) — the Jira email + API-token pair does NOT authenticate Bitbucket.
- **Raw file content:** `GET /2.0/repositories/{ws}/{repo}/src/{ref}/{path}` returns the **raw file text**, not JSON — read `resp.text`, and do not force `Accept: application/json`.

**Design pattern for a multi-service read-only client:** put the config + retry core (`_request(url, auth)`, `_get(path)`) on one `BaseClient`, and add each service as a thin **mixin** (`JiraMixin`, `ConfluenceMixin`, `BitbucketMixin`) whose methods call `self._request/_get`. Compose `class AtlassianClient(JiraMixin, ConfluenceMixin, BitbucketMixin, BaseClient)`. One retry core, no duplication, and the public API stays a single client. Package it as `atlassian/` (base + one module per service) so each service is isolated. Gotcha when splitting: a test that `monkeypatch.setattr("kga.atlassian.asyncio.sleep", ...)` must move to `kga.atlassian.base.asyncio.sleep` after `asyncio` relocates into `base.py`.

Context: kga `atlassian/` package (LUZ-159671 test-agent).

## Related

- [[Extracting every link from Jira ADF and Confluence storage]]

%% ai-graph-start %%

**Related notes:**
- [[KGA crawler fetches repo source files via client mixin plus NodeFetcher registered by kind]]
- [[Extracting every link from Jira ADF and Confluence storage]]
- [[Atlassian Cloud OAuth 3LO specifics JSON token body, rotating refresh, cloudId via accessible-resources]]
- [[Bitbucket app-password discovery endpoints deprecated (CHANGE-2770)]]
- [[GitHub fetch credentials PAT Bearer anonymous public Enterprise api-v3 raw Contents API]]

**Relations:**
- Bitbucket Cloud API — *differs from* — Jira
- Bitbucket Cloud API — *differs from* — Confluence
- Bitbucket Cloud API — *differs in aspect* — Host
- Bitbucket Cloud API — *differs in aspect* — Auth
- Bitbucket Cloud API — *differs in aspect* — Raw file content
- Bitbucket Cloud API — *uses Host* — https://api.bitbucket.org/2.0
- Jira — *uses Host* — *.atlassian.net
- Confluence — *uses Host* — *.atlassian.net
- Bitbucket Cloud API — *uses Auth method* — Basic auth
- Basic auth — *requires* — username
- Basic auth — *requires* — app password
- Bitbucket Cloud API — *uses Auth method* — repo/workspace access token
- repo/workspace access token — *is a type of* — Bearer
- Jira email — *does NOT authenticate* — Bitbucket Cloud API
- API-token pair — *does NOT authenticate* — Bitbucket Cloud API
- GET /2.0/repositories/{ws}/{repo}/src/{ref}/{path} — *returns* — Raw file content
- Raw file content — *is read using* — resp.text
- Accept: application/json — *should not be forced for* — Raw file content
- Bitbucket — *is part of* — Atlassian
- Design pattern — *is for* — multi-service read-only client
- multi-service read-only client — *includes* — config
- multi-service read-only client — *includes* — retry core
- config — *is on* — BaseClient
- retry core — *is on* — BaseClient
- multi-service read-only client — *adds service via* — JiraMixin
- multi-service read-only client — *adds service via* — ConfluenceMixin
- multi-service read-only client — *adds service via* — BitbucketMixin
- JiraMixin — *calls* — _request(url, auth)
- JiraMixin — *calls* — _get(path)
- ConfluenceMixin — *calls* — _request(url, auth)
- ConfluenceMixin — *calls* — _get(path)
- BitbucketMixin — *calls* — _request(url, auth)
- BitbucketMixin — *calls* — _get(path)
- AtlassianClient — *composes* — JiraMixin
- AtlassianClient — *composes* — ConfluenceMixin
- AtlassianClient — *composes* — BitbucketMixin
- AtlassianClient — *composes* — BaseClient
- atlassian/ (package) — *structure includes* — base.py
- atlassian/ (package) — *structure includes* — one module per service
- kga — *is context for* — atlassian/ (package)
- kga.atlassian.asyncio.sleep — *moves to* — kga.atlassian.base.asyncio.sleep
- asyncio — *relocates into* — base.py
- atlassian/ (package) — *is related to* — LUZ-159671
- LUZ-159671 — *is related to* — test-agent
- Bitbucket Cloud API differs from Jira/Confluence: host, auth, raw src — *is related to* — Extracting every link from Jira ADF and Confluence storage

%% ai-graph-end %%