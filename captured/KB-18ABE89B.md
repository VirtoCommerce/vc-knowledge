---
id: KB-18ABE89B
subject: admin order address blade false dirty state
plane: experiential
question: why does the admin order address form say the address has been modified when I only opened it?
status: active
refutableBy: observation
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: GET /api/order/customerOrders/{id}
  - coordinate: OrderAddressType.countryName
evidence:
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-14T07:50:45.243Z
    by: session:ea806a91
---

Because the form marks itself dirty on bind, before any interaction. Open any order-operation address blade in the Admin -- the order's own Shipping or Billing row, or a shipment's deliveryAddress -- and the Save button is already ENABLED on a form nobody has touched; close it and a modal asks The address has been modified. Do you want to save changes?. Answer NO. Answering Yes writes the bound values back over a record you did not intend to change, and on an order that write is not a no-op. The visible discrepancy is the Country control: the stored countryName is United States while the select renders United States of America, so binding the stored value to the option list substitutes a different string and the dirty check sees a change. Reproducible with zero keystrokes, on two different address blades of the same order. The practical rule for anyone inspecting order data through the Admin: treat every address blade as read-only, leave via No, and if you actually need the field values read them from the order API response instead, where they are exactly as stored.
