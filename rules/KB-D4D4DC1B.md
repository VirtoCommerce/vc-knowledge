---
id: KB-D4D4DC1B
subject: BL-ORD-001 Order state machine guards
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:03.330Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-ORD-001: Order state machine guards `[P0-revenue]`

- **Rule:** Payment and shipment follow strict state machines with guards:
  - **Payment:** `Pending → Authorized → Paid → Refunded`, with `Authorized → Voided` as a separate pre-capture branch. The capture operation sets `PaymentStatus = Paid` — there is **no `Captured` enum value**. Cannot capture without authorization. Cannot refund without capture (refund requires `Paid`). Void is only possible before capture (from `Authorized`, never from `Paid`). See BL-ORD-006 for the detailed transition table.
  - **Shipment:** `New → Pick & Pack → Ready to Send → Send`. Cannot mark "Send" before "Pick & Pack". No "Delivered" sub-state — delivered semantics live at ORDER level (`OrderStatus = Completed`). See BL-ORD-007 and `project_order_status_vocab` memory.
- **Verify:** In Admin → Order → attempt to skip states (e.g., capture without authorization) → should fail or button should be absent. Verify API rejects invalid state transitions.
- **Violation signal:** Payment captured without prior authorization; shipment marked "Send" while still "New"; state skipped without error.
- **Agents:** qa-backend-expert (order API, state transitions), qa-testing-expert (Admin SPA)
- **Source:** vc-module-order `PaymentFlowService.cs` — `CaptureAllowedPaymentStatuses => [Authorized, Paid]`, `RefundAllowedPaymentStatuses => [Paid, PartiallyRefunded, Refunded]` (Voided excluded); shipment status enum = New / Pick & Pack / Ready to Send / Send (no "Delivered").
- **Amended:** 2026-07-22 (triangulated — BL-AUDIT-2026-07-22; CONFIRMED 3/3, Source anchor recorded, Rule unchanged)
