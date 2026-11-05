---
id: KB-B77CCAE5
subject: On /catalog at 1280x900 the filters row wraps "Sort by" to a second row once the vertical scrollbar appears, pushing chips and the product grid 50px (CLS ~0.176)
plane: experiential
question: Why does /catalog shift layout on load at 1280x900?
questions:
  - text: What is the CLS of the catalog listing at 1280x900?
  - text: Why does the Sort by control on /catalog move down after load?
  - text: Does the catalog filters row depend on scrollbar width?
concepts:
  - id: layout-shift
status: active
appliesTo:
  - axis: auth
    value: guest
  - axis: page
    value: catalog
  - axis: surface
    value: storefront-ui
  - axis: viewport
    value: 1280x900
anchors:
  - coordinate: /catalog
evidence:
  - method: observation
    deployment: vcptcore_regression
    build: 99880d06bbb9b4cf
    conditions: theme=2.59.0; store=B2B-store; throttle=fast3g; browser=chromium
    defect: suspected
    at: 2026-11-05T07:26:10.059Z
    by: session:35ec50d2
    who: Aleksandra-Mitricheva
---
Guest /catalog at 1280x900, Fast-3G, deterministic layout runner (6 runs): CLS 0.1759. About 0.126 comes from one frame in which .category__sort, .active-filter-chips and .category__products all move 50px; a further 0.043 comes from .category__product-filters moving 420px as the category tree above it in the sidebar resolves. Read-only rect measurement on the lane: .category__filters (flex-wrap, column gap 24, row gap 12) holds view-mode 153 + controls 453 + sort 300 = 954px. With a vertical scrollbar the row is 941px wide, so Sort wraps (row 88px tall, chips y=385, grid y=425). With 1280px of content width (no scrollbar) it is 956px, so everything stays on one row (38px, chips y=335, grid y=375). The difference, 38 + 12 = 50px, equals the recorded shift. The first paint is shorter than the viewport, so there is no scrollbar. When content grows the scrollbar appears, the row wraps and everything below drops 50px. No scrollbar-gutter is reserved. Network 2xx, console clean.
