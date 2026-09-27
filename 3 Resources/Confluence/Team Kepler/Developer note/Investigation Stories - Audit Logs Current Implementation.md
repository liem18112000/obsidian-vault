---
ai_hash: f049ca219d5193db
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '48830087198'
confluence_path: 'Team Kepler > Developer note > LUZ Audit Refactor- 2025-2026 > LUZ
  Critical Concerns: Brief Summary'
created: 2025-11-04
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- luz-audit
title: 'Investigation Stories: Audit Logs Current Implementation'
type: source
updated: 2025-11-04
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48830087198/Investigation+Stories+Audit+Logs+Current+Implementation
---

# Investigation Stories: Audit Logs Current Implementation

*Confluence source · Team Kepler › Developer note › LUZ Audit Refactor- 2025-2026 › LUZ Critical Concerns: Brief Summary · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48830087198/Investigation+Stories+Audit+Logs+Current+Implementation) · updated 2025-11-04*

------------------------------------------------------------------------

### 1.1.  🔍 Security Investigation

#### 1.1.1.  INV-001: Investigate Authentication Mechanism

**User Story** As a Security Engineer, when we investigate the audit log authentication mechanism, I want to understand how logs are currently authenticated so that we can identify vulnerabilities to forgery attacks and make data-based security decisions.

**Context** The fingerprint chain approach uses SHA256 hashing but may lack proper authentication. There is concern that database administrators could forge logs by calculating valid fingerprints. The team needs to understand current authentication mechanisms before recommending security improvements.

**Acceptance Criteria**

- Document fingerprint calculation algorithm from `FingerprintService`

- Verify whether digital signatures or HMAC are implemented

- Complete proof-of-concept test: Can database admin forge a valid log?

- Identify all actors who can create/modify logs

- Map trust boundaries and access controls

- Produce security risk assessment (Critical/High/Medium/Low)

**Other Information**

**Phase 1: Code Analysis**

- Review `FingerprintService.calculateFingerprint()` implementation

- Check for any cryptographic key usage

- Document SHA256 usage and parameters

**Phase 2: Access Control Review**

- Identify database-level permissions

- Review application-level authentication

- Map user roles and capabilities

**Phase 3: Vulnerability Testing**

- Test forgery scenario with database admin access

- Attempt to inject fake log with valid fingerprint

- Document attack vectors

**Phase 4: Documentation**

- Security analysis report with code references

- Attack vector diagrams (Mermaid)

- Risk assessment with severity levels

- Recommendations for mitigation

**Time-box:** 2 days \| **Story Points:** 3

------------------------------------------------------------------------

#### 1.1.2.  INV-002: Analyze Tamper Detection Capabilities

**User Story** As a Compliance Officer, when we evaluate the audit log tamper detection system, I want to understand how tampering is detected and prevented so that we can assess legal and regulatory compliance risks with data-based evidence.

**Context** The current fingerprint chain is designed to detect tampering, but concerns exist about database-level modifications. Compliance requirements mandate that audit trails be immutable and tamper-evident. The team needs to verify if the current implementation meets regulatory standards (GDPR, SOX, HIPAA).

**Acceptance Criteria**

- Document complete fingerprint chain validation flow

- Test deletion scenario: Delete a log and verify detection

- Test modification scenario: Modify a log and verify detection

- Test rebuild scenario: Can database admin rebuild chain undetected?

- Document chain correction procedures and triggers

- Produce gap analysis against compliance requirements

**Other Information**

**Phase 1: Understanding Current Mechanism**

- Map chain validation algorithm

- Review `ChainValidationService` implementation

- Document validation frequency and triggers

**Phase 2: Tampering Tests**

- Test Case 1: Delete single log from middle of chain

- Test Case 2: Modify log content without updating fingerprint

- Test Case 3: Database admin rebuilds chain after deletion

- Test Case 4: Simulate data corruption

**Phase 3: Database Admin Capabilities**

- Document what database admin can do

- Test if admin can cover tracks

- Identify detection blind spots

**Phase 4: Compliance Assessment**

- Compare capabilities vs GDPR/SOX/HIPAA requirements

- Document gaps and risks

- Provide recommendations with compliance mapping

**Time-box:** 3 days \| **Story Points:** 5

------------------------------------------------------------------------

#### 1.1.3.  INV-003: Audit Timestamp Reliability

**User Story** As a Security Analyst, when we analyze audit log timestamps, I want to verify timestamp accuracy and reliability so that we can determine if chronological forensic analysis is trustworthy.

**Context** Audit logs contain timestamps, but the fingerprint chain order may differ from chronological order. Out-of-order processing could create discrepancies between when events occurred and when they were logged. This affects forensic investigations and compliance reporting.

**Acceptance Criteria**

- Document timestamp source (client-side vs server-side)

- Verify timestamp validation and sanitization logic

- Test out-of-order processing: Measure timestamp vs sequence discrepancies

- Compare chain order with timestamp order across 1000+ logs

- Document clock synchronization mechanisms (NTP, etc.)

- Quantify impact: How often do timestamps mismatch chain order?

**Other Information**

**Phase 1: Source Analysis**

- Trace timestamp generation in code

- Identify client vs server timestamp usage

- Review timezone handling

**Phase 2: Order Testing**

- Create scenario with delayed message processing

- Measure timestamp vs insertion order

- Test with clock skew simulation

**Phase 3: Forensic Impact**

- Analyze production data for discrepancies

- Document impact on audit reports

- Test chronological sorting reliability

**Phase 4: Recommendations**

- Timestamp flow diagram (Mermaid)

- Test results with examples of discrepancies

- Code references and improvement suggestions

**Time-box:** 2 days \| **Story Points:** 3

------------------------------------------------------------------------

### 1.2.  ⚡ Performance Investigation

#### 1.2.1.  INV-004: Measure Current Throughput Limits

**User Story** As a Performance Engineer, when we benchmark the audit logging system, I want to establish baseline throughput metrics so that we can make data-driven decisions about performance improvements and capacity planning.

**Context** The current system has an estimated limit of ~100 logs/sec per tenant, but this needs verification. High-volume customers require 1000+ logs/sec. The team needs concrete performance data to justify architecture changes and set realistic SLAs.

**Acceptance Criteria**

- Set up reproducible load testing environment (JMeter/Gatling)

- Execute load tests: 10, 50, 100, 200, 500, 1000 logs/sec

- Measure actual throughput per tenant at each load level

- Collect latency metrics: p50, p95, p99, p99.9

- Document error rates and retry rates at each load level

- Identify exact bottleneck component (code reference)

**Other Information**

**Phase 1: Test Environment Setup**

- Configure isolated test environment

- Set up monitoring (Prometheus/Grafana)

- Prepare test data generators

**Phase 2: Load Testing Execution**

- Progressive load tests: 10 → 50 → 100 → 200 → 500 → 1000 logs/sec

- Monitor CPU, memory, DB operations

- Capture all metrics in real-time

**Phase 3: Bottleneck Identification**

- Profile application code (JProfiler/YourKit)

- Analyze MongoDB slow queries

- Identify hot spots and contention points

**Phase 4: Results Documentation**

- Load test results spreadsheet with all metrics

- Performance graphs (throughput vs latency)

- Bottleneck analysis report with code references

- Current vs target (1000 logs/sec) comparison table

**Time-box:** 3 days \| **Story Points:** 5

------------------------------------------------------------------------

#### 1.2.2.  INV-005: Analyze Optimistic Lock Contention

**User Story** As a Backend Developer, when we investigate optimistic locking behavior, I want to quantify retry rates and contention impact so that we can make informed decisions about concurrency control mechanisms based on real data.

**Context** The `LastFingerprint` document uses optimistic locking with a version field. Under high concurrency, multiple writes compete for the same document, causing `OptimisticLockException` and retries. This creates a retry storm that degrades performance. The team needs data on how severe this problem is.

**Acceptance Criteria**

- Query production logs for all `OptimisticLockException` occurrences (last 90 days)

- Measure retry rate at different load levels: 10, 50, 100, 200 logs/sec

- Calculate success rate on first attempt vs after retries

- Profile `LastFingerprint.updateLastFingerprint()` execution time

- Identify hot spot documents in MongoDB

- Quantify cost per retry (DB ops, CPU time, latency)

**Other Information**

**Phase 1: Historical Analysis**

- Extract OptimisticLockException from production logs

- Calculate frequency and trends

- Identify peak retry periods

**Phase 2: Load Testing**

- Test concurrent writes: 10, 20, 50, 100 threads

- Measure retry rate at each concurrency level

- Document retry storm behavior

**Phase 3: Code Profiling**

- Profile `FingerprintService.updateLastFingerprint()`

- Measure time spent in retries

- Calculate effective throughput loss

**Phase 4: Cost Analysis**

- Retry statistics report with graphs

- Contention heatmap by tenant

- Code review findings

- Cost analysis: wasted DB operations and CPU time

**Time-box:** 2 days \| **Story Points:** 3

------------------------------------------------------------------------

#### 1.2.3.  INV-006: Profile Database Operations

**User Story** As a Database Administrator, when we analyze database operation patterns, I want to identify optimization opportunities so that we can make data-based decisions about database efficiency and cost reduction.

**Context** Each audit log write requires multiple database operations (read `LastFingerprint`, update `LastFingerprint`, insert `AuditLog`, update `AuditLog`). This write amplification increases costs and latency. The team needs to quantify the exact number and cost of operations.

**Acceptance Criteria**

- Enable and configure MongoDB profiler for audit operations

- Trace all database operations for a single audit log write

- Count and categorize operations: reads, writes, updates

- Identify slow queries (\>100ms) with explain plans

- Review index usage and efficiency for all collections

- Calculate write amplification factor (ops per logical write)

**Other Information**

**Phase 1: Profiler Setup**

- Enable MongoDB profiler (level 2)

- Configure operation sampling

- Set up monitoring dashboards

**Phase 2: Operation Tracing**

- Trace single log write end-to-end

- Document each database call

- Measure individual operation latency

**Phase 3: Performance Analysis**

- Identify slow queries with explain()

- Review index usage vs full scans

- Analyze hot spot collections

**Phase 4: Optimization Report**

- DB operation flow diagram (Mermaid)

- Slow query analysis with explain plans

- Index usage report with recommendations

- Write amplification calculation

**Time-box:** 2 days \| **Story Points:** 3

------------------------------------------------------------------------

#### 1.2.4.  INV-007: Investigate Sequential Processing Bottleneck

**User Story** As a System Architect, when we analyze the sequential processing constraint, I want to understand dependency chains and blocking operations so that we can make informed decisions about parallelization possibilities based on architecture analysis.

**Context** The fingerprint chain requires sequential processing because each log's fingerprint depends on the previous log's fingerprint. This prevents parallel writes and limits throughput. The team needs to understand if any parts can be parallelized or if the architecture must change.

**Acceptance Criteria**

- Map complete fingerprint chain dependency flow

- Identify all blocking operations in `AuditLogCreatingService`

- Review and document `createAuditLog()` step-by-step implementation

- Test hypothesis: Can fingerprints be calculated in parallel?

- Document what would break if we parallelize writes

- Provide parallelization feasibility assessment

**Other Information**

**Phase 1: Dependency Mapping**

- Map data flow from request to database

- Identify dependencies on `LastFingerprint`

- Document blocking points

**Phase 2: Code Review**

- Deep dive: `AuditLogCreatingService.createAuditLog()`

- Review each step's dependencies

- Identify critical section scope

**Phase 3: Parallelization Testing**

- Test removing chain dependency (what breaks?)

- Attempt batch fingerprint calculation

- Document constraints

**Phase 4: Feasibility Report**

- Sequence diagram of log creation (Mermaid)

- Dependency analysis with code references

- Parallelization feasibility: Yes/No/Partial

- Architecture alternatives if parallelization impossible

**Time-box:** 2 days \| **Story Points:** 3

------------------------------------------------------------------------

### 1.3.  🏗️ Architecture Investigation

#### 1.3.1.  INV-008: Map Fingerprint Chain Architecture

**User Story** As a Technical Lead, when we document the audit logging architecture, I want to create comprehensive architecture documentation so that the entire team has the same understanding and can make informed decisions about future changes.

**Context** The fingerprint chain architecture is complex and not fully documented. New team members struggle to understand the design. Before making improvement decisions, everyone needs a shared mental model of how the current system works, including all components, data flows, and failure modes.

**Acceptance Criteria**

- List all components: services, data models, jobs, collections

- Document data models: `AuditLog`, `LastFingerprint` with all fields

- Map service dependencies and interactions

- Create sequence diagrams for 5 key flows (listed below)

- Document MongoDB collection structure and sharding

- Produce architecture diagrams following C4 model

**Other Information**

**Phase 1: Component Inventory**

- List all Java services/classes

- Document all MongoDB collections

- Identify external dependencies

**Phase 2: Data Model Documentation**

- Document `AuditLog` schema

- Document `LastFingerprint` schema

- Explain optimistic locking mechanism

**Phase 3: Flow Documentation** Create sequence diagrams for:

1.  Create audit log (happy path)

2.  Create audit log (with optimistic lock retry)

3.  Chain validation process

4.  Chain correction process

5.  Database failover scenario

**Phase 4: Architecture Deliverables**

- C4 Context diagram

- C4 Container diagram

- Component interaction map

- Sequence diagrams (Mermaid)

- Data model ERD

**Time-box:** 3 days \| **Story Points:** 5

------------------------------------------------------------------------

#### 1.3.2.  INV-009: Review Scalability Characteristics

**User Story** As a Cloud Architect, when we assess system scalability, I want to test vertical and horizontal scaling capabilities so that we can make evidence-based decisions about growth strategies and infrastructure planning.

**Context** The system needs to support growing audit log volumes. It's unclear whether adding CPU, RAM, or application instances will improve throughput. The team needs empirical data on scalability limits and bottlenecks to plan for 2x and 5x growth.

**Acceptance Criteria**

- Test vertical scaling: Measure throughput with 2x, 4x CPU and RAM

- Test horizontal scaling: Measure throughput with 2, 5, 10 instances

- Test per-tenant isolation: Can high-volume tenants be isolated?

- Identify and document all single points of failure

- Assess multi-region deployment feasibility with latency tests

- Define scale ceilings with supporting data

**Other Information**

**Phase 1: Vertical Scaling Tests**

- Baseline: Current CPU/RAM configuration

- Test with 2x resources

- Test with 4x resources

- Measure throughput improvement

**Phase 2: Horizontal Scaling Tests**

- Baseline: Single instance

- Test with 2, 5, 10 instances

- Measure linear scalability

- Identify contention points

**Phase 3: Architecture Analysis**

- Document single points of failure

- Test multi-region latency

- Assess tenant isolation strategies

**Phase 4: Scalability Report**

- Scaling test results (graphs)

- Single points of failure diagram

- Capacity planning model (Excel)

- Multi-region architecture constraints

- Growth recommendations (2x, 5x, 10x)

**Time-box:** 3 days \| **Story Points:** 5

------------------------------------------------------------------------

#### 1.3.3.  INV-010: Analyze Chain Correction Mechanism

**User Story** As an SRE Engineer, when we investigate chain correction procedures, I want to understand recovery time and operational complexity so that we can make risk-informed decisions about system reliability and incident response.

**Context** Chain breaks occur due to bugs, database issues, or manual interventions. Recovery requires chain correction which can take hours. The team needs to understand correction complexity, recovery time, and business impact to assess operational risk.

**Acceptance Criteria**

- Document `ChainCorrectionJob` implementation with code references

- Test chain break scenarios: 1K, 10K, 100K logs to correct

- Measure correction time for each volume

- Document manual intervention steps and decision points

- Analyze 5 different failure scenarios (listed below)

- Calculate MTTR (Mean Time To Resolution) from historical data

**Other Information**

**Phase 1: Code Review**

- Review `ChainCorrectionJob` implementation

- Document correction algorithm

- Identify automated vs manual steps

**Phase 2: Recovery Testing** Test scenarios:

1.  Chain break with 1K subsequent logs

2.  Chain break with 10K subsequent logs

3.  Chain break with 100K subsequent logs

4.  Database failover mid-chain

5.  Partial data loss scenario

**Phase 3: Historical Analysis**

- Review past chain break incidents

- Calculate actual MTTR

- Document lessons learned

**Phase 4: Operational Documentation**

- Chain correction procedure guide

- Recovery time analysis (table)

- Risk assessment matrix

- Incident response runbook

- Automation recommendations

**Time-box:** 3 days \| **Story Points:** 5

------------------------------------------------------------------------

### 1.4.  🐛 Error Analysis

#### 1.4.1.  INV-011: Catalog Current Errors and Failures

**User Story** As a Support Engineer, when we analyze production errors, I want to catalog and quantify all audit-related failures so that we can make prioritized decisions about which problems to fix based on frequency and impact data.

**Context** Production logs contain various audit-related errors, but their frequency and impact are not quantified. The team needs a data-driven understanding of failure modes, error rates, and trends to prioritize improvements effectively.

**Acceptance Criteria**

- Query production logs for all audit-related errors (last 90 days)

- Categorize errors by type with examples

- Calculate error frequency per day and trends

- Identify top 5 most frequent error types

- Review incident reports and correlate with error patterns

- Quantify business impact (failed log writes, user complaints)

**Other Information**

**Phase 1: Log Analysis** Extract and categorize:

- `OptimisticLockException`

- `ChainValidationFailure`

- `FingerprintCalculationError`

- Timeout errors

- Database connection errors

- Other audit-related exceptions

**Phase 2: Statistical Analysis**

- Calculate daily error rates

- Identify trends (increasing/decreasing)

- Correlate with deployment events

- Find patterns (time of day, specific tenants)

**Phase 3: Impact Assessment**

- Quantify failed log writes

- Calculate success rate (%)

- Review user-reported issues

- Assess business impact

**Phase 4: Error Catalog**

- Error frequency report (spreadsheet)

- Trend analysis (graphs for 90 days)

- Top 5 issues ranked by impact

- Root cause analysis for top 3

- Prioritization recommendations

**Time-box:** 2 days \| **Story Points:** 3

------------------------------------------------------------------------

#### 1.4.2.  INV-012: Investigate Chain Break Incidents

**User Story** As an Incident Manager, when we review historical chain break incidents, I want to identify patterns and root causes so that we can make informed decisions about prevention strategies based on incident data.

**Context** Chain breaks have occurred multiple times in production, requiring manual intervention and causing downtime. The team needs to understand why chain breaks happen, how long they take to resolve, and what can be done to prevent them.

**Acceptance Criteria**

- List all chain break incidents from last 12 months with dates

- For each incident, document: root cause, detection time, resolution time, logs affected

- Calculate MTTR (Mean Time To Resolution) average and median

- Identify common root cause patterns

- Review and assess effectiveness of current prevention measures

- Quantify total business impact (downtime hours, customer complaints)

**Other Information**

**Phase 1: Incident Inventory**

- Extract incidents from Jira/ServiceNow

- Review post-mortem documents

- Interview team members

- Create incident timeline

**Phase 2: Root Cause Analysis** For each incident:

- What caused the chain break?

- How was it detected?

- How long to detect vs resolve?

- What was the impact?

**Phase 3: Pattern Identification**

- Group by root cause type

- Identify preventable vs unavoidable

- Analyze detection methods

- Review resolution procedures

**Phase 4: Incident Report**

- Incident timeline with details (table)

- Root cause analysis summary

- MTTR metrics (average, median, p95)

- Pattern analysis

- Prevention recommendations

**Time-box:** 2 days \| **Story Points:** 3

------------------------------------------------------------------------

### 1.5.  💰 Cost Analysis

#### 1.5.1.  INV-013: Calculate Infrastructure Costs

**User Story** As a Finance Manager, when we analyze audit logging infrastructure costs, I want to understand current spending and efficiency so that we can make ROI-informed decisions about optimization investments.

**Context** The fingerprint chain approach may require over-provisioned infrastructure due to inefficiencies. Before investing in improvements, the team needs concrete cost data and projections to calculate ROI and justify the investment.

**Acceptance Criteria**

- Document current MongoDB cluster configuration and monthly cost

- Calculate cost per million audit logs

- Analyze CPU and memory utilization (% used vs provisioned)

- Identify over-provisioning and waste

- Project costs at 2x, 5x, and 10x scale

- Calculate potential savings from optimization

**Other Information**

**Phase 1: Current Cost Inventory**

- MongoDB Atlas monthly cost

- Compute instance costs

- Monitoring tool costs (Datadog, etc.)

- Network/bandwidth costs

**Phase 2: Efficiency Analysis**

- Review resource utilization

- Calculate waste (provisioned vs used)

- Identify inefficiencies

- Cost per operation analysis

**Phase 3: Scaling Projections**

- Model costs at 2x volume

- Model costs at 5x volume

- Model costs at 10x volume

- Compare linear vs actual scaling

**Phase 4: Cost Report**

- Cost breakdown spreadsheet

- Utilization efficiency report (%)

- Scale projection model

- Optimization opportunity list

- Potential savings estimate

**Time-box:** 1 day \| **Story Points:** 2

------------------------------------------------------------------------

#### 1.5.2.  INV-014: Measure Operational Overhead

**User Story** As an Engineering Manager, when we quantify time spent on audit log issues, I want to calculate opportunity cost and team efficiency impact so that we can make investment-justified decisions about improvements.

**Context** Developers spend significant time on audit log issues (bugs, chain corrections, investigations). This time could be spent on features. The team needs to quantify this overhead to justify improvement investments and demonstrate ROI.

**Acceptance Criteria**

- Review Jira tickets: Count audit-related bugs/incidents (last 12 months)

- Calculate time spent on: bug fixes, chain corrections, investigations, support

- Estimate opportunity cost (hours × hourly rate)

- Assess impact on team velocity (sprint capacity consumed)

- Calculate developer efficiency loss (%)

- Project overhead reduction from improvements

**Other Information**

**Phase 1: Time Tracking** From Jira/time logs:

- Bug fix time

- Chain correction time

- Performance investigation time

- Customer support escalations

- On-call incident response

**Phase 2: Opportunity Cost**

- Calculate total hours

- Apply hourly rate (\$100-150/hr)

- Identify feature work delayed

- Assess velocity impact

**Phase 3: Efficiency Impact**

- Sprint capacity consumed (%)

- Context switching overhead

- Team morale impact

- Technical debt accumulation

**Phase 4: Overhead Report**

- Time tracking analysis (spreadsheet)

- Opportunity cost calculation

- Developer efficiency impact (%)

- Velocity trend analysis

- ROI model for improvements

**Time-box:** 2 days \| **Story Points:** 3

------------------------------------------------------------------------

### 1.6.  📊 Investigation Summary

#### 1.6.1.  Story Breakdown

|                 |         |              |              |
|-----------------|---------|--------------|--------------|
| Category        | Stories | Total Points | Time-box     |
| 🔐 Security     | 3       | 11 pts       | 7 days       |
| ⚡ Performance  | 4       | 14 pts       | 9 days       |
| 🏗️ Architecture | 3       | 15 pts       | 9 days       |
| 🐛 Errors       | 2       | 6 pts        | 4 days       |
| 💰 Cost         | 2       | 5 pts        | 3 days       |
| **TOTAL**       | **14**  | **51 pts**   | **~5 weeks** |

#### 1.6.2.  Recommended Execution Order

![[pako-eNp9VG1v2jAQ_iuWJ-0TdHkjKdZUqcAmIY11KmyTVvhgkgtYTezMdqq.png]]

------------------------------------------------------------------------

### 1.7.  📝 Investigation Deliverables

#### 1.7.1.  Technical Artifacts

- Architecture diagrams (C4 model)

- Sequence diagrams (Mermaid for key flows)

- Data model documentation

- Code review findings with references

- Performance test results

- Security analysis report

#### 1.7.2.  Analysis Reports

- Bottleneck analysis

- Scalability assessment

- Security gap analysis

- Error/incident analysis

- Cost-benefit analysis

#### 1.7.3.  Decision Support

- Executive summary (2-page)

- Problem statement document

- Solution options comparison

- ROI calculation

- Risk assessment

- Recommendation with rationale

#### 1.7.4.  Knowledge Base

- Architecture documentation

- Troubleshooting runbooks

- FAQ document

- Lessons learned

------------------------------------------------------------------------

### 1.8.  🎯 Success Criteria

Investigation is complete when:

- ✅ All 14 investigation stories are done

- ✅ Team has shared understanding of current architecture

- ✅ Performance baseline established with data

- ✅ Security gaps identified with severity levels

- ✅ Cost analysis complete with projections

- ✅ Recommendations presented to stakeholders

- ✅ Go/no-go decision made with rationale documented

------------------------------------------------------------------------

### 1.9.  🔗 Key Code Locations

Based on `luz-audit-concerns.md`, focus investigation on:

```
Backend Services:
├── AuditLogCreatingService.java
│   └── createAuditLog()
├── FingerprintService.java
│   ├── calculateFingerprint()
│   ├── getLastFingerprint()
│   └── updateLastFingerprint()
├── ChainValidationService.java
│   └── validateChain()
└── ChainCorrectionJob.java
    └── correctBrokenChain()

Data Models:
├── AuditLog.java
│   ├── fingerprint (String)
│   ├── previousFingerprint (String)
│   ├── content (Object)
│   └── timestamp (Date)
└── LastFingerprint.java
    ├── tenantId (String)
    ├── fingerprint (String)
    ├── version (Long) ← Optimistic lock
    ├── lastAuditLogId (ObjectId)
    └── updatedAt (Date)

MongoDB Collections:
├── auditlog_{tenantId}
└── lastfingerprint_{tenantId}
```

------------------------------------------------------------------------

### 1.10.  📚 Investigation Resources

#### 1.10.1.  Tools Required

- **Load Testing:** JMeter or Gatling

- **Database:** MongoDB Profiler, Compass

- **Java Profiling:** JProfiler, YourKit, or Async-profiler

- **Monitoring:** Grafana + Prometheus

- **Analysis:** Excel/Google Sheets for data analysis

#### 1.10.2.  Reference Documents

- [LUZ Audit Concerns](file:///C:/Users/dvtliem/Kepler/luz_audit/docs/confluence/luz-audit-concerns.md) - Problem analysis

- [Disadvantages Analysis](file:///C:/Users/dvtliem/Kepler/luz_audit/docs/alternative-solution/disadvantages-fingerprint.md) - Technical deep-dive

- Production logs (last 90 days)

- Incident reports (last 12 months)

- MongoDB Atlas metrics dashboard

------------------------------------------------------------------------

**Last Updated:** 2025-11-04

**Status:** Ready for Sprint Planning

**Format:** Standardized user story format with Context, Acceptance Criteria, and Phases

**Next Steps:**

1.  Team review of investigation stories

2.  Assign stories to team members

3.  Schedule investigation sprint (5 weeks)

4.  Set up tools and environments

5.  Execute investigations in phases

6.  Present findings to stakeholders

%% ai-graph-start %%

**Related notes:**
- [[Solution - Enhanced Chain-Signature Hybrid]]
- [[LUZ Critical Concerns - Brief Summary]]
- [[LUZ Audit Refactor- 2025-2026]]
- [[LUZ Audit - Basic Understanding Guide]]
- [[Luz Audit System - Performance Optimization Proposal]]

%% ai-graph-end %%