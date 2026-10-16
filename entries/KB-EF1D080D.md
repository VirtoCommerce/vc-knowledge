---
id: KB-EF1D080D
subject: An order placed by approving a quote keeps the list price as price and the proposal as placedPrice, with the difference in discountAmount
plane: experiential
question: What prices does the CustomerOrder created by approveQuoteRequest carry on its lines?
status: active
appliesTo:
  - axis: surface
    value: rest
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.approveQuoteRequest
  - coordinate: GET /api/order/customerOrders/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: e00c89f4da0e9c98
    conditions: platform=3.1083.0; module:VirtoCommerce.Quote=3.1005.0; module:VirtoCommerce.Orders=3.1018.0
    at: 2026-10-15T11:32:16.095Z
    by: session:10db9d7d
    who: Lenajava1
---
A quote line with listPrice 189 and an admin proposal (selectedTierPrice) of 170.10 was approved through approveQuoteRequest; the resulting order line had price 189, placedPrice 170.10, discountAmount 18.90 and extendedPrice 170.10 (a qty-2 line: price 349, placedPrice 314.10, extendedPrice 628.20); the order's discountTotal was the sum of the line differences and its discounts collection was empty. So the quoted price is honoured, but an assertion that reads the order line's price field sees the catalog price, not the quote price; read placedPrice or extendedPrice instead.
