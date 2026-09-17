---
id: KB-E98ECDD4
subject: BL-B2B-003 Quote expiry makes quote non-convertible
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:07.268Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-B2B-003: Quote expiry makes quote non-convertible `[P1-data]`

- **Rule:** Quotes (RFQ) have an expiration date set by the seller. After expiry, the buyer cannot convert the quote to an order — the "Convert to Order" action must be disabled or show an "Expired" message. Expired quotes remain visible in history but are not actionable. The seller can extend or reissue an expired quote.
- **Verify:** Create quote with expiry in 1 hour → wait for expiry → buyer attempts to convert → blocked with "Quote expired" message. Seller re-opens and extends → buyer can now convert.
- **Violation signal:** Expired quote converted to order; no expiry indication shown; "Convert" button active on expired quote; order placed at expired quote prices.
- **Agents:** qa-frontend-expert (quotes UI), qa-backend-expert (quotes API)
