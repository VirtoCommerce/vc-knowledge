---
id: KB-AA7CCB07
subject: "Draft quote request: Save changes after a line quantity edit keeps catalog prices"
plane: experiential
question: Does saving a quantity edit on a draft quote request (RFQ) zero the line prices?
questions:
  - text: Does changing the quantity in a draft request for quote reset the prices to zero?
  - text: Mutation.changeQuoteItemQuantity Draft quote Save changes listPrice
  - text: Which mutations does Save changes send on a draft quote request edit page?
  - text: Why is a gift line at 0.00 in a quote created from the cart?
  - text: Do quote totals recompute after a quantity change on a draft RFQ?
concepts:
  - id: quote
  - id: price
  - id: totals
status: active
appliesTo:
  - axis: module
    value: virtocommerce.quote
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.changeQuoteItemQuantity
  - coordinate: Query.quote
  - coordinate: /account/quotes
evidence:
  - method: observation
    deployment: vcst
    at: 2026-10-03T04:54:06.106Z
    by: session:51fcfb97
    who: kutasinaelena
---
On a Draft quote request created from the cart, the storefront edit page's Save changes sends ChangeQuoteItemQuantity (quoteId, lineItemId, quantity) per edited line, plus ChangeQuoteComment / UpdateQuoteAttachments / address updates when those changed, then refetches GetQuote. The refetched quote keeps each line's listPrice and selectedTierPrice (tier quantity follows the new quantity), imageUrl and product, and totals recompute (e.g. 2 to 5 units at 59.99 gives 299.95). Not reproduced: prices dropping to 0.00 on save, across 3 saves and 2 B2B buyers. An auto-added free gift line enters the quote at 0.00 by design. A zero-price state seen once on a quote AFTER Submit (status Processing) was not exercised here.
