---
id: KB-E23CFAF6
subject: A quote put into Proposal sent by a REST status write produces no storefront header notification for the buyer
plane: experiential
question: Does a quote status change to Proposal sent produce a notification in the storefront header notifications popup for the buyer?
status: active
appliesTo:
  - axis: surface
    value: storefront
anchors:
  - coordinate: /account/quotes
evidence:
  - method: observation
    deployment: vcst_qa
    build: 7ec352df61232ca8
    conditions: platform=3.1083.0; theme=2.60.0-alpha.2579; quote-status-source=REST status write
    at: 2026-10-15T13:54:12.768Z
    by: session:10db9d7d
    who: Lenajava1
---
The buyer storefront header notification popup (badge 18) listed no message about any quote, although the buyer owned quotes in Proposal sent. Those quotes reached that status through a back-office REST status write (seeding), not through the admin Submit proposal action, so the notification event may simply not have been raised. This shows only that a REST status write does not notify. Whether a real admin proposal notifies the buyer was not established.
