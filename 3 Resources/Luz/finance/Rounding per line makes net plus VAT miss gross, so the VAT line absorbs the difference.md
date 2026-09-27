---
title: "Rounding per line makes net plus VAT miss gross, so the VAT line absorbs the difference"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: SAP_Booking.csv — the accounting bookings export (2026-07-27)"
tags: [sap, luz-finance, rounding, accounting, vat, money, gotcha]
---

# Rounding per line makes net plus VAT miss gross, so the VAT line absorbs the difference

Each detail line in `SAP_Booking.csv` is rounded **independently** to two decimals. Sum them back up and `net + VAT` can land a cent away from the invoice's gross — which SAP rejects, because a posting that does not balance is not a posting.

The fix (`adjustSAPAmountForVATInformation`) is to designate **one line as the absorber**: the VAT-info amount is nudged so the identity holds exactly.

```
gross 178.85
net   166.05
raw VAT 12.75      →   166.05 + 12.75 = 178.80   ✗ one cent short
adj VAT 12.80      →   166.05 + 12.80 = 178.85   ✓
```

This is the general shape of every rounding-reconciliation problem: **you cannot round each component independently and expect the total to hold, so pick one component to carry the residual.** Which one is a domain decision — here VAT, because a cent of VAT is adjustable while the net amounts per account must match what was billed.

Two adjacent rules in the same generator, same spirit of "the code map cannot show you this":

- **VAT rate → SAP VAT code** (`generateVATCode`): `7.7% → A1`, `8.1% → C1`, `0%/none → AL`.
- **Posting key** (`generatePostingKey`): a normal booking uses the default (e.g. `50`); a **credit note or negative amount uses `40` and negates the amount** (`LUZ-111391`). Sign is carried by the posting key, not just by the number — get this wrong and the books move the right amount the wrong way.

## Related

- [[SAP in luz_finance is a manual CSV export, not a live integration]]

## Related

- [[SAP in luz_finance is a manual CSV export, not a live integration]]
