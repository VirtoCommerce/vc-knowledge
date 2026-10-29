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
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    conditions: theme=2.61.0-alpha.2605; module:VirtoCommerce.Quote=3.1005.0; platform=3.1090.0; setting:Quotes.Status=Processing,On hold,New,Declined,Proposal sent,Ordered
    at: 2026-10-29T08:58:33.436Z
    by: session:ce3cfc2a
    bodyHash: 3338a39b7f67
    who: Aleksandra-Mitricheva
    note: "Re-observed for the display half only (Approve not clicked): Proposal-sent quotes with expirationDate 3 days past and 30 days ahead both render status 'Proposal sent' in /account/quotes and on /account/quotes/{id}; Quote data lists only Created and Status; Approve/Decline stay enabled on the expired one. xAPI QuoteType does expose expirationDate (returned correctly for both), but vc-frontend dev GetQuote/GetQuotes queries never select it, and quote-status.vue only maps the backend status code via theme settings quote_statuses; Quotes.Status has no Expired value and nothing in vc-module-quote moves an expired quote to another status."
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    conditions: platform=3.1090.0; theme=2.61.0-alpha.2605; module:VirtoCommerce.Quote=3.1005.0
    at: 2026-10-29T08:58:36.118Z
    by: session:ce3cfc2a
    bodyHash: 3338a39b7f67
    who: Aleksandra-Mitricheva
    note: "Expired half re-observed 2026-10-29 on theme 2.61.0-alpha.2605: detail page of a 'Proposal sent' quote 3 days past expirationDate shows Approve and Decline enabled and no expiration date; Approve sent ApproveQuoteRequest, got an orderId and redirected to the new order. Out-of-stock half not re-checked."
---
On vcst (theme 2.60.0-alpha.2579) quotes in Proposal sent with expirationDate in the past or future both show status 'Proposal sent' in /account/quotes and on the detail page; the 'Quote data' block lists only Created and Status, never the expiration date, and there is no 'Expired' status. Approve on a quote whose expiration date had passed placed a New order with no error. Approve on a quote whose line was a permanently out-of-stock product placed an order containing that line with no warning. vc-module-quote ApproveQuoteCommandHandler checks only Status == 'Proposal sent' before ConvertToCartWithTax + PlaceCustomerOrderFromCartAsync, with no expiration or inventory check.
