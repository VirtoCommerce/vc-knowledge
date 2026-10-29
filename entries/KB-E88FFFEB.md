---
id: KB-E88FFFEB
subject: Cart page sends one recently_browsed_products view_item_list per load, not on a quantity change; saved-for-later Show more sends nothing and an SFL card click sends select_item under the recently_browsed_products list id
plane: experiential
question: What GA4 list events does /cart push for the recently browsed and saved-for-later blocks?
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
    conditions: theme=2.60.0-pr-2543-0127-01276486; browser=msedge; role=signed-in customer
    at: 2026-10-13T13:58:43.265Z
    by: session:aa76d86f
    who: Lenajava1
    build: 942f354b4df8bb56
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    conditions: theme=2.60.0-pr-2543-0127-01276486; platform=3.1078.0; browser=chromium; role=signed-in customer
    at: 2026-10-13T17:17:11.899Z
    by: session:aa76d86f
    bodyHash: c59e429335ed
    who: Lenajava1
    note: "Chrome regression run REG-2026-10-13-1643 (GA-038/047): after seed, 1 RB view_item_list with the 6 seeded codes; +1 quantity -> update_cart_item, no RB re-send; Show more -> nothing; SFL title click (7PLSH835) -> 1 select_item item_list_id recently_browsed_products, item_list_name Recently browsed."
  - method: observation
    deployment: vcst_qa
    conditions: theme=2.60.0-pr-2543-0127-01276486; browser=firefox; role=anonymous guest
    at: 2026-10-13T17:24:02.772Z
    by: session:aa76d86f
    bodyHash: c59e429335ed
    who: Lenajava1
    note: "Guest on firefox: a cart line quantity change pushed update_cart_item and add_shipping_info (cart had a saved address + Air method). The add_shipping_info re-send happens only on the FIRST quantity change after page load; a second change pushes update_cart_item only."
  - method: observation
    deployment: vcst_qa
    build: ca098b1f8643fb95
    conditions: theme=2.60.0-pr-2543-fdab-fdabd62d; browser=chromium; role=signed-in customer
    at: 2026-10-14T09:27:52.780Z
    by: session:b0e90290
    bodyHash: c59e429335ed
    who: Lenajava1
    note: "Held on fix build fdabd62: 1 RB view_item_list (6 = recentlyBrowsed response), qty +1 -> update_cart_item + add_shipping_info, no RB re-send; Show more -> nothing; SFL title click (152078) -> 1 select_item item_list_id recently_browsed_products / 'Recently browsed'. cart-for-later.vue hardcodes that id on dev too (pre-existing). EXCEPT the last sentence: on fdabd62 resize no longer re-sends RB (1920->800->1920->390->1920 stayed at 1, 6 items at every width) - see the dispute on KB-0B4A32AA."
  - method: observation
    deployment: vcst_qa
    build: ca098b1f8643fb95
    conditions: theme=2.60.0-pr-2543-fdab-fdabd62d; platform=3.1079.0; browser=msedge; role=signed-in customer
    at: 2026-10-14T09:43:52.332Z
    by: session:b0e90290
    bodyHash: c59e429335ed
    who: Lenajava1
    note: "Re-observed on fdabd62 build: 1 recently_browsed_products view_item_list per cart load (6 items), quantity +1 pushed update_cart_item with no RB re-send, saved-for-later Show more pushed nothing, SFL title click pushed 1 select_item under item_list_id recently_browsed_products."
  - method: observation
    deployment: vcst_qa
    build: ca098b1f8643fb95
    conditions: theme=2.60.0-pr-2543-fdab-fdabd62d; browser=chromium; role=signed-in customer
    at: 2026-10-14T10:01:14.259Z
    by: session:b0e90290
    bodyHash: c59e429335ed
    who: Lenajava1
    note: "Re-observed on the newer PR build: after the cart seed, one RB view_item_list with the 6 seeded codes; +1 quantity pushed add_shipping_info + update_cart_item, no RB re-send; SFL Show more 6->8 cards pushed nothing; SFL title click (7PLSH835) opened a new tab and pushed one select_item with item_list_id recently_browsed_products, item_list_name Recently browsed. SFL cards carry no data-vc-track markup."
  - method: observation
    deployment: vcst_qa
    build: f6080d062139e9f0
    conditions: theme=3.1.0-pr-11-d67b-d67b0e0a; browser=chromium; role=signed-in customer
    at: 2026-10-20T14:15:25.354Z
    by: session:d992339e
    bodyHash: c59e429335ed
    who: Lenajava1
    note: "Held on theme 3.1.0-pr-11-d67b, B2B org admin: one recently_browsed_products view_item_list per /cart load. An RB card title click (Doritos) pushed one select_item with the same id and name, and the link opened in a new tab."
  - method: observation
    deployment: vcst_qa
    build: 8d4bda9222c60036
    conditions: role=signed-in customer; browser=chromium; theme=vcst_qa deployed storefront (pre PR 2598)
    at: 2026-10-26T10:55:06.089Z
    by: session:1c2d4240
    bodyHash: c59e429335ed
    who: Lenajava1
    note: "Re-observed 2026-10-26 on the deployed vcst_qa storefront as the pre-fix baseline: cold /cart load pushed RB view_item_list and no saved-for-later impression; an SFL title click (Barths 151682) pushed select_item item_list_id recently_browsed_products with no index, no currency and no item_category. Unmerged PR 2598 build 608d changes this (separate entry)."
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    conditions: theme=2.61.0-alpha.2605; browser=firefox; role=signed-in customer
    at: 2026-10-29T00:34:35.571Z
    by: session:ce3cfc2a
    bodyHash: c59e429335ed
    who: Aleksandra-Mitricheva
    contradicts: true
    contested: theme 2.61.0-alpha.2605 vs 3.1.0-pr-11-d67b-d67b0e0a; browser firefox vs chromium; virtocommerce.cart 3.1013.0 vs 3.1012.0; virtocommerce.pages 3.1011.0 vs 3.1010.0; virtocommerce.profileexperienceapimodule 3.1022.0 vs 3.1021.0; virtocommerce.xapi 3.1028.0 vs 3.1027.0; …
    note: "Theme Ver. 2.61.0-alpha.2605 (2026-10-29), signed-in, after seed:ga4-lists:cart: /cart cold load pushed view_item_list recently_browsed_products (6 seeded codes) AND a separate view_item_list item_list_id saved_for_later ('Saved for later', 8 items). SFL title click (Fosters 152078, opens new tab) pushed one select_item with item_list_id saved_for_later — not recently_browsed_products. RB not re-sent on qty +1 or on SFL Show more (still holds)."
---
Signed in on /cart with 6 recently browsed and 8 saved-for-later products (disjoint): cold load pushed one view_item_list recently_browsed_products ('Recently browsed') with exactly the 6 RB products and none of the SFL products. Saved-for-later Show more (6 -> 8 cards) pushed no event. A cart line quantity +1 pushed update_cart_item and add_shipping_info but no RB re-send. An SFL card title click pushed one select_item with item_list_id recently_browsed_products (the SFL block reuses that id, manual call); an RB card title click pushed one select_item for that product. Cart-page product links open in a new tab. The RB list is re-sent on resize when the grid's rendered count changes (1920->390->800->1920 gave 6, 4, 6).
