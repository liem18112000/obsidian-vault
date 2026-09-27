---
title: "MongoTimeoutException from UnknownHostException is a DNS fault, not DB overload"
created: 2026-09-09
type: lesson
status: seedling
source: "PROD cross-check 2026-09-09"
tags: [mongodb, dns, coredns, root-cause, debugging, gotcha, luz]
---

# MongoTimeoutException from UnknownHostException is a DNS fault, not DB overload

A `com.mongodb.MongoTimeoutException: Timed out after 30000 ms while waiting for a server that matches ...` is NOT evidence that MongoDB is overloaded/OOM. Read the CAUSAL exception embedded in the "Client view of cluster state" — if servers show `type=UNKNOWN` with `java.net.UnknownHostException`, the failure is **DNS name resolution**, and the database is fine (up, fast). This is a control-plane/DNS fault, not a data-plane one.

Hard-won during the Luz PROD "MongoDB killed at 08:39 CEST" incident (2026-09-08): I initially (wrongly) inferred MongoDB **saturation -> OOM** from app-side symptoms (a MongoTimeoutException storm + 90-109s request "latencies"). A reference report flagged DNS; re-checking PROD proved it: the `time-consuming=90000..109000` values were requests blocked on DNS/server-selection **retries** (30s timeouts stacking), not query execution; slowest real query ~2.4s. The proximate cause was `java.net.UnknownHostException` on the mongod hostnames while **`coredns-custom` failed its liveness probe (HTTP 404) and was killed repeatedly**.

Diagnostic checklist for "DB timeouts" before blaming the DB:
1. Grep app logs for `UnknownHostException | Name or service not known | Temporary failure in name resolution` -> DNS.
2. Check the driver`s cluster-state text: `type=UNKNOWN`/`CONNECTING` + a socket-open exception = cannot reach/resolve, not slow queries.
3. Check status codes of the "slow" requests: many **200s** at 30-100s means requests eventually succeeded (intermittent DNS), not a saturated DB (which would fail or stay slow).
4. Check CoreDNS / kube-dns pod health + liveness/readiness events (`Killing ... failed liveness probe`, `statuscode: 404`).
5. Only claim DB saturation/OOM if you actually have mongod-side evidence (slow-query log, currentOp, OOMKilled). If you cant read those, do NOT assert it.

Also: correlation (my enrichment dose-response) can be real yet causally mis-assigned. High request volume drove high MongoClient creation -> high DNS lookup volume -> more failures when DNS was flaky. That is an amplifier, not proof the DB was the bottleneck.

## Related
[[luz-docs enrichment re-run campaign saturates PROD MongoDBs each morning]]
[[Diagnose all-DBs-die-at-time-T with a dose-response table across crash vs quiet days]]
[[Intermittent DB saturation = stacked loads crossing a fixed ceiling]]

## Related

- [[luz-docs enrichment re-run campaign saturates PROD MongoDBs each morning]]
- [[Diagnose all-DBs-die-at-time-T with a dose-response table across crash vs quiet days]]
