---
id: KB-7F57B1FE
subject: BL-BOPIS-004 BOPIS store-selector modal is view-only on PDP
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:14.753Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-BOPIS-004: BOPIS store-selector modal is view-only on PDP `[P1-data]`

- **Rule:** The "Check Availability" / "Pick Up In Store" modal on the Product Detail Page (PDP) is a read-only view. It shows which stores have the product available but does NOT add the product to cart or select a pickup location. Cart addition and pickup-store selection happen from the cart page, not the PDP modal. The modal must close cleanly without side effects.
- **Verify:** Open PDP → open BOPIS modal → verify no "Add to Cart" button inside modal → close modal → cart is empty (no items added) → no store selection persisted.
- **Violation signal:** Modal adds item to cart on open or close; modal persists a store selection without user action; modal has a functional "Add to Cart" button; closing the modal triggers a navigation.
- **Agents:** qa-frontend-expert (PDP modal), qa-backend-expert (console — no addToCart mutation fired)
