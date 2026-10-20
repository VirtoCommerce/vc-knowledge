---
id: KB-8A4AB65C
subject: On /compare at 390px Firefox 151 gives the compare table's tbody scroller a Tab stop and Arrow keys scroll it, although the tbody has no tabindex; tabbing to a clipped column's header controls also scrolls the body via scroll sync
plane: experiential
question: On /compare at mobile width, can a keyboard-only user reach and horizontally scroll the product columns of the compare table?
questions:
  - text: Is the /compare tbody.compare-table__scroll scroller keyboard-reachable in Firefox?
  - text: Does the axe scrollable-region-focusable finding on /compare block keyboard scrolling in a real browser?
  - text: How does a keyboard user bring the third compare column into view at 390px?
status: active
appliesTo:
  - axis: feature
    value: product-compare
  - axis: surface
    value: storefront-ui
  - axis: viewport
    value: mobile
anchors:
  - coordinate: /compare
evidence:
  - method: observation
    deployment: vcst_qa
    build: f6080d062139e9f0
    conditions: theme=3.1.0-pr-11-d67b-d67b0e0a; browser=firefox-151; viewport=390x844; role=guest
    at: 2026-10-20T15:52:50.697Z
    by: session:d992339e
    who: Lenajava1
    note: "distinct: KB-3CC64161 records the static axe result; this records what a real keyboard user can do in Firefox, which the axe rule does not measure."
---
Guest, Firefox 151 (Playwright), viewport 390x844, theme 3.1.0-pr-11-d67b, 3 laptops in one compare tab; the table is 448px wide in a 364px tbody scroller (overflow-x:auto, display:block). DOM: tbody.compare-table__scroll has no tabindex attribute (tabIndex property -1), no role, and none of its 11 focusable descendants (row pin buttons, info triggers) are rendered below md; thead.compare-table__header-row carries tabindex=-1. Keyboard walk from the page heading: Clear all, category tab, All, Differences, Clear category, then per column Remove from compare, product link, Add to cart (9 stops). Tabbing to the third column's Remove button scrolled the thead by 84px and the tbody followed (scroll sync), so the clipped column came into view. The next Tab focused the tbody rowgroup itself, with Firefox's default focus outline visible; ArrowLeft/ArrowRight then scrolled body and header together (column x 41 -> 63 -> 73). So in Firefox the axe scrollable-region-focusable finding (KB-3CC64161) does not stop a keyboard user from scrolling: the browser makes the scroller focusable itself. Browsers that do not make scrollers focusable (WebKit) were not tested; there only the header-control scroll-sync path would remain.
