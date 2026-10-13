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
---
Signed in on /cart with 6 recently browsed and 8 saved-for-later products (disjoint): cold load pushed one view_item_list recently_browsed_products ('Recently browsed') with exactly the 6 RB products and none of the SFL products. Saved-for-later Show more (6 -> 8 cards) pushed no event. A cart line quantity +1 pushed update_cart_item and add_shipping_info but no RB re-send. An SFL card title click pushed one select_item with item_list_id recently_browsed_products (the SFL block reuses that id, manual call); an RB card title click pushed one select_item for that product. Cart-page product links open in a new tab. The RB list is re-sent on resize when the grid's rendered count changes (1920->390->800->1920 gave 6, 4, 6).
