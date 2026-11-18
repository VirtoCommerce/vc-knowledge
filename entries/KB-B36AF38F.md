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
  - method: observation
    deployment: vcst_qa
    build: 2ff67d68f7987d9e
    conditions: theme=2.63.0-pr-2617-a143-a1435e3f; setting:design=classic; browser=chromium; viewport=1920x1080; role=organization member
    at: 2026-11-18T15:24:01.398Z
    by: session:8bd71225
    bodyHash: 8484886872a4
    who: Lenajava1
    note: "Chromium 1920: All orders (12 pages) page 1 shows 1-7, '...', 12, Previous disabled; current page and '...' are spans, the other pages are buttons. The 320px part was not re-checked."
  - method: observation
    deployment: vcst_qa
    build: 2ff67d68f7987d9e
    conditions: theme=2.63.0-pr-2617-a143-a1435e3f; setting:design=classic; browser=firefox-156; role=organization member
    at: 2026-11-18T15:24:05.830Z
    by: session:8bd71225
    bodyHash: 8484886872a4
    who: Lenajava1
    note: "Re-seen in Firefox 1280px, design classic: All orders gives 1 2 3 4 5 6 7 ... 12 on page 1; clicking 3 makes 3 current and scrolls the page to the top. After a page click the focused button is replaced by the current-page span, so document.activeElement becomes BODY; Firefox's next Tab lands on the page button after it (4), scrolling back to the pager. The header ship-to dropdown also keeps a hidden VcPagination (4 pages) in the DOM."
---
Signed in as an organization member on vcst_qa (theme 2.63.0-pr-2617-a143, design classic), Firefox 1920x1080: /account/orders with the 'My orders' toggle (8 orders) shows no pagination; clicking 'All orders' shows a 12-page pager: page 1 = pages 1-7, '...', 12, Previous disabled; clicking 6 = 1, '...', 4-8, '...', 12. The current page and the ellipsis are not buttons (the accessibility tree shows them as plain text; the other pages are buttons named by their digit; no aria-current, no nav landmark). With the dark toggle on and the viewport 320 px wide the pages row wraps ('12' on its own second line) and Previous/Next stack on a third row with their text labels - they do not switch to icon-only, because compact is a prop the caller sets.
