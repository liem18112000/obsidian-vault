---
ai_hash: 87fc3422bd4fae03
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-19
entities: []
source: leo-customer360 deployments/cache session 2026-08-19
status: seedling
tags:
- ssh
- bash
- gotcha
- heredoc
- terraform
title: ssh host 'bash -s' flattens args into a remote shell string
type: lesson
---

# ssh host 'bash -s' flattens args into a remote shell string

When you run `ssh host 'bash -s' arg1 arg2 <<'REMOTE'`, ssh does NOT pass arg1/arg2 as clean argv to the remote process. It concatenates everything after the host into ONE string and hands it to the remote login shell, which re-parses it. So `bash -s "$IMG" "$PORT" "$PW_B64"` becomes the literal remote command text `bash -s <img> <port> <pw>` and is word-split + interpreted by the remote shell before bash -s ever assigns $1/$2/$3.

Consequence: any argument containing shell metacharacters breaks it. A real case: a value read from a tfvars line by a naive `grep|sed` helper returned the WHOLE line `redis_port  = 6580  # redis.conf port` (because the helper only extracted quoted values). Passed as an arg, the remote shell treated `#` as a comment and silently dropped every following argument (the base64 password), so the remote `base64 -d` got empty/garbage input and failed with "invalid input" — a confusing error far from the root cause.

Fixes:
- Sanitize values before passing them (make the tfvars reader strip trailing `#` comments and surrounding quotes; handle unquoted numbers/bools, not just quoted strings).
- Prefer passing untrusted/complex values base64-encoded and decoding inside the remote heredoc.
- Keep args simple (ids, ports, base64 blobs) — never pass a raw multi-word string with `#`, `=`, or parens through `ssh host cmd arg`.

Related: [[VNG Cloud vServer Terraform id resolution chain]]

## Related

- [[VNG Cloud vServer Terraform id resolution chain]]

%% ai-graph-start %%

**Related notes:**
- [[ssh 'bash -s' flattens args, so empty middle args shift positionals]]
- [[SSH flattens remote command args, so empty-string arguments collapse and shift positionals]]
- [[ssh drops empty positional args; pass a base64 newline-joined argv + mapfile]]
- [[Escape shell ${VAR} as $${VAR} in a Terraform Cloud Run command list]]
- [[Apostrophe inside bash ${varmessage} breaks the parser]]

%% ai-graph-end %%