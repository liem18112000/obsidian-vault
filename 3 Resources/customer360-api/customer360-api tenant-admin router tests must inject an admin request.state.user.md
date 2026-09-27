---
title: "customer360-api tenant-admin router tests must inject an admin request.state.user"
created: 2026-09-14
type: lesson
status: seedling
source: "leo-customer360 test_campaign_draft_api.py, PR #69, session 2026-09-14"
tags: [customer360-api, pytest, auth, sso, ci, gotcha]
---

# customer360-api tenant-admin router tests must inject an admin request.state.user

In customer360-api, endpoints guarded by \`require_tenant_admin\` (campaign approve/reject, segment->CRM sync, campaign activation, email config) return **401** in CI when the test harness does not populate \`request.state.user\`. Root cause: \`run_unit_tests.sh\` forces \`export SSO_LOGIN=true\`, and \`require_tenant_admin\` (unlike the older \`require_admin\`) has **no test-harness escape hatch** — with SSO on and \`request.state.user\` not a dict it raises 401 before any role check.

## The convention (do this in the test app middleware)
Every sibling router test injects a real admin caller unconditionally:
\`\`\`python
request.state.user = {"roles": ["tenant_admin"]}   # test_crm_sync_router.py:57, test_campaign_activation_router.py:35
\`\`\`
A middleware that only sets \`state.user\` when an \`X-Roles\` header is present (and happy-path tests that send none) is the bug — it produces "green locally (SSO off in .env), red in CI (SSO forced on)".

## Fix pattern
Default the injected role to admin, let enforcement tests override:
\`\`\`python
roles = request.headers.get("X-Roles", "tenant_admin")
request.state.user = {"roles": [r.strip() for r in roles.split(",") if r.strip()]}
\`\`\`
Happy-path approve/reject/isolation tests then pass the gate; the dedicated \`*_requires_tenant_admin_role_in_sso_mode\` tests still send \`X-Roles: analyst\` (+ \`patch("core.auth.SSO_LOGIN", True)\`) to assert the 403 path.

## Why fix the test, not the gate
The production gate is correct and consistent across all sibling routers; only the new \`test_campaign_draft_api.py\` deviated from the inject-admin-user convention. Fix once in the shared \`_build_test_app\` middleware -> all 6 failing tests pass.

TENANT_ADMIN_ROLES = {platform_admin, super_admin, system_admin, tenant_admin, admin}. Real case: PR #69 (commit a398d84 "Enforce tenant-admin review actions"), fixed 2026-09-14.

## Related

- [[Fork PRs get no secrets in GitHub Actions]]
