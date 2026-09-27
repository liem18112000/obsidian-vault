---
title: "Dedup fix result 117 to 54 scenarios, 0.58 to 0.62, triplication gone; 0.70 now gated on coverage"
created: 2026-09-23
type: project
status: seedling
source: "session 2026-09-23"
tags: [testing-agent, tpd, dedup, quality, luz-158390, result]
---

# Dedup fix result 117 to 54 scenarios, 0.58 to 0.62, triplication gone; 0.70 now gated on coverage

RESULT of the cross-source semantic dedup fix (LUZ-158390, run-3be0f1e0): WORKED. 117->54 scenarios, 435->81 steps, assured score 0.58->0.62, and the judge "systemic triplication across 3 naming schemes" complaint is GONE. So the dedup is a net win + the correct code lever. TWO caveats: (1) coverage dipped 24%->18% AC×kind — the 0.86 cosine threshold is slightly aggressive and over-merged some DISTINCT security scenarios (zip-bomb/symlink read as near-zip-slip); bump TPD_DEDUP_THRESHOLD toward 0.90 to preserve them. (2) it was a PLAIN re-run (no guidance) so it did not re-request the security/i18n scenarios. WHAT NOW BLOCKS 0.70 changed from duplication to COVERAGE COMPLETENESS — judge wants: zip bomb/deep-nesting/symlink/duplicate-ZIP-entry security, CP437-vs-UTF-8 + NFC/NFD encoding, pool-size-N concurrency comparison + repeated runs, and the 102,399/102,401 size-boundary points, plus splitting one non-atomic 5-in-1 field-failure scenario. PATH TO 0.70: raise threshold to ~0.90 AND re-run WITH the judge coverage guidance (dedup now prevents re-triplication). LESSON: semantic dedup trades a little recall for precision — tune the threshold; too low over-merges distinct-but-similar cases. See [[Cross-source semantic scenario dedup: embed+cosine-cluster, keep canonical, union citations]].

## Related

- [[Cross-source semantic scenario dedup: embed+cosine-cluster]]
- [[keep canonical]]
- [[union citations]]
