---
id: KB-BC61FC6B
subject: At mobile width the /account/orders filter icon button has no accessible name in the accessibility tree
plane: experiential
question: What accessible name does the /account/orders filter button expose at 375px?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: viewport
    value: mobile
anchors:
  - coordinate: /account/orders
evidence:
  - method: observation
    deployment: vcst_qa
    build: e00c89f4da0e9c98
    conditions: platform=3.1083.0; theme=2.60.0-alpha.2579; viewport=375x812; browser=firefox
    defect: suspected
    at: 2026-10-14T14:43:45.361Z
    by: session:6630b76f
    who: Lenajava1
  - method: observation
    deployment: vcptcore_regression
    build: 5609fc0c36e7302c
    conditions: platform=3.1076.7; theme=2.59.0; browser=edge; viewport=375x812
    at: 2026-10-21T13:42:53.379Z
    by: session:624d35a1
    bodyHash: 07eb82579c83
    who: kutasinaelena
    note: "Same on Edge at 375x812: the filter button left of the search box is an unnamed button in the accessibility tree and shows only the icon; Playwright locates it by hasText 'Filters'. At 768px the desktop variant is used and the button is named 'Filters'. Source orders/orders-mobile-filters.vue renders VcButton icon with a <span>Filters</span> child and no aria-label; unchanged since 2.58.0."
---
At 375x812 (Firefox) the button left of the orders search box (44x44, filter icon, opens the Filters sidebar) appears in the accessibility snapshot as an unnamed button; its 'Filters' text span is in the DOM (a has-text match finds it) but not exposed as the accessible name. At 1920 the same control is named 'Filters'. Screen-reader users get an unlabelled button.
