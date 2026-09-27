---
title: "Part B: luz-antivirus Analysis"
created: 2025-12-18
updated: 2025-12-18
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48989208583/Part+B+luz-antivirus+Analysis
confluence_id: "48989208583"
confluence_path: "Team Kepler > Developer note > Service Error Analysis Report: FAILED_TO_STORE on Production"
tags: [confluence, luz-antivirus]
---

# Part B: luz-antivirus Analysis

*Confluence source · Team Kepler › Developer note › Service Error Analysis Report: FAILED_TO_STORE on Production · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48989208583/Part+B+luz-antivirus+Analysis) · updated 2025-12-18*

------------------------------------------------------------------------

### B.1 Error Statistics

|                                |        |          |
|--------------------------------|--------|----------|
| Error Type                     | Count  | Severity |
| HTTP 503 (Health Check)        | 1,000+ | LOW      |
| HTTP 504 (Scanner Timeout)     | 6      | MEDIUM   |
| Malware Detected               | 0      | \-       |
| Service Unavailable (External) | 0      | \-       |

------------------------------------------------------------------------

### B.2 Service Unavailability Analysis (HTTP 503)

#### B.2.1 503 Events Summary (Sample - Dec 9, 2025)

|                 |       |           |          |            |           |               |
|-----------------|-------|-----------|----------|------------|-----------|---------------|
| Timestamp (UTC) | Pod   | First 503 | Last 503 | Duration   | 503 Count | Cause         |
| 2025-12-09      | psqxq | 20:02:20  | 20:02:36 | **16 sec** | 9         | ClamAV update |
| 2025-12-09      | 8zsjn | 20:02:04  | 20:02:18 | **14 sec** | 8         | ClamAV update |
| 2025-12-09      | 2jn6g | 19:33:02  | 19:33:20 | **18 sec** | 10        | ClamAV update |
| 2025-12-09      | r79fj | 19:32:48  | 19:33:00 | **12 sec** | 7         | ClamAV update |
| 2025-12-09      | l6rmb | 19:01:54  | 19:02:08 | **14 sec** | 8         | ClamAV update |
| 2025-12-09      | k8tzc | 18:31:50  | 18:32:04 | **14 sec** | 8         | ClamAV update |
| 2025-12-09      | lqbjt | 17:32:55  | 17:33:15 | **20 sec** | 11        | ClamAV update |
| 2025-12-09      | b5grh | 17:05:00  | 17:05:14 | **14 sec** | 8         | ClamAV update |
| 2025-12-09      | 4xz8x | 17:04:43  | 17:04:59 | **16 sec** | 9         | ClamAV update |
| 2025-12-09      | xsf4m | 17:02:52  | 17:03:12 | **20 sec** | 11        | ClamAV update |
| 2025-12-09      | dwc8h | 16:43:41  | 16:43:57 | **16 sec** | 9         | ClamAV update |

#### B.2.2 Root Cause Analysis

|                       |                                               |
|-----------------------|-----------------------------------------------|
| Factor                | Finding                                       |
| **Trigger**           | freshclam downloads virus definition updates  |
| **Impact**            | clamd daemon restarts to load new definitions |
| **Duration**          | 12-20 seconds per update (avg 15 seconds)     |
| **Frequency**         | ~10-12 updates per day per pod                |
| **Probe interval**    | 2 seconds (readinessProbe.periodSeconds)      |
| **Failure threshold** | 1 (readinessProbe.failureThreshold)           |
| **503 per incident**  | 7-11 responses (duration ÷ probe interval)    |

#### B.2.3 Update Flow

![[3 Resources/Confluence/Team Kepler/Developer note/attachments/part-b-luz-antivirus-analysis/image-20251210-101331.png]]

 

#### B.2.4 Conclusion

**Root Cause:** ClamAV virus definition updates require daemon restart. During restart (~15 seconds), the clamd socket is unavailable, causing health checks to fail.

**Expected Behavior:** This is normal ClamAV operation. With 4-8 pods (HPA), updates are staggered across pods, maintaining overall service availability.

**Not a bug** - this is expected behavior for ClamAV with automatic virus definition updates

------------------------------------------------------------------------

### B.3 Scanner API Timeouts (HTTP 504)

#### B.3.1 Timeout Events Summary

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| Timestamp (UTC) | Pod | User | File Size | Duration | Error |
| 2025-11-24 09:06:41 | v5vsx | [support@printcom.ch](mailto:support@printcom.ch) | **46 MB** | 300.6s | SocketTimeoutException |
| 2025-11-24 09:05:35 | 492lq | [support@printcom.ch](mailto:support@printcom.ch) | **75 MB** | 300.7s | SocketTimeoutException |
| 2025-11-24 09:05:28 | v5vsx | admin | **37 MB** | 300.1s | SocketTimeoutException |
| 2025-11-24 09:04:23 | 492lq | [support@printcom.ch](mailto:support@printcom.ch) | **74 MB** | 300.6s | SocketTimeoutException |
| 2025-11-24 09:03:53 | v5vsx | [support@printcom.ch](mailto:support@printcom.ch) | **85 MB** | 301.1s | SocketTimeoutException |
| 2025-11-24 09:03:28 | 4skkn | [support@printcom.ch](mailto:support@printcom.ch) | **44 MB** | 300.4s | SocketTimeoutException |

#### B.3.2 Root Cause Analysis

|  |  |
|----|----|
| Factor | Finding |
| **Tenant** | `227d3d71-9c70-4e89-b934-6c3a7c0d1cae` (same tenant, 5 of 6 cases) |
| **User** | `support@printcom.ch` (5 of 6 cases) |
| **File sizes** | 37-85 MB (very large PDFs) |
| **Duration** | All ~300 seconds (exactly at timeout limit) |
| **Timeout config** | `QUARKUS_ANTIVIRUS_CLAMAV_SCAN_TIMEOUT=300000` (300s) |

#### B.3.3 Conclusion

**Root Cause:** Single tenant uploading very large PDF files (37-85 MB) that exceeded the 5-minute scan timeout.

**Not a system issue** - this was user behavior with unusually large files. The timeout configuration (300s) is appropriate for normal file sizes.

------------------------------------------------------------------------

### B.4 Malware Detection Analysis

#### B.4.1 Scan Results (31 Days)

|                 |       |
|-----------------|-------|
| Result          | Count |
| OK (Clean)      | 100%  |
| FOUND (Malware) | 0     |

**Conclusion:** No malware or viruses were detected in any scanned files over the 31-day period.

------------------------------------------------------------------------

### B.5 Architecture Overview

![[3 Resources/Confluence/Team Kepler/Developer note/attachments/part-b-luz-antivirus-analysis/image-20251210-101831.png]]

 

#### B.5.1 Component Versions

|                  |                  |                    |
|------------------|------------------|--------------------|
| Component        | Version          | Purpose            |
| ClamAV           | 1.4.3            | Antivirus engine   |
| Virus DB         | 27845            | Signature database |
| Quarkus          | Native (GraalVM) | REST API framework |
| luz-clamav image | 1.4.101          | ClamAV container   |
