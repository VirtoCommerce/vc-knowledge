---
id: KB-D47B8DF1
subject: Quote expiration date is neither shown nor enforced on the storefront; approving an expired or out-of-stock quote places the order
plane: experiential
question: Does the storefront show a quote's expiration date or block Approve on an expired quote or a quote with an out-of-stock line?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
anchors:
  - coordinate: /account/quotes/{id}
  - coordinate: Mutation.approveQuoteRequest
evidence:
  - method: observation
    deployment: vcst_qa
    build: 7ec352df61232ca8
    conditions: theme=2.60.0-alpha.2579; module:VirtoCommerce.Quote=3.1005.0
    at: 2026-10-15T13:24:11.120Z
    by: session:10db9d7d
    who: Lenajava1
---
On vcst (theme 2.60.0-alpha.2579) quotes in Proposal sent with expirationDate in the past or future both show status 'Proposal sent' in /account/quotes and on the detail page; the 'Quote data' block lists only Created and Status, never the expiration date, and there is no 'Expired' status. Approve on a quote whose expiration date had passed placed a New order with no error. Approve on a quote whose line was a permanently out-of-stock product placed an order containing that line with no warning. vc-module-quote ApproveQuoteCommandHandler checks only Status == 'Proposal sent' before ConvertToCartWithTax + PlaceCustomerOrderFromCartAsync, with no expiration or inventory check.
