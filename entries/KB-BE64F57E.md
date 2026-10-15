---
id: KB-BE64F57E
subject: Quote statuses have no "Cancelled" value; a full-body PUT /api/quote/requests moves a Processing quote to Declined and keeps its attachments
plane: experiential
question: How do I cancel or decline a Processing quote request via admin REST, and which status values are allowed?
status: active
appliesTo:
  - axis: principal
    value: admin
  - axis: surface
    value: rest
anchors:
  - coordinate: PUT /api/quote/requests
  - coordinate: GET /api/quote/requests/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-15T10:55:25.315Z
    by: session:10db9d7d
    who: Lenajava1
---
2026-10-15, admin token. GET /api/platform/settings/modules/VirtoCommerce.Quote shows Quotes.Status allowed values Processing, On hold, New, Declined, Proposal sent, Ordered (default New), with Quotes.DefaultStatus = Draft. There is no Cancelled status. For three B2B quote requests in Processing, I sent PUT /api/quote/requests with the full GET /api/quote/requests/{id} body, changed only in status Declined, isCancelled true, cancelReason and cancelledDate. Each read back as status Declined, isCancelled true, with the cancelReason, and its attachments (one quote had one) unchanged. So to withdraw a quote via REST, set Declined (optionally also isCancelled and cancelReason) rather than looking for a Cancelled status.
