---
id: KB-59E4B5FC
subject: a configured product as an order line item
plane: experiential
question: how does a configurable product become a line item on an order
status: active
appliesTo:
  - axis: surface
    value: rest
  - axis: surface
    value: graphql
anchors:
  - coordinate: OrderLineItemType.configurationItems
  - coordinate: OrderConfigurationItemType
  - coordinate: GET /api/order/customerOrders/{id}
evidence:
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-14T12:31:41.915Z
    by: session:a1f22912
---

The cart line crosses whole: one order line item on the base product, carrying the rolled-up price and its configurationItems, and the option products still do not become lines of their own. Comparing the two STORED shapes in the modules' own swagger settles what the crossing costs: CartConfigurationItem has 16 fields and OrderConfigurationItem has 15, and the one field that does not cross is sectionId. Everything else is kept -- productId, sku, quantity, imageUrl, catalogId, categoryId, type, customText, files -- and a placed order read back through GET /api/order/customerOrders/{id} showed all of them populated per option, so the order DOES record which catalog product was chosen and how many were taken. Losing sectionId costs the ability to say which question an answer belonged to: two Product sections yield two indistinguishable Product configuration items, their array order is not stable across surfaces, and a configuration offering the same option product in two sections would be unreconstructable. The bigger loss is not in the record but in one read of it: the storefront's OrderConfigurationItemType exposes only id, name, type, customText and files, and refuses productId, quantity, sectionId, sku, catalogId and categoryId with a validation error -- so a buyer-facing order page has only a display NAME to match on, while the same order read over REST has the ids. Note also that a line whose configurationItems is an empty array is a legitimate order line for a configurable product, not a data fault.
