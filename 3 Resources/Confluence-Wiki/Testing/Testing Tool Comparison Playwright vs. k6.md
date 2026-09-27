---
title: "Testing Tool Comparison: Playwright vs. k6"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49068277762/Testing+Tool+Comparison+Playwright+vs.+k6
space: "LUZ"
topic: testing
relevance: 0.909
depth: 3
updated: 2026-01-21
attachments: 0
tags:
  - confluence
  - testing
  - space/luz
---

# Testing Tool Comparison: Playwright vs. k6

> [!info] Imported from Confluence
> Space **LUZ** · updated 2026-01-21 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49068277762/Testing+Tool+Comparison+Playwright+vs.+k6)
> Relevance 0.909 · topic `testing`

The following table summarizes the key functionalities and limitations of each tool as discussed during the meeting:

<div>

|  |  |  |
|----|----|----|
| **Feature** | **Playwright** | **k6** |
| **Primary Focus** | **End-to-End (E2E) UI Testing**. Simulates real user browser actions. | **Load and Performance Testing**. Focuses on backend infrastructure. |
| **Ease of Use** | **Higher.** Features a "Test Generator" (recorder) that creates scripts automatically by clicking through the UI. | **Technical.** Requires more manual coding; lacks a native recorder, though "k6 Studio" was mentioned. |
| **Maintenance** | Brittle to UI changes (e.g., changing IDs or tags). Separation into small test cases is recommended. | Also brittle to ID changes in its UI testing module. |
| **Environment** | Runs on local machines, dev, and staging. Can run "headless" on a server. | Used with a license for cloud-based load tests. |
| **Functionality** | Supports parallel testing. Provides detailed reports and snapshots. | Excellent for multiple iterations and load scenarios. |
| **Limitations** | Potential timeout issues if the server is slow. Not primary choice for heavy load tests. | UI testing is possible but described as the "unwanted sibling" to its load testing strengths. |

</div>

### **Final Conclusion**

The meeting concluded with the decision to **start using Playwright for automated UI testing**. The key factors behind this conclusion were:

- **Specialization:** Playwright is viewed as the superior tool for **functional UI testing**, whereas k6 will remain the standard for **load testing**.

- **Developer Efficiency:** The team preferred Playwright's **Test Generator (recorder)** and user-friendly interface, which make it faster and easier to create and maintain UI scripts compared to k6.

- **Immediate Action:** Team Titan will begin exploring Playwright and start writing test cases, leveraging Miracle's existing code in the master branch (such as authentication flows) to save time.

- **Wider Involvement:** The findings will be shared with Michael and the rest of the peers to align on this direction across teams.
