---
id: KB-8DDFC7BA
subject: "A zero-hit keyword search on /search?q= shifts layout heavily on load (CLS ~0.47 at 1920px, ~0.81 at 375px): sidebar, controls and skeleton cards collapse in one frame into the empty view"
plane: experiential
question: Does the storefront /search?q= zero-hit (no results) empty state cause a layout shift on load, and how large?
status: active
appliesTo:
  - axis: aspect
    value: layout-stability
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /search?q=
  - coordinate: Query.products
evidence:
  - method: observation
    deployment: virtostart_demo_admin
    at: 2026-10-09T11:21:05.960Z
    by: session:df9d131f
    who: Lenajava1
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-09T11:40:03.294Z
    by: session:df9d131f
    who: Lenajava1
    contradicts: true
    note: "The entry says the values were 'identical on upstream vc-frontend 2.59.0-pr-2494'. That build (the virtostart stand) is an unmerged demo branch that carries the same new theme, so it is not an upstream baseline. On a genuine upstream build, vc-frontend 2.59.0-pr-2458 (vcptcore_qa, signed in, same method, 2 loads per cell), the zero-hit /search?q= shift is present but NOT identical: 0.644 at 1920x1080 (largest 0.6275: content container 502,192 1158x888 -> 251,192 1404x502, footer and empty-state svg enter), and 0.810 at 375x812 (largest 0.7503). vc-frontend-next 3.0.0-alpha.2685 signed in gives 0.475 at 1920 and 0.807-0.838 at 375. The core claim stands, that the shift exists and is inherited from upstream with the same mechanism. Only the 'identical on upstream' provenance is wrong: upstream is equal at 375 and HIGHER at 1920. See KB-7CE03201."
---
Cold, cache-disabled guest loads of /search?q=<nonsense term>, layout-shift PerformanceObserver (buffered) read ~5 s after load, 2 loads per cell. While the products request is in flight the page renders the full search layout: filter sidebar (desktop), the controls row (sort, view mode, chips) and a page of product skeleton cards (1-column at 375px). When the answer has 0 items, all of it is removed in the same frame and replaced by the short 'Sorry, your search for ... didn't return any results' empty view; the footer jumps into the viewport. Largest shift at 1920x1080: div.vc-layout__content-container moves from x=308 w=1595 h=926 to x=2 w=1901 h=520 plus footer entering at y=778, value ~0.474 (total 0.476-0.487). At 375x812: div.category__body 253->287 and height 559->330 plus footer entering, value 0.806 (total 0.807-0.837). A non-empty search (/search?q=bolt) on the same builds scores 0.002-0.004 at 1920 and 0.035 at 375, so the shift is specific to the zero-hit transition. The values were identical, to the reported precision, on upstream vc-frontend 2.59.0-pr-2494 and on a vc-frontend-next 3.0.0-alpha.2685 build. In upstream source (vc-frontend dev, client-app/shared/catalog/components/category.vue) emptyViewSearchOnly = filteredOnlyBySearch && products.length === 0 && !fetchingProducts, and both hideAllControls and the sidebar's visibility key off it, so the controls cannot be hidden until the fetch resolves; category-products.vue renders itemsPerPage skeletons while fetching. CLS above 0.25 is in the Web Vitals 'poor' band.
