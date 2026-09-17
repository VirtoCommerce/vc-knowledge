---
id: KB-B3DC5FBD
subject: BL-CAT-001 Stock zero disables purchase
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:08.915Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-CAT-001: Stock zero disables purchase `[P0-revenue]`

- **Rule:** When a product's aggregated stock across all fulfillment centers reaches 0, the storefront must show "Sold out" (or equivalent), and the "Add to Cart" button must be disabled. The product remains visible but non-purchasable.
- **Verify:** Set stock to 0 in Admin → storefront shows "Sold out" label → "Add to Cart" disabled/hidden → attempt via xAPI `addToCart` → error response.
- **Violation signal:** "Add to Cart" still active when stock=0; product purchasable via API despite zero stock; no visual indicator of out-of-stock.
- **Agents:** qa-frontend-expert (PDP, listing), qa-backend-expert (inventory API, xAPI)
