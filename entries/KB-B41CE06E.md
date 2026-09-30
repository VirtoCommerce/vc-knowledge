---
id: KB-B41CE06E
subject: "Storefront /search?barcode= with a repeated barcode param or an extra q: first barcode wins, q is ignored, barcode lookups never enter search history"
plane: experiential
question: What does /search?barcode= do when the URL repeats barcode, also carries q, and does a barcode lookup add to the search history dropdown?
status: active
appliesTo:
  - axis: feature
    value: search-history
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /search?barcode=&barcode=
  - coordinate: /search?barcode=&q=
evidence:
  - method: observation
    deployment: vcst
    at: 2026-09-28T18:22:15.665Z
    by: session:p56308
    who: Lenajava1
  - method: observation
    deployment: vcst
    at: 2026-09-30T15:34:48.677Z
    by: session:134990b5
    who: Lenajava1
    note: "Re-observed on a later build: /search?barcode=<single-hit>&q=<term> still opened the PDP (header box empty there). /search?barcode=&barcode=<shared gtin>&q=<t> takes the FIRST value, which is empty, so the page ran a keyword search for <t> (products query=<t>, no barcode term) and the header box showed <t>."
---
Observed on a vc-frontend build carrying the barcode scanner search setup (theme PR 2501 over x-catalog PR 113 and catalog PR 909), store barcode fields configured. /search?barcode=<A>&barcode=<B> does not crash and logs no console error: the first value wins (a shared-GTIN A listed its 3 products even though B alone opens one product). /search?barcode=<single-hit value>&q=<unrelated term> still opens the product page, so the keyword is suppressed while a barcode is present; on a 0-hit barcode with q, Reset search navigates straight to /catalog with both barcode and q cleared, and Back returns to the barcode URL (one history step). Barcode lookups, whether on /search or on a category route, add nothing to the search history shown in the search dropdown for a signed-in buyer, while a keyword submitted from the search bar does appear there right away.
