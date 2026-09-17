---
id: KB-0B50A3FF
subject: BL-BOPIS-006 BOPIS checkout requires billing address, skips shipping address
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:15.097Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-BOPIS-006: BOPIS checkout requires billing address, skips shipping address `[P1-data]`

- **Rule:** For a pure-BOPIS checkout (all items are pickup), the checkout form must NOT display a shipping address section. The billing address is still required (for payment processing). This is a strict fulfillment-type-driven form variant — the absence of the shipping address section is correct behavior, not a bug. For mixed carts (BOPIS + delivery), the shipping address section IS displayed (for the delivery items).
- **Verify:** Pure BOPIS cart → checkout → no shipping address form visible → billing address form IS visible → order placed successfully with billing address only → Admin order shows pickup location, no shipping address.
- **Violation signal:** Shipping address form shown for pure-BOPIS checkout; billing address not required; order placed without any address; shipping address form missing for mixed-cart checkout (delivery items need it).
- **Agents:** qa-frontend-expert (checkout form structure), qa-backend-expert (order address fields in xAPI)
