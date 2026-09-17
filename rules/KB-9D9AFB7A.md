---
id: KB-9D9AFB7A
subject: BL-ORD-007 Shipment state machine (detailed)
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:04.088Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-ORD-007: Shipment state machine (detailed) `[P1-data]`

- **Rule:** Live admin Shipment Status dropdown exposes 5 values (verified 2026-04-22 on the environment):
  - `New` → `Pick & Pack` (items being prepared)
  - `Pick & Pack` → `Ready to Send`
  - `Ready to Send` → `Send` (shipped with tracking number — note admin spelling is "Send" not "Sent")
  - `Any state` → `Cancelled`
- **No "Delivered" shipment sub-state.** Delivered/fulfilled semantics are represented at the ORDER level via `OrderStatus = Completed`, not at the shipment level.
- Illegal: `New → Send` (skipping Pick & Pack and Ready to Send); reversing transitions.
- **Verify:** For each illegal transition, attempt via API → expect error. In Admin, verify available actions match current state. Tracking number required for `Send` transition.
- **Violation signal:** State skipped; shipment marked `Send` without tracking number; API allows illegal jump.
- **Storefront label mapping:** per `project_order_status_vocab` memory — admin `Send` → storefront "Shipped"; order-level `Completed` → storefront "Completed" (delivered semantics).
- **Agents:** qa-backend-expert (shipment API), qa-testing-expert (Admin SPA)
