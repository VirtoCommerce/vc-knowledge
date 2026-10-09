---
id: KB-4FBD30E2
subject: On Firefox every sorted Admin SPA ui-grid header shows an ellipsis instead of the sort arrow, whatever the column width; Chromium shows the arrow
plane: experiential
question: "Admin SPA ui-grid sortable column headers (e.g. Orders list blade /workspace/orders) on Firefox: is the sort arrow shown after sorting, or is it lost to the header label ellipsis (platform ui-grid header pattern)?"
status: active
appliesTo:
  - axis: browser
    value: firefox
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /workspace/orders
  - coordinate: /workspace/Return
  - coordinate: POST /api/order/customerOrders/search
evidence:
  - method: observation
    deployment: vcptcore_dev
    at: 2026-10-09T21:30:42.727Z
    by: session:61a7b05a
    who: yuskithedeveloper
---
Platform 3.1079.0-alpha.13411, Firefox 151, fresh isolated context at 1920x1080 and 1366x768. Orders list: the default sorted header reads 'Created …' with no down arrow, although its cell has 130 px of free space. Return list (Return 3.1005.0-pr-28): the default 'Return number' header and Create date / Organization (ascending and descending) read '<label> …' with no arrow. aria-sort is correct and the search request carries the sort, so this is rendering only. Each header's .ui-grid-cell-contents is display:inline-block with max-width:100%, so it shrinks to its content, and on Firefox that content always ends about 2 px past the box. The 2 px is the space before ui-grid's hidden sort-priority sub, which has margin-left:-8px. text-overflow:ellipsis then replaces the visible sort icon. A debug override back to width:100% (the platform rule before vc-platform PR 2950 / VCST-4161, first in 3.913.0) brought the arrow back with 0 px overflow. Widening a column cannot fix it.
