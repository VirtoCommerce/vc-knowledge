---
id: KB-2F7FEBE2
subject: Quote approval places the order without any cart validation, so an out-of-stock tracked line is ordered and its stock goes negative, although checkout refuses the same product
plane: experiential
question: Does Mutation.approveQuoteRequest run the same cart validation (inventory, product availability) as Mutation.createOrderFromCart before placing the order?
questions:
  - text: Can a buyer order an out-of-stock product by approving a quote?
  - text: Does approving a quote decrement inventory below zero?
  - text: Why does createOrderFromCart reject an out-of-stock product that a quote approval accepts?
status: active
appliesTo:
  - axis: module
    value: quote
  - axis: store
    value: b2b-store
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.approveQuoteRequest
  - coordinate: Mutation.createOrderFromCart
  - coordinate: Mutation.addItem
evidence:
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    conditions: platform=3.1090.0; module:VirtoCommerce.Quote=3.1005.0; module:VirtoCommerce.Orders=3.1019.0; module:VirtoCommerce.XOrder=3.1017.0; module:VirtoCommerce.XCart=3.1042.0; module:VirtoCommerce.Inventory=3.1011.0
    defect: suspected
    at: 2026-10-29T09:05:31.290Z
    by: session:ce3cfc2a
    who: Aleksandra-Mitricheva
---
On vcst_qa a quote in Proposal sent with a tracked product (stock 0 in the store main FC, inventory Enabled, no backorder, no preorder) plus a second line was approved from the storefront quote page. approveQuoteRequest returned 200 with an orderId and no errors, the New order held the out-of-stock line, and the product's inStockQuantity went from 0 to -1. With the same buyer, normal checkout refused the product: addItem returned PRODUCT_FFC_QTY ('Available quantity is 0') and createOrderFromCart returned 'The cart has validation errors'. Source at the deployed tags explains the difference: ApproveQuoteCommandHandler (Quote 3.1005.0) calls QuoteConverter.ConvertToCartWithTax, which maps lines and taxes only, and then Orders CustomerOrderBuilder.PlaceCustomerOrderFromCartAsync (3.1019.0), which converts and saves with no validation. CreateOrderFromCartCommandHandler (XOrder 3.1017.0) runs cartAggregate.ValidateAsync("*") first. The order save still triggers the inventory adjustment, so stock goes negative.
