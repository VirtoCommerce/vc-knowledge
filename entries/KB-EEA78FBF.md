---
id: KB-EEA78FBF
subject: POST /api/return/{id}/authorize items[].lineItemId is the RETURN line item id, every line must be decided, and the status follows the approved quantities
plane: experiential
question: POST /api/return/{id}/authorize — is items[].lineItemId the return line item id or the order line item id, and which status results?
status: active
appliesTo:
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/return/{id}/authorize
evidence:
  - method: source
    deployment: VirtoCommerce.Return@3.1004.0
    at: 2026-10-15T16:00:57.306Z
    by: session:3ec30154
    who: Lenajava1
---
ReturnFlowService.Authorize matches each ReturnLineDecision.lineItemId against orderReturn.LineItems[].Id (the return's own line), not OrderLineItemId; an unknown id is refused ("has no line"), a line without a decision is refused ("has no decision"), approvedQuantity must be 0..requested. Resulting status: every line approved in full -> Approved, every line 0 -> Rejected, otherwise PartiallyApproved; the return-level rejectReason is kept only when not Approved, and a per-line rejectReason only when approved < requested. Authorize is allowed only from statuses the state provider permits (Requested/New). Live-consistent: seeding on virtostart with return-line ids produced Approved, PartiallyApproved (4->1, 1->1) and Rejected exactly as computed.
