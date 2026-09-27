---
title: "Order a test matrix by feedback latency - smoke, sync rejections, volume last"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: ePost ZIP Import Test Fixture Matrix (2026-08-14)"
tags: [testing, test-design, epost, zip-import, qa, luz-docs-import]
---

# Order a test matrix by feedback latency - smoke, sync rejections, volume last

The ePost ZIP-import suite (40+ fixtures) is ordered by **how fast a failure tells you something**, not by feature area:

1. **Smoke** (`01`) — if the happy path fails, nothing else is meaningful. Run it alone first.
2. **Synchronous rejections** — validator and request-shape failures that never create a job. Fastest feedback, no async waiting, no cleanup.
3. **Structure & encoding** against existing behaviour.
4. **New feature surface** (sidecar metadata).
5. **Limits** — file-type allow-list, per-document size.
6. **External dependency** (antivirus) — needs a service or stub, so it is quarantined behind everything that does not.
7. **Idempotency & duplicates** — requires running a fixture twice.
8. **Precedence & notifications.**
9. **Security** — *"before anything ships."*
10. **Concurrency & tuning** — environment must be set before class-load, so these cannot share a JVM with the rest.
11. **Volume** (500 docs) — **last**, because it is slow and its failures are usually symptoms of something an earlier case already caught.

Three transferable principles:

- **Order by feedback latency, not by taxonomy.** Synchronous rejections before anything that creates a job; volume last.
- **Push tests with environmental prerequisites down the list** (AV service, pre-class-load env vars) so a missing dependency does not block the cheap signal.
- **A case that must run twice** (idempotency) belongs after the single-run cases pass, or you cannot tell a duplicate-handling bug from an import bug.

The matrix also asserts on one stable surface per run — `GET {tenant-id}/import-jobs/{id}`, checking `status`, `failureCode`, and the `successfulFiles` / `skippedFiles` / `rejectedFiles` / `failedFiles` / `unprocessedFiles` buckets. **Fixed assertion shape across 40 fixtures** is what makes a matrix this size maintainable.

## Related

- [[Silently-ignored input needs a visible reason field, or it looks like data loss]]

## Related

- [[Silently-ignored input needs a visible reason field, or it looks like data loss]]
