---
id: KB-6CDB7E6F
subject: Storefront return details page shows 'Return' title placeholder while GetReturn loads
plane: experiential
question: What does /account/returns/{id} show as title and breadcrumb while the return is loading?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/returns/{id}
  - coordinate: /account/returns/new/{orderId}
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-30T14:50:56.372Z
    by: session:0cfc9f97
    who: kutasinaelena
---
On theme 2.59.0-pr-2500, navigating from /account/returns to /account/returns/{id} shows the h1 'Return' and breadcrumb 'Home / Account / Returns / Return' for about 0.8 s, then 'Return <number>' in both. Return-quantity inputs on /account/returns/new/{orderId} and /account/returns/{id}/edit are vc-input size sm (38 px tall); typing a value above the returnable quantity leaves the field at the last valid value (5 then 0 with 5 returnable shows 5).
