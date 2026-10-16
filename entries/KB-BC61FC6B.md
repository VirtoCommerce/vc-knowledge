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
---
At 375x812 (Firefox) the button left of the orders search box (44x44, filter icon, opens the Filters sidebar) appears in the accessibility snapshot as an unnamed button; its 'Filters' text span is in the DOM (a has-text match finds it) but not exposed as the accessible name. At 1920 the same control is named 'Filters'. Screen-reader users get an unlabelled button.
