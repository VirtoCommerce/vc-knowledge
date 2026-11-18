---
id: KB-0BD8E0D5
subject: "On vc-frontend 2.63.0 category, /catalog and search listings page by infinite scroll only (16 cards per batch): no numbered pager and no Load more button at 1280 or 768 px"
plane: experiential
question: Does a storefront category, catalog or search results listing show a numbered pager or a Load more button, or does it load more products on scroll?
concepts:
  - id: catalog
status: active
appliesTo:
  - axis: role
    value: guest
  - axis: setting
    value: catalog_pagination_mode
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /catalog
  - coordinate: /search
  - coordinate: /computer-office-education
evidence:
  - method: observation
    deployment: vcst_qa
    build: 2ff67d68f7987d9e
    conditions: theme=2.63.0-pr-2617-a143-a1435e3f; browser=firefox; setting:catalog_pagination_mode=infinite_scroll (theme default, not read from the stand's theme settings)
    at: 2026-11-18T15:57:01.457Z
    by: session:8bd71225
    who: Lenajava1
---
Guest, Firefox, theme 2.63.0-pr-2617-a143-a1435e3f. /catalog, /computer-office-education and /search?q=laptop, viewports 1280x900 and 768x1024: first paint shows 16 product cards; pressing End appends another 16 each time (16 -> 32 -> 48) with no click; the accessibility snapshot after scrolling holds no pagination landmark, no page-number link or button, no Load next page / Load more button, ending at the Scroll to top button. Source at a1435e3f: client-app/config/settings_data.json sets catalog_pagination_mode to infinite_scroll; category.vue (used by pages/catalog.vue and pages/search.vue) reads it from the theme context with infinite_scroll as fallback; category-products.vue renders VcInfinityScrollLoader for infinite_scroll and a Load previous page / Load next page VcButton pair for load_more. VcPagination is not used on any catalog listing. At 768 px the grid is 3 columns and the filter sidebar stays a sidebar.
