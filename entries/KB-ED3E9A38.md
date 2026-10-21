---
id: KB-ED3E9A38
subject: On /printers the first SearchProducts page (first=16) returns 13 items with totalCount 21, the next (after=16) returns 8 with totalCount 24, so the grid shows 21 cards under a "24 results" header
plane: experiential
question: Does Query.products return a full page and a consistent totalCount on every infinite-scroll page of a category?
questions:
  - text: Why does the category header say 24 results but only 21 cards load before 'You have reached the end of the list'?
  - text: Why is totalCount different between the first and second SearchProducts page?
status: active
appliesTo:
  - axis: element
    value: category-pagination
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.products
evidence:
  - method: observation
    deployment: vcst_qa
    conditions: theme=2.61.0-pr-2570-5cc5-5cc519ff; role=guest; store=B2B-store
    defect: suspected
    at: 2026-10-21T13:29:29.124Z
    by: session:facb37b1
    who: Lenajava1
---
Guest, B2B-store, /printers with the default 'Show in stock' (filter inStock_variations:true). SearchProducts after=0 first=16 withFacets=true returned 13 items and totalCount 21; the next request after=16 first=16 withFacets=false returned 8 items and totalCount 24. Paging resumes at offset 16 although only 13 came back, and the header switches to "24 results" while 21 cards render before "You have reached the end of the list". Reproduced 4 times identically on two theme builds (2.61.0-pr-2570 and 2.61.0-pr-2594) against the same backend, so it is backend behaviour. Suggests the page is post-filtered after search (short page) and that totalCount depends on withFacets; 3 products are never reachable by scrolling.
