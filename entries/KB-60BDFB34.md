---
id: KB-60BDFB34
subject: On theme 2.61.0-pr-2598-9b6d, the GA4 add_to_wishlist item carries currency equal to the event currency when a product is added to a list from a category card
plane: experiential
question: Does the GA4 add_to_wishlist item carry a currency, and does it match the event currency?
questions:
  - text: What fields does the add_to_wishlist event item carry after adding a product to a list from a category card?
  - text: Is currency set on items of the add_to_wishlist GA4 event?
status: active
appliesTo:
  - axis: integration
    value: ga4
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/lists/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    conditions: theme=2.61.0-pr-2598-9b6d-9b6db3d9; store=B2B-store; browser=chromium; role=signed-in B2B user; currency=USD
    at: 2026-10-22T15:37:55.357Z
    by: session:b1c1bce0
    who: Lenajava1
---
Signed in, Chromium 1920, theme 2.61.0-pr-2598-9b6d-9b6db3d9 against the vcst_qa API, store B2B-store, currency USD. On /printers I clicked a category card's 'Add to list', ticked a list in the 'Please select list' dialog and saved. window.dataLayer got exactly one gtag event, add_to_wishlist, with event currency USD and value 20 (the item price). Its single item had currency USD, price 20, discount 0, affiliation, item_category 1-4, and no index or item_list_id. So the item currency equals the event currency. The pre-fix builds (pr-2570) carried no currency on that item (KB-9D50E60B).
