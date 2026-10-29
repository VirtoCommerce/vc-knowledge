---
id: KB-B27E3CAF
subject: Save changes on a draft quote request does not reprice lines; a $0.00 RFQ total comes from zero line prices stored at createQuoteFromCart, intermittently
plane: experiential
question: Why do some quote requests created from the cart show Total $0.00 in /account/quotes while others show the calculated total, and does clicking Save changes before Submit fix it?
questions:
  - text: Does Save changes on the quote request edit page recalculate quote prices or totals?
  - text: Are quote request totals stored or calculated on read?
  - text: Why is a submitted RFQ total 0.00 for a buyer?
  - text: Is createQuoteFromCart pricing deterministic?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.quote
  - axis: module
    value: virtocommerce.xcart
  - axis: surface
    value: rest
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.createQuoteFromCart
  - coordinate: Mutation.changeQuoteComment
  - coordinate: Mutation.updateQuoteAddresses
  - coordinate: /account/quotes
  - coordinate: GET /api/quote/requests/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    conditions: platform=3.1090.0; theme=2.61.0-alpha.2605; module:VirtoCommerce.Quote=3.1005.0; module:VirtoCommerce.XCart=3.1042.0; module:VirtoCommerce.Cart=3.1013.0
    defect: suspected
    at: 2026-10-29T09:19:46.797Z
    by: session:ce3cfc2a
    who: Aleksandra-Mitricheva
    note: "distinct: KB-303CC514 records that persisted cart prices flow into the quote. This entry records that the outcome is intermittent within one session, that Save changes (comment/address mutations) neither causes nor fixes the 0.00 lines, and that quote totals are computed on read. KB-AA7CCB07 covers only quantity edits on an already-priced quote."
---
On vcst (2026-10-29) the same buyer converted the same two priced products (49.99, 79.99) from the cart four times. Two quotes were stored with listPrice/salePrice/selectedTierPrice/proposalPrices 0 on every non-gift line right at Mutation.createQuoteFromCart. Two were stored with catalog prices. Admin GET /api/quote/requests/{id} returned totals 0 for the zero ones, so the manager sees a $0 quote as well. Quote totals are not persisted: they are recalculated on every read from the stored line prices, so a $0.00 list total means 0.00 lines. Save changes on /account/quotes/{id}/edit sends only ChangeQuoteComment and UpdateQuoteAddresses (then GetQuote). It did not reprice a zero quote and did not zero a priced one. A quote that showed $120 after Save was already priced at creation. In source (Quote 3.1005.0), CreateQuoteFromCartCommandHandler converts the raw stored cart instance. That instance gets priced only if xCart builds a fresh aggregate around it during validation (aggregate cache miss), which explains the intermittency.
