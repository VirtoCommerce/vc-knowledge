---
id: KB-0B4A32AA
subject: /cart recently browsed view_item_list is re-sent with the rendered subset whenever a resize changes how many cards the short grid shows
plane: experiential
question: Does the /cart recently browsed block re-send view_item_list when the viewport is resized?
status: active
appliesTo:
  - axis: element
    value: google-analytics
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
evidence:
  - method: observation
    deployment: vcst_qa
    conditions: theme=2.60.0-pr-2543-0127-01276486; browser=firefox; role=guest
    at: 2026-10-13T17:09:10.470Z
    by: session:aa76d86f
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: ca098b1f8643fb95
    conditions: theme=2.60.0-pr-2543-fdab-fdabd62d; browser=chromium; role=signed-in
    at: 2026-10-14T09:27:27.615Z
    by: session:b0e90290
    bodyHash: 1ef4ca0aea85
    who: Lenajava1
    contradicts: true
    contested: theme 2.60.0-pr-2543-fdab-fdabd62d vs 2.60.0-pr-2543-0127-01276486; browser chromium vs firefox; role signed-in vs guest
    note: "On build theme=2.60.0-pr-2543-fdab-fdabd62d (fix fdabd62), Chrome, signed in: /cart recently browsed sent one view_item_list recently_browsed_products with 6 items (= the 6 the recentlyBrowsed query returned) on cold loads at 390, 800 and 1920; one page view resized 1920->800->1920->390->1920 kept it at 1. A cart line quantity change and Saved-for-later Show more added no view_item_list."
---
Guest on /cart with 6 recently browsed products (markup-tracked list recently_browsed_products), Firefox. Cold load at 1920 pushed one view_item_list with 6 items. Resizing on the same page view: 1300 pushed nothing (still 6 cards); 1100 pushed 5 items; 800 pushed 4; 700 pushed nothing (still 4); back to 1920 pushed 6. Every push matches the number of product cards rendered by the short products grid at that width (2xl/xl 6, lg 5, md/sm 4), so the event count per page view grows with each count-changing breakpoint crossing. The pre-markup component sent the whole products array once per array change.
