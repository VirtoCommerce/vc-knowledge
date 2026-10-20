---
id: KB-CCFCAEEB
subject: On a category page the infinite-scroll view_item_list for page 2 repeats every page-1 item, and a filter that empties the grid pushes a view_item_list with items []
plane: experiential
question: Which items does a category page's view_item_list carry after infinite scroll loads page 2, and what is pushed when a search-within-category returns nothing?
questions:
  - text: Does category_<slug>_page_2 view_item_list include page 1 products again?
  - text: Is view_item_list sent with an empty items array on a category with no results?
  - text: Is a second view_item_list sent after Reset filters restores the same products?
status: active
appliesTo:
  - axis: element
    value: google-analytics
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /products-with-options/cfg-parents
  - coordinate: window.dataLayer
evidence:
  - method: observation
    deployment: vcst_qa
    build: f6080d062139e9f0
    conditions: theme=3.1.0-pr-11-d67b-d67b0e0a (vc-frontend-next, merges vc-frontend 2.60.0); setting:GoogleAnalytics4.EnableTracking=true; browser=firefox-1920; role=B2B org member
    defect: suspected
    at: 2026-10-20T13:59:27.422Z
    by: session:d992339e
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: f6080d062139e9f0
    conditions: theme=3.1.0-pr-11-d67b-d67b0e0a; browser=firefox-1920; role=guest
    at: 2026-10-20T15:31:31.533Z
    by: session:d992339e
    bodyHash: fb66e98d536f
    defect: suspected
    who: Lenajava1
    note: "Re-observed 3/3 as a GUEST (not role-dependent). /printers: page_1 15 items, page_2 21 items = the same 15 page-1 ids (index 0-14) + the 6 items the page-2 SearchProducts (first 18, after 18) returned. /accessories: page_1 18, page_2 36, page_3 54, each event's items prefix equal to the previous event. Payload is built client-side from the accumulated grid; the API page is correct."
---
B2B member, Firefox 1920, /printers. Cold load: view_item_list category_printers_page_1, 15 items. "Search within Printers" with a nonsense term (0 results, "There are no results found"): another view_item_list category_printers_page_1 with items [] and items_count 0. Reset filters: a third view_item_list category_printers_page_1 with the same 15 items (same first item_id). Pressing End to load the next page: view_item_list category_printers_page_2 with 23 items, of which 15 are exactly the page-1 item_ids (index 0,1,2… restart at 0), i.e. the event carries the whole accumulated grid rather than the 8 newly shown cards, so page-1 impressions are reported twice. On /products-with-options/cfg-parents a "Customize" click pushed exactly one select_item with the category list id.
