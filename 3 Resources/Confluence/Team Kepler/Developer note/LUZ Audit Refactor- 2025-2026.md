---
title: "LUZ Audit Refactor- 2025-2026"
created: 2025-11-17
updated: 2025-11-17
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48882909190/LUZ+Audit+Refactor-+2025-2026
confluence_id: "48882909190"
confluence_path: "Team Kepler > Developer note"
tags: [confluence, luz-audit]
---

# LUZ Audit Refactor- 2025-2026

*Confluence source · Team Kepler › Developer note · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48882909190/LUZ+Audit+Refactor-+2025-2026) · updated 2025-11-17*

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<tbody>
<tr>
<th><p>**Title**</p></th>
<th><p>**Who should read**</p></th>
<th><p>**Summary**</p></th>
<th><p>**Link**</p></th>
<th><p>**Status**</p></th>
</tr>
&#10;<tr>
<td><p>LUZ Critical Concerns: Brief Summary</p></td>
<td><p>Product Owner, Project Manager, Business Analysis</p></td>
<td><p>The LUZ Audit system has critical issues:</p>
<ul>
<li><p>Security: No authentication, allowing admins to forge or delete logs undetectably; attackers can insert backdated events.</p></li>
<li><p>Functional: Logs must be processed one-by-one with no horizontal scaling, limiting throughput to 100 logs/sec per tenant.</p></li>
<li><p>Performance: Each log requires 5 database operations, causing high overhead and slow processing (~47ms per log).</p></li>
</ul></td>
<td><p>[https://axonivy.atlassian.net/wiki/x/CYCJXgs](https://axonivy.atlassian.net/wiki/x/CYCJXgs)</p></td>
<td><ul>
<li>Investigate</li>
<li>Create document</li>
<li>Propose to Inner Team</li>
<li>Propose to PO</li>
<li>Propose to CTO and others</li>
<li>Confirmed</li>
</ul></td>
</tr>
<tr>
<td><p>LUZ Audit - Basic Understanding Guide</p></td>
<td><p>Everyone</p></td>
<td><p>LUZ Audit is an enterprise audit logging system designed to securely track and record all activities within the LUZ Document Management System for compliance and security purposes.</p>
<ul>
<li><p>LUZ Audit uses a blockchain-like fingerprint chain to prevent unauthorized modifications and ensure tamper-proof logs.</p></li>
<li><p>The system supports asynchronous processing, providing fast response times to clients while handling background operations.</p></li>
<li><p>It features a multi-tenant architecture, allowing multiple companies to use the same platform with isolated data.</p></li>
<li><p>Digital timestamping is integrated to provide legal proof of document existence at specific times.</p></li>
<li><p>The system employs enterprise-grade security measures, including JWT authentication, encryption, and access control, but faces performance limitations due to sequential fingerprint processing.</p></li>
</ul></td>
<td><p>[https://axonivy.atlassian.net/wiki/x/GwDCYAs](https://axonivy.atlassian.net/wiki/x/GwDCYAs)</p></td>
<td></td>
</tr>
<tr>
<td><p>Luz Audit System - Performance Optimization Proposal</p></td>
<td><p>Technical positions</p></td>
<td><p>This document outlines the migration from a Jakarta EE + WildFly + REST-based MongoDB architecture to a Quarkus + Direct MongoDB + Caffeine Cache setup, achieving significant performance improvements.</p>
<ul>
<li><p>The migration resulted in a 34x throughput improvement for single-tenant workloads, increasing from 23 logs/sec to 778 logs/sec.</p></li>
<li><p>Key optimizations include using Caffeine cache for sub-microsecond lookups, AtomicLong for lock-free sequence allocation, and direct MongoDB access to eliminate HTTP overhead.</p></li>
<li><p>The migration plan spans 13 weeks and involves phases such as infrastructure setup, core service migration, and batch processing enhancements.</p></li>
<li><p>The optimized architecture supports 1000+ parallel tenants with virtual threads and achieves a 10x speedup for multi-tenant workloads.</p></li>
<li><p>The migration strategy recommends a blue-green deployment approach to ensure data consistency and performance validation before fully transitioning.</p></li>
</ul></td>
<td><p>[https://axonivy.atlassian.net/wiki/x/SgCHXgs](https://axonivy.atlassian.net/wiki/x/SgCHXgs)</p></td>
<td><ul>
<li>Investigate</li>
<li>Create document</li>
<li>Propose to Inner Team</li>
<li>Propose to PO</li>
<li>Propose to CTO and others</li>
<li>Confirmed</li>
</ul></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>
