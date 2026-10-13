---
id: KB-8DDFC7BA
subject: "A zero-hit keyword search on /search?q= shifts layout heavily on load (CLS ~0.47-0.65 at 1920px, ~0.81 at 375px): sidebar, controls and skeleton cards collapse in one frame into the empty view"
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
    deployment: virtostart
    at: 2026-10-09T11:21:05.960Z
    by: session:df9d131f
    who: Lenajava1
    build: 6c967e42c643aa12
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-09T11:40:03.294Z
    by: session:df9d131f
    who: Lenajava1
    contradicts: true
    note: "The entry says the values were 'identical on upstream vc-frontend 2.59.0-pr-2494'. That build (the virtostart stand) is an unmerged demo branch that carries the same new theme, so it is not an upstream baseline. On a genuine upstream build, vc-frontend 2.59.0-pr-2458 (vcptcore_qa, signed in, same method, 2 loads per cell), the zero-hit /search?q= shift is present but NOT identical: 0.644 at 1920x1080 (largest 0.6275: content container 502,192 1158x888 -> 251,192 1404x502, footer and empty-state svg enter), and 0.810 at 375x812 (largest 0.7503). vc-frontend-next 3.0.0-alpha.2685 signed in gives 0.475 at 1920 and 0.807-0.838 at 375. The core claim stands, that the shift exists and is inherited from upstream with the same mechanism. Only the 'identical on upstream' provenance is wrong: upstream is equal at 375 and HIGHER at 1920. See KB-7CE03201."
    resolved: claim-amended
    resolvedAt: 2026-10-13T06:15:01.911Z
    resolvedBy: session:34990ccd
    resolvedIn: judge/list-sharing-20261012
    resolution: "The shift and mechanism hold, but not 'identical on upstream': upstream vc-frontend 2.60 builds give 0.63-0.67 at 1920 (vcst, vcptcore, 2026-10-13), as the dispute's 0.644; ~0.47 is the demo branch and 3.0.0-alpha."
    amendedFrom: 8d6f07549a3b
    build: f960736e1109b3de
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    conditions: theme=2.60.0-pr-2511-81f1; role=guest
    at: 2026-10-13T06:14:53.173Z
    by: session:34990ccd
    note: "2026-10-13, buffered layout-shift ~5 s after load, 2 runs: zero-hit /search?q= 0.648-0.673 @1920 (largest 0.628-0.647), 0.807-0.809 @375; /search?q=bolt 0.001-0.003 @1920, 0.089-0.091 @375."
    investigation: true
    bodyHash: 4cbfe233a8af
    build: 4ca08b7825765bae
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_qa
    conditions: theme=2.60.0-pr-2522-6f90; role=signed-in
    at: 2026-10-13T06:14:54.991Z
    by: session:34990ccd
    note: "2026-10-13 signed in (guests redirected to /sign-in), 2 runs: zero-hit 0.628-0.629 @1920 (largest 0.612), 0.805 @375; non-empty (printer) 0.001 @1920, 0.090 @375."
    investigation: true
    bodyHash: 4cbfe233a8af
    build: c55c1a124b302d3b
    buildFrom: deploy-history
---
Cold, cache-disabled guest loads of /search?q=<nonsense term>, layout-shift PerformanceObserver (buffered) read ~5 s after load, 2 loads per cell. While the products request is in flight the page renders the full search layout: filter sidebar (desktop), the controls row (sort, view mode, chips) and a page of product skeleton cards (1-column at 375px). When the answer has 0 items, all of it is removed in the same frame and replaced by the short 'Sorry, your search for ... didn't return any results' empty view; the footer jumps into the viewport. Largest shift at 1920x1080: div.vc-layout__content-container moves from x=308 w=1595 h=926 to x=2 w=1901 h=520 plus footer entering at y=778, value ~0.474 (total 0.476-0.487). At 375x812: div.category__body 253->287 and height 559->330 plus footer entering, value 0.806 (total 0.807-0.837). A non-empty search (/search?q=bolt) on the same builds scores 0.002-0.004 at 1920 and 0.035 at 375, so the shift is specific to the zero-hit transition. The values were identical, to the reported precision, on upstream vc-frontend 2.59.0-pr-2494 and on a vc-frontend-next 3.0.0-alpha.2685 build. In upstream source (vc-frontend dev, client-app/shared/catalog/components/category.vue) emptyViewSearchOnly = filteredOnlyBySearch && products.length === 0 && !fetchingProducts, and both hideAllControls and the sidebar's visibility key off it, so the controls cannot be hidden until the fetch resolves; category-products.vue renders itemsPerPage skeletons while fetching. CLS above 0.25 is in the Web Vitals 'poor' band. The 'identical on upstream' provenance does not hold: on upstream vc-frontend builds the 1920 value is higher. Re-measured 2026-10-13 (2 runs per cell): vcst (2.60.0-pr-2511-81f1, guest) zero-hit 0.648-0.673 at 1920 (largest 0.628-0.647: content container h888->502, footer and the background svg enter) and 0.807-0.809 at 375; non-empty 0.001-0.003 at 1920, 0.089-0.091 at 375. vcptcore (2.60.0-pr-2522-6f90, signed in; guests are redirected to /sign-in) zero-hit 0.628 at 1920 and 0.805 at 375, non-empty 0.001 / 0.090. The ~0.474 at 1920 belongs to the virtostart demo branch (2.59.0-pr-2494) and vc-frontend-next 3.0.0-alpha.
