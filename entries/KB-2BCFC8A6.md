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
    deployment: vcst_qa
    at: 2026-10-05T13:36:12.324Z
    by: session:51fcfb97
    who: kutasinaelena
    build: 59983c56933fe571
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    build: e00c89f4da0e9c98
    conditions: platform=3.1083.0; module:VirtoCommerce.Quote=3.1005.0; surface=xapi
    at: 2026-10-15T11:31:33.939Z
    by: session:10db9d7d
    bodyHash: 6c2eab92af70
    who: Lenajava1
    note: "2026-10-15: as the slot-1 B2B buyer via xAPI, approveQuoteRequest on an API-created 'Proposal sent' quote returned {id, orderId} and the quote read back 'Ordered' in the same call; a CustomerOrder (status New) was placed for the same customerId + organizationId. Quotes.Status allowed values unchanged (Processing, On hold, New, Declined, Proposal sent, Ordered)."
  - method: observation
    deployment: vcst_qa
    build: 7ec352df61232ca8
    at: 2026-10-15T13:23:19.760Z
    by: session:10db9d7d
    bodyHash: 6c2eab92af70
    who: Lenajava1
    note: "Theme 2.60.0-alpha.2579: six Proposal sent quotes approved from /account/quotes/{id}; each single click (no confirmation dialog) redirected to /account/orders/{id} with a New order and set the quote to Ordered."
  - method: observation
    deployment: vcst_qa
    build: f6080d062139e9f0
    conditions: theme=3.1.0-pr-11-d67b-d67b0e0a
    at: 2026-10-20T14:52:05.310Z
    by: session:d992339e
    bodyHash: 6c2eab92af70
    who: Lenajava1
    note: "Theme 3.1.0-pr-11-d67b: two Proposal-sent quotes (slot-1 buyer) approved from /account/quotes/{id}: only Decline/Approve offered (no Accept Quote / Convert to Order); single click, no confirmation, redirect to /account/orders/{id} with a New order whose total equals the quote total (quoted line prices); quote then reads Ordered."
---
The storefront quote detail page shows Approve and Decline only when quote.status === 'Proposal sent' and renders the raw status code as its label (vc-frontend view-quote.vue, settings_data.json quote statuses). The approveQuoteRequest mutation (ApproveQuoteCommandHandler) accepts only 'Proposal sent', then converts the quote to a cart, places the customer order and sets the quote to 'Ordered' in the same call; declineQuoteRequest sets 'Declined'. There is no intermediate 'Accepted' status and no separate checkout step for a quote. On vcst the Quotes.Status allowed values are Processing, On hold, New, Declined, Proposal sent, Ordered; Quotes.DefaultStatus is Draft.
