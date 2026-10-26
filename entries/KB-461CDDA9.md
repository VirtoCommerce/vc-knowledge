---
id: KB-461CDDA9
subject: On theme 2.61.0-pr-2598-608d, a /cart line click pushes select_item with item_list_id 'cart' and item index = the line's position in the page-load view_cart; an in-page line removal does not re-index, a reload does
plane: experiential
question: What index does a /cart line's GA4 select_item carry, and does it change after a line is removed without reloading?
questions:
  - text: Does the cart page select_item use item_list_id cart?
  - text: Is the cart select_item index taken from view_cart or from the live cart?
  - text: After removing a cart line, what index do the remaining lines report on click?
status: active
appliesTo:
  - axis: integration
    value: ga4
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
evidence:
  - method: observation
    deployment: vcst_qa
    build: 8d4bda9222c60036
    conditions: theme=2.61.0-pr-2598-608d-608d6e2f; store=B2B-store; role=anonymous guest; browser=chromium
    at: 2026-10-26T10:40:33.084Z
    by: session:1c2d4240
    who: Lenajava1
    note: "distinct: KB-E88FFFEB covers the recently browsed and saved-for-later blocks on /cart; this fact is about the cart LINE ITEMS' select_item (list id 'cart') and how its index is derived from the page-load view_cart after an in-page removal."
---
Guest, Chromium 1920, theme 2.61.0-pr-2598-608d-608d6e2f (frontend-only local stack, API proxied to vcst_qa, store B2B-store). Cart with 3 lines; page-load view_cart items: Brother 564698896 index 0, Canon 566903892 index 1, HP 565567699 index 2 (no item_list_id at event level). Clicking the 2nd line's product link (opens a new tab) pushed in the cart tab one select_item with event params item_list_id 'cart' / item_list_name 'Cart' and one item with index 1, currency USD, item_list_id 'cart'. Removing the Brother line in-page (remove_from_cart pushed, no reload), then clicking Canon and HP pushed select_item index 1 and 2: the load-time view_cart positions, not the new visual positions 0 and 1. After a reload the new view_cart listed Canon 0 / HP 1 and a Canon click pushed index 0. view_cart value (6656.21) equalled the cart Total shown in Order summary.
