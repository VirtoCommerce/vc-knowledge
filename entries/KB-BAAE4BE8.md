---
id: KB-BAAE4BE8
subject: ProductComparison.Enabled = false hides all storefront compare UI, makes /compare a 404 and refuses saveProductsToCompareList with COMPARE_LIST_DISABLED; the list survives and returns when re-enabled
plane: experiential
question: What happens on the storefront and xAPI when the store setting ProductComparison.Enabled is turned off (/compare, Mutation.saveProductsToCompareList)?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /compare
  - coordinate: Mutation.saveProductsToCompareList
  - coordinate: Query.compareList
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: c2cbb357a76b771a
    conditions: theme=2.63.0-pr-2635-63a1; module:VirtoCommerce.ProductComparison=3.1000.0-pr-1-98d6; store=B2B-store
    at: 2026-11-17T14:57:52.438Z
    by: session:fcdb91ae
    who: kutasinaelena
---
With ProductComparison.Enabled=false on the store (after a page reload — the storefront reads it from InitializeApplication store.settings.modules per session): category cards and the PDP show no compare toggle, the header has no Compare entry, /compare answers "Page not found" (404) for guests and buyers; saveProductsToCompareList is refused with errors[].extensions.code COMPARE_LIST_DISABLED ("Product comparison is disabled for this store") for anonymous and signed-in callers; compareList returns null with no error. Switching it back on and reloading shows the buyer's previous list unchanged (same list id, same items).
