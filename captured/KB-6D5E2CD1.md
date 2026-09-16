---
id: KB-6D5E2CD1
subject: what makes a price list apply to a store, and what the storefront reads it through
plane: experiential
question: Which price list's price does a B2B storefront shopper actually get, and what decides it?
status: active
refutableBy: observation
appliesTo:
  - axis: surface
    value: admin-ui
  - axis: surface
    value: rest
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: GET /api/pricing/assignments
  - coordinate: PricelistAssignment.storeId
  - coordinate: PricelistAssignment.dynamicExpression
evidence:
  - method: observation
    deployment: vcptcore_stable
    at: 2026-09-16T12:26:23.843Z
    by: session:8ec21246
---

A pricelist reaches a storefront shopper only through a PricelistAssignment, and the assignment is scoped EITHER by catalogId OR by storeId -- both routes work and the Admin blade makes them mutually exclusive (picking a Store greys out Catalog). The store's own catalog is what a catalogId-scoped assignment must match: B2B-store's catalog is a VIRTUAL catalog, and an assignment on it applies to products whose own catalogId is the underlying physical catalog, so do not look for the assignment on the product's catalog. Four things gate an assignment and all four were observed to bite independently: (1) scope match, catalog or store; (2) the date window -- setting endDate to a past instant on an otherwise-matching assignment made the storefront fall straight back to the next-best pricelist, with no other change; (3) the condition tree under 'Eligible shoppers', which on this deployment is in practice a single condition type, UserGroupsContainsCondition; (4) priority, higher wins -- an assignment at priority 20000 beat a matching unconditional one at priority 1 for the same product. An assignment with an EMPTY BlockPricingCondition (children: []) is unconditional and matches everybody, which is how the baseline lists here are wired. A pricelist with no assignment at all is invisible to the storefront however many prices it holds. Pricelist.priority exists on the REST model but the Admin price-list blade offers no field for it; the number that decides anything is the ASSIGNMENT's priority.
