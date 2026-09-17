---
id: KB-96EC4E8E
subject: BL-CHK-001 Guest vs authenticated checkout
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:02.366Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-CHK-001: Guest vs authenticated checkout `[P0-revenue]`

- **Rule:** Guest checkout is only available when the store setting `createAnonymousOrderEnabled = true`. When disabled, anonymous users must sign in before reaching checkout. Guest checkout skips address book (no saved addresses) and order history is not linked to an account.
- **Verify:** With flag OFF → "Add to cart" → "Checkout" → redirect to sign-in. With flag ON → anonymous user can complete full checkout without account.
- **Violation signal:** Anonymous user reaches checkout when flag is OFF; guest order appears in a registered user's order history; saved addresses shown to guest.
- **Agents:** qa-frontend-expert (checkout flow), qa-backend-expert (store settings API)
- **Source:** vc-frontend `client-app/pages/checkout/index.vue` — no auth guard on the checkout route; a guest may initialize and complete checkout, and the guest order is not linked to an account.
- **Amended:** 2026-07-22 (triangulated — BL-AUDIT-2026-07-22; CONFIRMED 3/3, Source anchor recorded, Rule unchanged)
