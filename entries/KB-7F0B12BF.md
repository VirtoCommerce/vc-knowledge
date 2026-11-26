---
id: KB-7F0B12BF
subject: POST /api/payment/search pages saved methods first, then the unsaved ones, with one totalCount (saved + unsaved) on every page; each code appears once across pages
plane: experiential
question: How does POST /api/payment/search page through saved and unsaved payment methods, and is totalCount the same on every page?
questions:
  - text: Does the payment method search return a saved method again as a new instance on a later page?
  - text: What totalCount does POST /api/payment/search return with take=0?
  - text: In what order are payment methods returned when paging the payment method search?
status: active
appliesTo:
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/payment/search
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 1857bc5940dff4ff
    conditions: platform=3.1101.0; module:VirtoCommerce.Payment=3.1014.0-pr-83-9661; store=B2B-store
    at: 2026-11-26T14:22:58.646Z
    by: session:ede6a633
    who: Lenajava1
---
On vcptcore with Payment 3.1014.0-pr-83-9661 (the VCST-6930 fix), POST /api/payment/search {storeId:B2B-store, skip, take} was paged with take=2 and take=1 until an empty page, plus take=0, 20, 100 and skip=50, three times. B2B-store holds 7 saved methods and 1 unsaved registered method (OZTest2). Every page returned totalCount 8 (7 with isActive:true), take=0 returned totalCount 8 and no rows, skip beyond the total returned an empty page with the same total, and each code appeared exactly once across the pages; no saved method came back as an id-less new instance. The saved methods fill the first pages, the unsaved ones follow, and each page is sorted by code inside itself, so the order across several pages differs from a single take=100 page (which is fully sorted by code): with take=1 OZTest2 comes last, after SkyflowPaymentMethod. The three runs returned identical pages.
