---
title: "Rethink and decide for testing strategy"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38229346753/Rethink+and+decide+for+testing+strategy
space: "Helios"
topic: testing
relevance: 0.721
depth: 2.38
updated: 2021-02-02
attachments: 2
tags:
  - confluence
  - testing
  - space/helios
---

# Rethink and decide for testing strategy

> [!info] Imported from Confluence
> Space **Helios** · updated 2021-02-02 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38229346753/Rethink+and+decide+for+testing+strategy)
> Relevance 0.721 · topic `testing`

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_38229346753_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-28614" macro-id="f492861e-09d2-4050-9c6d-b24f4e4540d4" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-28614" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-28614</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="008fff86-d373-4f55-906a-92cda7765e41" macro-name="toc">

</div>

##### What

- The unit test (<a href="https://www.guru99.com/unit-testing-guide.html" class="external-link" rel="nofollow">https://www.guru99.com/unit-testing-guide.html</a>)
- The Integration-test (<a href="https://www.guru99.com/integration-testing.html" class="external-link" rel="nofollow">https://www.guru99.com/integration-testing.html</a>)
- End 2 End testing (<a href="https://www.guru99.com/end-to-end-testing.html" class="external-link" rel="nofollow">https://www.guru99.com/end-to-end-testing.html</a>)

##### Why

- Unit test
  - Single unit level 
  - Reduce the side effects when updating feature or touching legacy code.
  - Prevent bugs to occur again.
  - Document the compute/handling at the unit level.
- Integration test
  - the whole logic flows in code, from the start of the request to the end.
  - Reduce the side effects when updating feature or touching legacy code.
  - Prevent bugs to occur again.
  - Document the compute/handling at the module/flow level.
- End to End test
  - acts as QC, the end-user.
  - do all the regression tests automatically.

##### When to apply

- during feature implementation/maintenance
  - Unit test
  - Integration test
- after released one epic
  - End 2 End test

##### Effort/Cost

- Unit Test
  - 100% of coding hours
- Integration Test
  - 100% of coding hours 
- End 2 End Testing
  - Test Server setup
  - Test Client setup
  - Totally different codes will be written
    - maintenance will be doubled
  - Required automation expert?

NOTE: the setup time will depend greatly on the reusable setup, so as the time grows, the percentage will decrease.

##### References

- Unit test (<a href="https://www.guru99.com/unit-testing-guide.html" class="external-link" rel="nofollow">https://www.guru99.com/unit-testing-guide.html</a>)
- Integration-test (<a href="https://www.guru99.com/integration-testing.html" class="external-link" rel="nofollow">https://www.guru99.com/integration-testing.html</a>)
- End 2 End testing (<a href="https://www.guru99.com/end-to-end-testing.html" class="external-link" rel="nofollow">https://www.guru99.com/end-to-end-testing.html</a>)
