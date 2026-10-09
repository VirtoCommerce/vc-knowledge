---
id: KB-AF957CC2
subject: "Storefront return: Continue on select-items creates a draft with a RET number, Submit return sets Requested, Cancel return (optional reason) sets Cancelled and releases the returnable quantity"
plane: experiential
question: What happens across the storefront create-return flow from an order through submit and cancel?
status: active
appliesTo:
  - axis: feature
    value: returns
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/returns/new/{orderId}
  - coordinate: /account/returns/{id}
  - coordinate: Mutation.createReturn
  - coordinate: Mutation.submitReturn
  - coordinate: Mutation.cancelReturn
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-09T10:56:17.118Z
    by: session:df9d131f
    who: Lenajava1
---
On theme vc-frontend-next 3.0.0-alpha.2685 with Return 3.1005.0-pr-28 (Return.ReturnEnabled on, window 30 days), a Completed order with a delivered shipment shows 'Request return' on /account/orders/{id}. It opens /account/returns/new/{orderId} ('Items can be returned within 30 days of delivery', per-line Ordered/Returnable/Return qty). Continue fires CreateReturn and navigates to /account/returns/{returnId}/edit titled 'Add details to return RET<yymmdd>-<n>' (draft auto-saved, 'Saved automatically'); Submit return stays disabled until each line has a Reason (dictionary keys shown raw, e.g. NoLongerNeeded). SubmitReturn lands on /account/returns/{id} with Status Requested and a Cancel return button; its dialog ('will be withdrawn and its items will become available to return again', optional reason) fires CancelReturn, status becomes Cancelled, and the wizard for the same order shows the full returnable quantity again.
