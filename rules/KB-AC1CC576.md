---
id: KB-AC1CC576
subject: BL-CROSS-001 Price list deletion → storefront unavailability
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
  - coordinate: Query.products
evidence:
  - method: observation
    at: 2026-09-17T15:21:10.793Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-CROSS-001: Price list deletion → storefront unavailability `[P0-revenue]`

- **Rule:** When a price list is deleted in Admin, affected products on the storefront must show as "Unavailable" (not $0). The "Add to Cart" button must be disabled. Products without any active price list in the current currency cannot be purchased.
- **Verify:** Delete price list in Admin → storefront product shows "Unavailable" or equivalent, not "$0.00" → "Add to Cart" disabled → xAPI `products` query returns `price: null` or empty price object.
- **Violation signal:** Product displays "$0.00" price after price list deletion; "Add to Cart" remains active; order can be placed for $0.
- **Agents:** qa-backend-expert (pricing API, Admin), qa-frontend-expert (storefront display), qa-testing-expert (end-to-end)
