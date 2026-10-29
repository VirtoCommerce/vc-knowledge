---
id: KB-0CB8AB36
subject: Unchecking the auto-promotion gift on /cart (RejectGiftItems) keeps the gift line in the stored cart with isRejected true, and createQuoteFromCart leaves it out of the quote
plane: experiential
question: "/cart page \"+ Add a gift\" checkbox: does unchecking it remove the isGift line from the stored cart (Mutation.rejectGiftItems)?"
status: active
appliesTo:
  - axis: promotion
    value: auto gift (no coupon)
  - axis: store
    value: b2b-store
  - axis: surface
    value: rest
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.rejectGiftItems
  - coordinate: POST /api/carts/search
  - coordinate: Mutation.createQuoteFromCart
  - coordinate: /cart
evidence:
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    conditions: platform=3.1090.0; theme=2.61.0-alpha.2605; module:VirtoCommerce.XCart=3.1042.0; module:VirtoCommerce.Quote=3.1005.0
    at: 2026-10-29T10:04:05.767Z
    by: session:ce3cfc2a
    who: Aleksandra-Mitricheva
---
On vcst B2B-store (2026-10-29) the "+ Add a gift" panel shows the Auto Gift (no coupon) printer pre-checked. Unchecking it sends Mutation RejectGiftItems with the gift lineItemId; the response cart has gifts []. POST /api/carts/search afterwards still lists the gift line (isGift true, price 0) but with isRejected true. Quotes created afterwards with createQuoteFromCart had no gift line (4 of 4), while quotes from carts with the gift checked carried it as a 0.00 line.
