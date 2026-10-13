---
id: KB-34C7A8DD
subject: On /compare the released theme exposes TWO native tables whose header and cell accessible names are noisy concatenations
plane: experiential
question: how is the compare products table exposed in Chromium's native accessibility tree on /compare, and what are the column header and row header names
status: active
appliesTo:
  - axis: engine
    value: chromium
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /compare
evidence:
  - method: observation
    deployment: vcst_qa
    conditions: theme=2.60.0-pr-2511-81f1-81f18d01; role=guest; store=B2B-store
    at: 2026-10-12T16:28:14.871Z
    by: session:7b1a1a36
    who: Lenajava1
    build: 4ca08b7825765bae
    buildFrom: deploy-history
---
Guest, 5 products in one category tab, Chromium via DevTools verbose snapshot. The tree holds two separate `table` nodes: a header table (rowgroup > row > 5 columnheader, no corner cell) and a body table (row > rowheader + cell per product). The `display:block`/flex overrides do NOT strip the roles in Chromium: table, rowgroup, row, columnheader, rowheader and cell are all exposed. The names are concatenations: each columnheader is "<product> Remove from compare <product> Add to cart", and each rowheader is "<label> [explanation] Pin “<label>”", because the name is computed from content and there is no aria-label. The header and body are different tables, so a value cell has no columnheader association in AT. axe reports `th-has-data-cells` (incomplete) on the header table. Header/body column x-edges match to 0 px at 1920/1280/768/375.
