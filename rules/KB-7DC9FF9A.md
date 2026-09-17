---
id: KB-7DC9FF9A
subject: BL-B2B-004 Pre-purchase approval is quote-based; no native per-order spending limit
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
  - coordinate: CustomerOrderType.isApproved
evidence:
  - method: observation
    at: 2026-09-17T15:21:07.411Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-B2B-004: Pre-purchase approval is quote-based; no native per-order spending limit `[P0-revenue]`

- **Rule:** Virto Commerce has **no** native per-order spending-limit / budget-threshold gate, and **no** auto-approval order status (live-verified 2026-07-15: the settable `Order.Status` set is an admin-editable dictionary (e.g. New, Pending, Payment required, Ready for pickup, Completed, Cancelled, Custom, and Processing — all settable, not a fixed enum; see BL-ORD-009) — there is **no** "Pending approval"). Pre-purchase approval is **quote-based**: a buyer submits a Purchase Request / Quote (`submitQuoteRequest`), which an organization approver accepts or declines (`approveQuoteRequest` / `declineQuoteRequest`) before it can become an order. The `CustomerOrderType.isApproved` boolean is a passive data flag with no workflow or mutation behind it — not a spending-limit gate. Any budget-threshold / delegated-limit enforcement is a **custom or roadmap** capability, not stock platform behavior.
- **Verify:** As an org member, create a Purchase Request / Quote → `submitQuoteRequest` → an org approver `approveQuoteRequest` / `declineQuoteRequest` before it is ordered. Introspect xAPI: order-status enum has **no** `PendingApproval`; there is **no** `approveOrder`/`rejectOrder` mutation and no org/member `limit`/`budget` field.
- **Violation signal:** A test asserts a stock budget-limit order-approval workflow ("Pending approval" order status, per-order spending cap, self-approval block) — this feature does not exist in the base platform and must not be asserted as native. (A custom deployment MAY add one; scope such tests to that deployment.)
- **Agents:** qa-frontend-expert (quote request/approval flow), qa-backend-expert (quote xAPI + order-status enum)
