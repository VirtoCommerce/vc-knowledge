---
id: KB-B36AF38F
subject: On /account/orders (All orders, 12 pages) VcPagination renders 1..7 ... 12 on page 1 and 1 ... 4 5 6 7 8 ... 12 on page 6; at 320 px the last page wraps to a second row and Prev/Next drop to a third row, still with text labels
plane: experiential
question: How does the storefront VcPagination on /account/orders lay out its page numbers and Prev/Next at desktop and at 320 px width?
status: active
appliesTo:
  - axis: component
    value: vcpagination
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/orders
evidence:
  - method: observation
    deployment: vcst_qa
    build: 2ff67d68f7987d9e
    conditions: theme=2.63.0-pr-2617-a143-a1435e3f; setting:design=classic; browser=firefox; mode=dark at 320px
    at: 2026-11-18T15:09:50.854Z
    by: session:8bd71225
    who: Lenajava1
---
Signed in as an organization member on vcst_qa (theme 2.63.0-pr-2617-a143, design classic), Firefox 1920x1080: /account/orders with the 'My orders' toggle (8 orders) shows no pagination; clicking 'All orders' shows a 12-page pager: page 1 = pages 1-7, '...', 12, Previous disabled; clicking 6 = 1, '...', 4-8, '...', 12. The current page and the ellipsis are not buttons (the accessibility tree shows them as plain text; the other pages are buttons named by their digit; no aria-current, no nav landmark). With the dark toggle on and the viewport 320 px wide the pages row wraps ('12' on its own second line) and Previous/Next stack on a third row with their text labels - they do not switch to icon-only, because compact is a prop the caller sets.
