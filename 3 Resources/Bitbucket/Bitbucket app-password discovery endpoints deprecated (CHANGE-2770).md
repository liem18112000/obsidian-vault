---
title: "Bitbucket app-password discovery endpoints deprecated (CHANGE-2770)"
created: 2026-09-13
type: lesson
status: seedling
source: "session 2026-09-13 LUZ-156281 demo"
tags: [bitbucket, atlassian, api, auth, deprecation, gotcha]
---

# Bitbucket app-password discovery endpoints deprecated (CHANGE-2770)

Atlassian is sunsetting Bitbucket Cloud **app passwords** and several old REST discovery endpoints (see changelog CHANGE-2770). Observed 2026-09 with an app-password credential (username = Atlassian *email*):

- `GET /2.0/user` → **403**
- `GET /2.0/workspaces` → **404** ("no API hosted at this URL")
- `GET /2.0/repositories?role=member&q=name~"..."` (list/search) → **410 Gone** — "Functionality has been deprecated"

BUT the **direct, fully-qualified** endpoints still work with the same credential:
- `GET /2.0/repositories/<ws>/<repo>` → **200**
- `GET https://bitbucket.org/<ws>/<repo>/get/<ref>.tar.gz` → **200**

**Lesson:** to verify a Bitbucket repo/slug, hit the direct `repositories/<ws>/<repo>` endpoint, not the deprecated search — the search 410s and looks like an auth failure. The 403 on `/user` is misleading: auth is fine for direct resource access. (App passwords authenticate with the Bitbucket account username; the deprecated endpoints reject the email-as-username combo, the direct ones tolerate it.) Migration path: move to Atlassian API tokens / scoped tokens before app passwords are fully removed.

## Related

- [[gather_codebase needs axonivy-prod/<repo> workspace slug]]
