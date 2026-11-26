---
id: KB-B2ACE8E1
subject: In dashboard Edit layout, a mouse press on another draggable handle commits an in-flight keyboard move and makes Escape inert, with no cancel announcement; a press on the edit-bar text cancels and announces
plane: experiential
question: What happens to a live keyboard grab on /company/dashboard Edit layout when the user clicks another draggable handle?
status: active
appliesTo:
  - axis: feature
    value: sales-rep-hub-layout
  - axis: role
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/dashboard
evidence:
  - method: observation
    deployment: vcst_qa
    build: 1fe74fdd17a1b14e
    conditions: theme=2.63.0-alpha.2681; store=B2B-store
    defect: suspected
    at: 2026-11-26T04:50:18.103Z
    by: session:779b94fc
    who: kutasinaelena
---
Sales rep, Edit layout, My recent orders grabbed (Space) and moved ArrowDown to position 2 of 2. Clicking the 'Reorder Top sellers' handle left My recent orders at position 2, the announcer still read 'My recent orders moved to position 2 of 2.', and Escape changed nothing (aria-pressed cleared on the grabbed handle). Repeating the setup but clicking .layout-edit-bar__text instead restored the order and announced 'Move of My recent orders cancelled.'
