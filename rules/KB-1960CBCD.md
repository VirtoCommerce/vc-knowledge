---
id: KB-1960CBCD
subject: BL-CART-015 Configuration items survive a Saved-for-Later round trip
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
  - coordinate: Mutations.moveToSavedForLater
  - coordinate: Mutations.moveFromSavedForLater
  - coordinate: Query.configurationItems
evidence:
  - method: observation
    at: 2026-09-17T15:21:02.246Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-CART-015: Configuration items survive a Saved-for-Later round trip `[P1-data]`

- **Rule:** Moving a configurable lineItem to Saved for Later (`moveToSavedForLater`) and back into the cart (`moveFromSavedForLater`) MUST preserve its `configurationItems` (customText, selected option/productId, files, section) unchanged. The lineItem is re-created with a new `lineItemId` on each leg, but its configuration payload is not lost, truncated, or reset to defaults.
- **Verify:** Add a configurable product with a Text-section custom value to cart; confirm via the cart's line-item configuration view. Move it to Saved for Later. Move it back to cart. Confirm the configuration view shows the identical custom value on the new lineItem.
- **Violation signal:** The custom text/option/file is blank, reset to a default, or the section is missing entirely after the item returns to cart.
- **Agents:** qa-frontend-expert (storefront round trip), qa-backend-expert (GraphQL fragment/response verification)
- **Source:** vc-frontend `client-app/core/api/graphql/cart/fragments/fullLineItem.graphql` (`configurationItems` block on `LineItemType`); `.../mutations/moveToSavedForLater/moveToSavedForLaterMutation.graphql` and `.../moveFromSavedForLater/moveFromSavedForLaterMutation.graphql` (both return `cart { ...fullCart }`).
- **Docs:** N/A — implementation detail: the user guide documents the Save-for-Later and product-configuration features but not this field-level persistence guarantee across the move mutations (§1a).
- **Amended:** 2026-07-22 (auto-applied, triangulated — BL-AUDIT-2026-07-22; MISSING → new entry, Source + Live agree, Docs N/A per §1a; scoped to single-item move, bulk not independently verified).

---
