---
title: "Cross-cutting pass validated: security/i18n/concurrency now generated; score 0.62, 0.70 is a completeness long tail"
created: 2026-09-23
type: project
status: seedling
source: "session 2026-09-23"
tags: [testing-agent, tpd, crosscutting, quality, luz-158390, validated]
---

# Cross-cutting pass validated: security/i18n/concurrency now generated; score 0.62, 0.70 is a completeness long tail

VALIDATED (LUZ-158390, live subscription): the cross-cutting generation pass WORKS — the suite now contains the distinct security (zip-slip, zip-bomb/high-compression, deeply-nested folders, symlink entries, duplicate entry names, JSON-bomb), i18n (CP437/UTF-8 decode, NFC-vs-NFD), and concurrency (pool-size-identical, concurrent-same-folder, no-lost-counters) scenarios that guidance alone could NEVER inject before. 57 clean scenarios, no triplication. BUT the assured score held at 0.62 (did not cross 0.70): the coverage gain was OFFSET because the judges critique shifted from "kinds missing" to finer WITHIN-CELL completeness + a few residual issues: 2x2 valid/invalid × healthData matrix only 2/4 cells; size boundary only at 102,399 (missing 102,400/102,401); negative EP set half-covered (missing companyId-wrong-type, empty-title, documentTypes-as-string); one scenario asserts the gap-1 token-mismatch outcome instead of marking it BLOCKED (groundedness hit); ~3 residual NFC/NFD near-dups the 0.86 cosine dedup did not merge. NET: dedup + cross-cutting are both real improvements (triplication gone, cross-cutting kinds present); 0.70 is now a LONG TAIL of generation-completeness (enumerate each boundary point rather than one; fill every matrix cell; block gap-1 cases; slightly tighter dedup for near-identical i18n variants) — each a small targeted change, diminishing returns. LESSON: adding coverage can leave the aggregate score flat when the additions also introduce new dup/groundedness penalties — the judge rebalances to the next weakest dimension. See [[Cross-source semantic scenario dedup: embed+cosine-cluster, keep canonical, union citations]] and [[0.70 is an architecture ceiling not a tuning miss: guidance cannot inject cross-cutting kinds]].

## Related

- [[0.70 is an architecture ceiling not a tuning miss: guidance cannot inject cross-cutting kinds]]
