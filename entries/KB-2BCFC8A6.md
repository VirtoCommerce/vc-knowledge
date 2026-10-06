---
id: KB-2BCFC8A6
subject: Quote Approve converts to an order in one step; no Accepted status
plane: experiential
question: What quote status does the storefront need to offer Accept/Approve, and is there an Accepted state before conversion to order?
questions:
  - text: When does the storefront show Approve and Decline buttons on a quote?
  - text: Mutation.approveQuoteRequest Proposal sent status converts quote to order Ordered
  - text: Is there an Accepted quote status before the quote becomes an order?
  - text: Does approving a quote place the order immediately or require a separate checkout?
  - text: Which Quotes.Status values are allowed and what does declineQuoteRequest set the status to?
concepts:
  - id: quote
  - id: customer-order
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.approveQuoteRequest
  - coordinate: Mutation.declineQuoteRequest
  - coordinate: /account/quotes
evidence:
  - method: observation
    deployment: vcst
    at: 2026-10-05T13:36:12.324Z
    by: session:51fcfb97
    who: kutasinaelena
---
The storefront quote detail page shows Approve and Decline only when quote.status === 'Proposal sent' and renders the raw status code as its label (vc-frontend view-quote.vue, settings_data.json quote statuses). The approveQuoteRequest mutation (ApproveQuoteCommandHandler) accepts only 'Proposal sent', then converts the quote to a cart, places the customer order and sets the quote to 'Ordered' in the same call; declineQuoteRequest sets 'Declined'. There is no intermediate 'Accepted' status and no separate checkout step for a quote. On vcst the Quotes.Status allowed values are Processing, On hold, New, Declined, Proposal sent, Ordered; Quotes.DefaultStatus is Draft.
