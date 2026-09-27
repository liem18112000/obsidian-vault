---
title: "LUZ Critical Concerns: Brief Summary"
created: 2025-11-04
updated: 2025-11-17
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48830709769/LUZ+Critical+Concerns+Brief+Summary
confluence_id: "48830709769"
confluence_path: "Team Kepler > Developer note > LUZ Audit Refactor- 2025-2026"
tags: [confluence, luz-audit]
---

# LUZ Critical Concerns: Brief Summary

*Confluence source · Team Kepler › Developer note › LUZ Audit Refactor- 2025-2026 · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48830709769/LUZ+Critical+Concerns+Brief+Summary) · updated 2025-11-17*

------------------------------------------------------------------------

### 🔴 Security Flaws

|  |  |  |  |
|----|----|----|----|
| Vulnerability | Severity | Attack Vector | Impact |
| **No Authentication** | CRITICAL | Database admin can forge logs | Untraceable log injection |
| **Database Admin Attack** | CRITICAL | Rebuild chain after deletion | Evidence destruction undetectable |
| **Timestamp Manipulation** | HIGH | Out-of-order processing | Chronological inconsistencies |

#### Authentication Vulnerability

![[image-20251104-072415.png]]

**Problem:** SHA256 alone cannot prove WHO created the log.

#### Database Admin Attack Flow

![[image-20251104-072523.png]]

#### Timestamp Manipulation Attack

![[image-20251111-101314.png]]

**Problem:** Chain validates fingerprint order but NOT timestamp order. Attackers can insert backdated events that appear legitimate.

------------------------------------------------------------------------

### ⚠️ Functional Flaws

|  |  |  |
|----|----|----|
| Issue | Description | Business Impact |
| **Sequential Processing Lock** | Must process logs one-by-one | Cannot handle high-volume customers |
| **No Horizontal Scaling** | Single tenant = single thread | Growth ceiling at 100 logs/sec |

#### Sequential Processing Bottleneck

![[image-20251104-072644.png]]

#### No Horizontal Scaling

![[image-20251111-101554.png]]

**Problem:** Single tenant = single thread processing. Adding more servers does NOT increase throughput. Growth ceiling at 100 logs/sec per tenant.

![[image-20251111-101925.png]]

------------------------------------------------------------------------

### 🐌 Performance Issues

|  |  |  |  |  |
|----|----|----|----|----|
| Use Case | Problem | Root Cause | Impact | Severity |
| **Write Amplification** | 5 DB operations per log | Chain maintenance overhead | 47ms per log (5x amplification) | 🟡 HIGH |

#### Write Amplification

![[image-20251104-073248.png]]

**Write Amplification** refers to the performance overhead where a single audit log entry requires multiple database operations instead of just one. In this system, each log requires **5 database operations** (insert log, read fingerprint, calculate hash, update fingerprint, update log), creating a **5x amplification factor**. This means the database performs 5 times more operations than necessary, resulting in ~47ms processing time per log and significantly increased database load and contention.
