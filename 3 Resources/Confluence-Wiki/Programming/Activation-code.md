---
title: "Activation-code"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZPOS/pages/21072653714/Activation-code
space: "LUZPOS"
topic: programming
relevance: 0.812
depth: 3
updated: 2018-03-16
attachments: 0
tags:
  - confluence
  - programming
  - space/luzpos
---

# Activation-code

> [!info] Imported from Confluence
> Space **LUZPOS** · updated 2018-03-16 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZPOS/pages/21072653714/Activation-code)
> Relevance 0.812 · topic `programming`

## Implementation proposal for tablet activation

**Use case:** Company owner can add new tablet to store by using activation code. We use a global table to keep track on the activation codes for each tenant.

The pos_activation is on public schema, since it's not linked to any specific company.

Table pos_activation:

<div>

|  |  |  |  |
|----|----|----|----|
| ID | activation_code (text) | tenant_id (text) | expiry_date (timestamp) |
| 1 | 123434829135 | s_1121c411_7e98_4a1d_ade8_2c31aaa47214 | 2018-01-04 16:08:21.773129 |
| 2 | 582967392947 | s_1c8fd988_8a28_4372_a7fc_ce4634186aee | 2018-01-04 16:08:21.773129 |
| 3 | 375849405967 | s_216b2470_8d4a_4459_bdc3_feaede5be454 | 2018-01-04 16:08:21.773129 |

</div>
