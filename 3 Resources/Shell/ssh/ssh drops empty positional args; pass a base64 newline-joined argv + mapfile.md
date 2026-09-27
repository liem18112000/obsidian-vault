---
ai_hash: b6d6ca3c71facf4d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-16
entities: []
source: session 2026-09-16
status: seedling
tags:
- ssh
- bash
- deploy
- gotcha
- leo-cdp
- argv
title: ssh drops empty positional args; pass a base64 newline-joined argv + mapfile
type: lesson
---

# ssh drops empty positional args; pass a base64 newline-joined argv + mapfile

Running `ssh host 'bash -s' "$a" "$b" "$c"` does NOT preserve argument boundaries. ssh concatenates all args into a SINGLE command string, sends it, and the remote login shell RE-SPLITS it on whitespace (IFS). Consequences:
- **Empty args vanish** (`"$X"` with X="" collapses to nothing) -> every later positional shifts left.
- Args containing spaces/newlines break apart.

This caused a real incident (leo-customer360 deploy-api.sh): the event-S3 vars were empty on UAT, so `EVENT_S3_BUCKET` / endpoint / keys collapsed and `SMTP_B64` (the last arg) shifted into the `S3_REGION` slot -> boto3 `InvalidRegionError` (500 on every /events query) + the SMTP secret baked into api.env/logs. A LOCAL guard on the value could not catch it -- the corruption happens in the ssh hop, remotely.

## Fix: transport all args as ONE base64 blob of newline-joined fields
```bash
# local
ARGV_B64="$(printf '%s\n' "$A" "$B" "" "$D" | base64 | tr -d '\r\n')"
ssh "${OPTS[@]}" "$host" 'bash -s' "$ARGV_B64" < <(cat <<'REMOTE'
mapfile -t A < <(printf %s "${1:-}" | base64 -d)   # empties + order preserved
X="${A[0]}"; Y="${A[1]}"; Z="${A[2]:-}"; ...
REMOTE
)
```
`printf '%s\n'` emits one line per field (empty field = empty line); base64 survives ssh as a single space-free token; `mapfile -t` splits back, keeping empty elements and exact order. Values must not contain literal newlines (base64 any that might; `tr -d '\r\n'` so Windows Git Bash CRLF does not corrupt the blob). Add a REMOTE-side re-check of critical fields after decode, since a LOCAL guard runs before transport.

Related: [[Validate S3_REGION at the deploy boundary, not after boto3 fails]] [[customer360-api events reader per-source vs single-bucket mode|customer360-api events reader: per-source vs single-bucket mode]]

## Related

- [[Validate S3_REGION at the deploy boundary, not after boto3 fails]]

%% ai-graph-start %%

**Related notes:**
- [[SSH flattens remote command args, so empty-string arguments collapse and shift positionals]]
- [[ssh 'bash -s' flattens args, so empty middle args shift positionals]]
- [[ssh host 'bash -s' flattens args into a remote shell string]]
- [[Validate S3_REGION at the deploy boundary, not after boto3 fails]]
- [[Ship a local bash function to a remote ssh bash -s with declare -f]]

%% ai-graph-end %%