---
id: KB-59E4B5FC
subject: "a configured product as an order line item: stored order loses only sectionId, storefront GraphQL order item exposes five fields"
plane: experiential
question: how does a configurable product become a line item on an order
questions:
  - text: When I order a customized product, does the order remember which options I picked?
  - text: Does a configurable product become one order line with its options nested, or separate lines per option?
  - text: Which configuration item field is lost when a cart line becomes an order line - is sectionId dropped?
  - text: Why does the storefront order configuration item type reject productId, sku and quantity while the REST order has them?
  - text: Is an order line for a configurable product with an empty configuration items array a data fault?
  - text: After I order a custom-built item, does the shop still know exactly which parts and how many I chose?
  - text: Can a back-office integration reconcile a configured order line to catalog products from the order record?
  - text: Which configuration item field is lost when a cart becomes an order in the REST model?
  - text: Does the order REST endpoint return productId and quantity for Product-type configuration items?
  - text: In my order history, can the shop tell which product I picked for each configuration option?
  - text: Why can't a storefront order query return the product and quantity chosen in a configurable item?
  - text: Which fields does the GraphQL order configuration item type lack compared with the cart configuration item type?
  - text: Does selecting productId or quantity on an order's configuration items fail schema validation?
concepts:
  - id: configured-line-item
  - id: order-line-item
  - id: configurable-product
status: active
appliesTo:
  - axis: surface
    value: rest
  - axis: surface
    value: xapi
anchors:
  - coordinate: OrderLineItemType.configurationItems
  - coordinate: OrderConfigurationItemType
  - coordinate: GET /api/order/customerOrders/{id}
  - coordinate: CartConfigurationItemType
evidence:
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-14T12:31:41.915Z
    by: session:a1f22912
  - method: observation
    deployment: vcptcore_stable
    at: 2026-09-13T14:07:00.315Z
    by: session:6d4f2631
    splitFrom: KB-360127D0
    mergedFrom: KB-9DB54091
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-14T12:30:55.989Z
    contradicts: true
    note: "Checked against a real placed order (the entry says it was read off the schema, and that is where it goes wrong). The SCHEMA DIFF it reports is right and the conclusion drawn from it is not. The stored order keeps the product and the quantity: GET /api/order/customerOrders/{id} returned, per Product-type configuration item, productId, sku, quantity, imageUrl, catalogId and categoryId alongside name and type -- so the order does identify the catalog product that was chosen and how many were taken. Comparing the two stored shapes in the modules' own swagger, CartConfigurationItem carries 16 fields and OrderConfigurationItem 15, and the ONLY field the crossing loses is sectionId. What the entry actually describes is the storefront GraphQL projection of an order: OrderConfigurationItemType exposes 5 fields and refuses productId, quantity, sectionId, sku, catalogId, categoryId and imageUrl by validation error. So the loss is in one read surface, not in the record."
    splitFrom: KB-360127D0
    mergedFrom: KB-9DB54091
  - method: observation
    deployment: vcptcore_stable
    at: 2026-09-13T14:07:00.315Z
    by: session:6d4f2631
    splitFrom: KB-360127D0
    mergedFrom: KB-E1E2716F
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-14T12:30:55.989Z
    contradicts: true
    note: "Checked against a real placed order (the entry says it was read off the schema, and that is where it goes wrong). The SCHEMA DIFF it reports is right and the conclusion drawn from it is not. The stored order keeps the product and the quantity: GET /api/order/customerOrders/{id} returned, per Product-type configuration item, productId, sku, quantity, imageUrl, catalogId and categoryId alongside name and type -- so the order does identify the catalog product that was chosen and how many were taken. Comparing the two stored shapes in the modules' own swagger, CartConfigurationItem carries 16 fields and OrderConfigurationItem 15, and the ONLY field the crossing loses is sectionId. What the entry actually describes is the storefront GraphQL projection of an order: OrderConfigurationItemType exposes 5 fields and refuses productId, quantity, sectionId, sku, catalogId, categoryId and imageUrl by validation error. So the loss is in one read surface, not in the record."
    splitFrom: KB-360127D0
    mergedFrom: KB-E1E2716F
---
The cart line crosses whole: one order line item on the base product, carrying the rolled-up price and its configurationItems, and the option products still do not become lines of their own. Comparing the two STORED shapes in the modules' own swagger settles what the crossing costs: CartConfigurationItem has 16 fields and OrderConfigurationItem has 15, and the one field that does not cross is sectionId. Everything else is kept -- productId, sku, quantity, imageUrl, catalogId, categoryId, type, customText, files -- and a placed order read back through GET /api/order/customerOrders/{id} showed all of them populated per Product-type option alongside name and type, so the order DOES record which catalog product was chosen and how many were taken. Losing sectionId costs the ability to say which question an answer belonged to: two Product sections yield two indistinguishable Product configuration items, their array order is not stable across surfaces, and a configuration offering the same option product in two sections would be unreconstructable. The bigger loss is not in the record but in one read of it: the storefront GraphQL CartConfigurationItemType has eight fields (id, name, type, customText, files, productId, quantity, sectionId), while OrderConfigurationItemType exposes only five -- id, name, type, customText and files -- and refuses productId, quantity, sectionId, sku, catalogId, categoryId and imageUrl with a validation error. So for a Product section a buyer-facing order page has only the chosen option's display NAME to match on, nothing identifying the product or quantity and not which section it answered, while the same order read over REST has the ids. This is read off the deployment's own schema: diff the two type tables on any deployment to confirm or retire it. Note also that a line whose configurationItems is an empty array is a legitimate order line for a configurable product, not a data fault.
