---
id: KB-C963BFF2
subject: BL-ORD-002 Cancellation restores inventory conditionally
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:03.454Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-ORD-002: Cancellation restores inventory conditionally `[P1-data]`

- **Rule:** When an order is cancelled, inventory is restored ONLY if the "Adjust inventory on order cancellation" flag is enabled in store settings. Without the flag, cancellation does NOT restore stock — manual inventory adjustment required.
- **Verify:** Enable flag → cancel order → check FFC inventory (should increase). Disable flag → cancel order → inventory unchanged.
- **Violation signal:** Inventory restored when flag is OFF; inventory NOT restored when flag is ON; stock count mismatch after cancellation.
- **Agents:** qa-backend-expert (inventory API, order API), qa-testing-expert (Admin SPA workflow)
