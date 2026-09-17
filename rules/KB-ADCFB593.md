---
id: KB-ADCFB593
subject: BL-LOY-010 Mixed Cart — a points-only cart is rejected; at least one cash line is required
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
  - coordinate: CartType.validationErrors
evidence:
  - method: observation
    at: 2026-09-17T15:21:22.312Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-LOY-010: Mixed Cart — a points-only cart is rejected; at least one cash line is required `[P1-data]`

- **Rule:** A cart whose lines are **entirely** loyalty-currency (points-priced), with no cash-currency line, MUST surface a `LOYALTY_ONLY_POINT_PRODUCTS_NOT_ALLOWED` validation error and MUST NOT be checked out. Adding at least one cash-currency line MUST clear that specific error (the cart becomes valid if no other rule fires). Implements the VCST-5103 AC "at least one common (cash) product must be in the cart, otherwise error."
- **Verify:** Auth as a loyalty user in a Mixed-Cart store; build a cart with only a PTS line → `cart.validationErrors` contains `LOYALTY_ONLY_POINT_PRODUCTS_NOT_ALLOWED`; add a cash line and re-read → that error code is gone.
- **Violation signal:** A points-only cart validates clean / is allowed to check out; or the error fails to clear after a cash line is added.
- **Agents:** qa-backend-expert
- **Source:** vc-module-loyalty #10 `LoyaltyCartValidator.cs` rule 2; VCST-5103 AC. Covered by suite 075b MCO-GQL-006. Live-verified PASS 5/5 on the environment 2026-06-24.
- **Promoted:** 2026-06-24. *(NOTE: PROPOSED-BL-LOY-011 — points-priced products only allowed in Mixed Cart mode, rule 1 — is reserved/pending live verification via the MCO-GQL-007 store-mode flip; see TLC-2026-06-24-1121 bl-proposals.md.)*
