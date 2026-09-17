---
id: KB-4FFECD14
subject: BL-CART-002 Out-of-stock mid-session
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:00.726Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-CART-002: Out-of-stock mid-session `[P0-revenue]`

- **Rule:** If a product's stock reaches 0 after the user has added it to cart but before checkout completes, the system must prevent the order from being placed. The cart should show an error state for the affected item.
- **Verify:** Add item (stock=2) → in another session/admin, reduce stock to 0 → attempt checkout → error message, order NOT created. Cart should flag the item.
- **Violation signal:** Order placed successfully for an out-of-stock item; no error shown at checkout; oversold inventory.
- **Agents:** qa-frontend-expert (checkout flow), qa-testing-expert (multi-session scenario)
