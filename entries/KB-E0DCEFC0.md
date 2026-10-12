---
id: KB-E0DCEFC0
subject: On /compare pinning or unpinning any row that is not already first drops keyboard focus to body
plane: experiential
question: On /compare, where does keyboard focus go after pressing Enter on a row's Pin / Unpin button?
status: active
appliesTo:
  - axis: auth
    value: guest
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /compare
evidence:
  - method: observation
    deployment: vcst_qa
    conditions: theme=2.60.0-pr-2511; browser=edge; viewport=1280x900
    at: 2026-10-12T17:22:27.533Z
    by: session:7b1a1a36
    who: Lenajava1
---
Guest, desktop 1280, Edge, 3 products in one category tab. Tab to the Pin button of a row that is not first (e.g. SKU or a catalog property row) and press Enter: the row moves to the top as pinned and document.activeElement is <body> both immediately and 500 ms later (3/3 attempts). Unpinning the same row (it moves back down) also drops focus to body (3/3). Control: pinning or unpinning the row that is already first (Price per unit) does NOT move it and focus stays on the button, now labelled Unpin / Pin. The next Tab after the loss lands on the Pin button of the row that followed the moved row's old position, so the browser keeps its navigation start point but the focus indicator and screen-reader context are gone. Rows are rendered from visibleRows = [...pinnedRows, ...rest] keyed by row key, so a pin reorders the DOM and the moved row, which holds focus, is detached and re-inserted. No focus restore follows a pin, unlike Remove (focus moves to the header row). Same on a PR theme build (2.60.0-pr-2518).
