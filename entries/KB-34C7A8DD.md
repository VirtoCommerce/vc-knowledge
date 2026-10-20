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
  - method: observation
    deployment: vcst_qa
    build: f6080d062139e9f0
    conditions: theme=3.1.0-pr-11-d67b-d67b0e0a; store=B2B-store; role=guest; browser=chromium; viewport=1920x1080
    at: 2026-10-20T14:22:49.073Z
    by: session:d992339e
    bodyHash: 3a9b9e04e990
    who: Lenajava1
    contradicts: true
    contested: theme 3.1.0-pr-11-d67b-d67b0e0a vs 2.60.0-pr-2511-81f1-81f18d01; virtocommerce.xcart 3.1042.0 vs 3.1041.0; virtocommerce.xorder 3.1016.0 vs 3.1015.0
    note: On vc-frontend-next 3.1.0-pr-11-d67b (guest, 3 products in one tab, Chromium, 1920x1080 and 390x844) /compare exposes ONE native table (querySelectorAll('table').length is 1) with aria-label 'Compare products', not a raw i18n key. Column headers are th scope=col and the AX tree names each by the product only (no 'Remove from compare' or 'Add to cart' text concatenated); row headers are th scope=row named by the label only (the Pin button is not part of the name), with aria-describedby set only on rows that have a description (Price per unit -> 'Price, excl. VAT, per unit.', Min. order qty). In Differences mode the describedby target stays inside its own row. Header and body are the same table, so a value cell does have a column header.
---
Guest, 5 products in one category tab, Chromium via DevTools verbose snapshot. The tree holds two separate `table` nodes: a header table (rowgroup > row > 5 columnheader, no corner cell) and a body table (row > rowheader + cell per product). The `display:block`/flex overrides do NOT strip the roles in Chromium: table, rowgroup, row, columnheader, rowheader and cell are all exposed. The names are concatenations: each columnheader is "<product> Remove from compare <product> Add to cart", and each rowheader is "<label> [explanation] Pin “<label>”", because the name is computed from content and there is no aria-label. The header and body are different tables, so a value cell has no columnheader association in AT. axe reports `th-has-data-cells` (incomplete) on the header table. Header/body column x-edges match to 0 px at 1920/1280/768/375.
