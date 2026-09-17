---
id: KB-B5E612CD
subject: BL-B2B-001 Org switching isolates cart, addresses, and lists
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:06.981Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-B2B-001: Org switching isolates cart, addresses, and lists `[P0-revenue]`

- **Rule:** When a B2B user switches between organizations, the cart, saved addresses, wish lists, and pricing context must completely reset to the new organization's scope. No data from the previous org should leak into the new org's context.
- **Verify:** Org A has items in cart → switch to Org B → cart is empty (or shows Org B's cart). Org A's addresses not visible under Org B. Switch back → Org A's cart restored.
- **Violation signal:** Org A's cart items visible under Org B; addresses from wrong org shown; prices from wrong org applied.
- **Agents:** qa-frontend-expert (org switcher), qa-backend-expert (xAPI context switching)
