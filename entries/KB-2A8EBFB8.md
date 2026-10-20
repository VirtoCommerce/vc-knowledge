---
id: KB-2A8EBFB8
subject: Only the catalog/search grid (category.vue) sends view_item_list for an empty product list; every other list in the theme skips an empty list
plane: experiential
question: Which code sends a view_item_list with an empty items array when a catalog or search grid has zero results, and do other lists do the same?
questions:
  - text: Why does a zero-result category search push view_item_list with items []?
  - text: Does the storefront analytics module drop view_item_list events with no items?
  - text: Which view_item_list callers check the list length before sending?
  - text: Does the dom-analytics engine fire view_item_list for an empty product-list container?
status: active
appliesTo:
  - axis: element
    value: google-analytics
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /search
  - coordinate: window.dataLayer
evidence:
  - method: source
    deployment: vc-frontend-next@d67b0e0a
    defect: suspected
    at: 2026-10-20T15:56:53.830Z
    by: session:d992339e
    who: Lenajava1
    note: "distinct: The held entries record the observed dataLayer push; this records the code that produces it and the theme-wide contrast (every other view_item_list path suppresses an empty list), which neither entry states."
---
In client-app/shared/catalog/components/category.vue (also used by pages/search.vue), fetchProducts() and changeProductsPage() call analytics("viewItemList", products.value, categoryListProperties.value) with no length check; the only skip is the single-barcode-hit redirect. modules/google-analytics/events.ts viewItemList maps and sends whatever it receives (items_count = length), so a 0-result grid yields view_item_list with items [] and items_count 0. All other manual callers guard on length first: pages/shared-list.vue, pages/account/list-details.vue, pages/account/saved-for-later-details.vue, the header search dropdown (only when the dropdown is visible and has products), and useCompareProductsPage.ts. The dom-analytics engine (related, recommended, recently browsed, variations widgets) returns undefined for an empty collect source, so the required argument is missing and the event is skipped. The same unguarded category.vue calls exist upstream in vc-frontend 2.40.0 and dev. The VirtoOZ ga-events page and Google's view_item_list reference say nothing about an empty items array.
