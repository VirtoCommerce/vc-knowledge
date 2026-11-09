---
id: KB-FEC126AB
subject: /account/orders Filters popover keeps un-applied Start/End dates after it is dismissed by outside click or Escape
plane: experiential
question: Are un-applied Created-date entries discarded when the /account/orders Filters popover is closed without Apply?
status: active
appliesTo:
  - axis: feature
    value: orders-date-filter
  - axis: layout
    value: desktop-split
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/orders
evidence:
  - method: observation
    deployment: vcst_qa
    build: e00c89f4da0e9c98
    conditions: platform=3.1083.0; theme=2.60.0-alpha.2579
    at: 2026-10-14T14:58:09.837Z
    by: session:6630b76f
    who: Lenajava1
  - method: observation
    deployment: vcptcore_regression
    build: 9648699210e92bd7
    conditions: platform=3.1076.13; theme=2.59.0; browser=firefox
    at: 2026-11-09T15:01:42.239Z
    by: session:cb5a6bb9
    bodyHash: 90eb0631b9ff
    who: Aleksandra-Mitricheva
    note: "Same: 01/01/2026-06/30/2026 typed, outside click on the Orders heading closed popover, no chip/filter; reopen shows both dates retained."
---
Desktop split layout: on a fresh load typed Start 01/01/2026 and End 06/30/2026 in the Filters popover without Apply, then clicked the page heading outside the popover. The popover closed, no filter chip appeared and the list stayed unfiltered (no GetOrders with a date filter). Reopening Filters showed both dates still filled and Reset/Apply enabled. The same retention happened after closing the popover with Escape. Only Apply, Reset, 'Reset filters' or a page reload clear the draft values. Separately, the applied filter itself is not restored after navigating away and pressing Back, nor after a reload (no URL params).
