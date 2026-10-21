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
  - method: observation
    deployment: vcst_qa
    build: f6080d062139e9f0
    conditions: theme=3.1.0-pr-11-d67b-d67b0e0a; browser=edge-1920; role=guest
    at: 2026-10-20T15:56:34.549Z
    by: session:d992339e
    bodyHash: fb66e98d536f
    who: Lenajava1
    note: "Empty-grid half re-observed 3/3 as guest on Edge: /printers?q=<nonsense> and /accessories?q=<nonsense> each pushed view_item_list category_<slug>_page_1 with items [] / items_count 0 (between search items_count 0 and view_search_results results_count 0); /search?q=<nonsense> pushed view_item_list category_undefined_page_1 with items []. API SearchProducts returned totalCount 0, so the push is client-side (category.vue fetchProducts calls viewItemList without a length check). Neither VirtoOZ ga-events nor Google's view_item_list reference addresses an empty items array, so whether this is a defect is a product question; the suspected marker is left as is."
  - method: observation
    deployment: vcst_qa
    conditions: theme=2.61.0-pr-2570-5cc5-5cc519ff; browser=chromium-1920; role=guest; store=B2B-store
    at: 2026-10-21T13:28:54.337Z
    by: session:facb37b1
    bodyHash: fb66e98d536f
    defect: VCST-6453
    who: Lenajava1
    note: "Re-observed on vc-frontend 2.61.0-pr-2570 (not vc-frontend-next): /printers page_1 13 items, page_2 21 items = the 13 page-1 ids + the 8 the after=16 SearchProducts page returned; /accessories page_1/2/3 = 16/32/48; /search?q=laser page_2 = 21 (16 page-1 + 5). On the vc-frontend PR #2594 build (2.61.0-pr-2594-5ab1-5ab19015) against the same backend, every page_N event carried exactly that page's SearchProducts items (printers 13/8, accessories 16/16/16, facet run 16x5+3, search 16/5), so the defect is ticketed as VCST-6453 and fixed in that PR build (not yet released)."
---
B2B member, Firefox 1920, /printers. Cold load: view_item_list category_printers_page_1, 15 items. "Search within Printers" with a nonsense term (0 results, "There are no results found"): another view_item_list category_printers_page_1 with items [] and items_count 0. Reset filters: a third view_item_list category_printers_page_1 with the same 15 items (same first item_id). Pressing End to load the next page: view_item_list category_printers_page_2 with 23 items, of which 15 are exactly the page-1 item_ids (index 0,1,2… restart at 0), i.e. the event carries the whole accumulated grid rather than the 8 newly shown cards, so page-1 impressions are reported twice. On /products-with-options/cfg-parents a "Customize" click pushed exactly one select_item with the category list id.
