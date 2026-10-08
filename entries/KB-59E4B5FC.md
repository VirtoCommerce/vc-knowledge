---
id: KB-59E4B5FC
subject: "A configured product as an order line item: one line with configurationItems; on the 3.1000.x support line the stored order drops sectionId and storefront GraphQL exposes five fields, from XOrder 3.1005 the ids are exposed and from 3.1013 sectionId and sectionName cross"
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
    resolved: claim-amended
    resolvedAt: 2026-10-08T10:55:43.672Z
    resolvedBy: session:d3bcc6b6
    resolvedIn: judge/configured-line-item-20261008
    resolution: "The body states it: the record keeps the product and quantity and drops only sectionId on that build; the September order re-read live 2026-10-08 shows exactly that."
  - method: observation
    deployment: vcptcore_stable
    at: 2026-09-13T14:07:00.315Z
    by: session:6d4f2631
    splitFrom: KB-360127D0
    mergedFrom: KB-E1E2716F
    duplicateSession: true
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-14T12:30:55.989Z
    contradicts: true
    note: "Checked against a real placed order (the entry says it was read off the schema, and that is where it goes wrong). The SCHEMA DIFF it reports is right and the conclusion drawn from it is not. The stored order keeps the product and the quantity: GET /api/order/customerOrders/{id} returned, per Product-type configuration item, productId, sku, quantity, imageUrl, catalogId and categoryId alongside name and type -- so the order does identify the catalog product that was chosen and how many were taken. Comparing the two stored shapes in the modules' own swagger, CartConfigurationItem carries 16 fields and OrderConfigurationItem 15, and the ONLY field the crossing loses is sectionId. What the entry actually describes is the storefront GraphQL projection of an order: OrderConfigurationItemType exposes 5 fields and refuses productId, quantity, sectionId, sku, catalogId, categoryId and imageUrl by validation error. So the loss is in one read surface, not in the record."
    splitFrom: KB-360127D0
    mergedFrom: KB-E1E2716F
    resolved: claim-amended
    resolvedAt: 2026-10-08T10:56:14.685Z
    resolvedBy: session:d3bcc6b6
    resolvedIn: judge/configured-line-item-20261008
    resolution: "The body states it: the record keeps the product and quantity and drops only sectionId on that build; the September order re-read live 2026-10-08 shows exactly that."
  - method: observation
    deployment: vcst_qa
    at: 2026-10-05T10:07:31.572Z
    by: session:8dcbfefd
    who: Dan-BV
    contradicts: true
    note: "2026-10-05 vcst_qa (XOrder 3.1013.0, Orders 3.1016.0): OrderConfigurationItemType exposes 15 fields including sectionId/productId/sku/quantity, not five; and stored orders DO keep sectionId and sectionName. Both version-bound claims fail on this build; they may still describe another deployment."
    resolved: version-scoped
    resolvedAt: 2026-10-08T10:55:44.494Z
    resolvedBy: session:d3bcc6b6
    resolvedIn: judge/configured-line-item-20261008
    resolution: True from XOrder 3.1013 / Orders 3.1016 (live on 3.1014 / 3.1018 2026-10-08); the five-field projection and the missing sectionId belong to the 3.1000.x support line. Body scoped by build.
  - method: observation
    deployment: vcptcore_stable
    conditions: platform=3.1039.12; module:VirtoCommerce.Orders=3.1009.0; module:VirtoCommerce.XOrder=3.1005.0; module:VirtoCommerce.Cart=3.1006.1; module:VirtoCommerce.XCart=3.1021.2; theme=2.51.2; store=B2B-store; role=Administrator; state:order=written 2026-09-14 on Orders 3.1000.4
    at: 2026-10-08T10:55:42.101Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "Order CO260914-00003 (placed on the September build) read 2026-10-08: one configured line per configured product, no option lines; stored items keep productId, sku, quantity, imageUrl, catalogId, categoryId, sectionId null. GraphQL OrderConfigurationItemType has 14 fields incl. productId, sku, quantity and sectionId String!; selecting sectionId on this order fails \"Cannot return null for a non-null type\"."
  - method: observation
    deployment: vcst_qa
    conditions: platform=3.1076.0; module:VirtoCommerce.Orders=3.1018.0; module:VirtoCommerce.XOrder=3.1014.0; module:VirtoCommerce.Cart=3.1012.0; module:VirtoCommerce.XCart=3.1039.0; theme=2.59.0; store=B2B-store; role=Customer (owner) / Administrator
    at: 2026-10-08T10:55:42.893Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "Fresh configured order 2026-10-08 (3 sections: Product, Product, Text): one configured line on the base product, extendedPrice rolled up, no option lines; stored items keep sectionId and sectionName matching the chosen sections plus productId, sku, quantity, prices, catalogId, categoryId; GraphQL order(id) returns all 15 OrderConfigurationItemType fields with the same values as REST."
---
The cart line crosses whole on every build checked: one order line item on the base product, carrying the rolled-up price and its configurationItems, and the option products do not become lines of their own (observed live 2026-10-08 on a fresh order on vcst_qa, and on a September order on vcptcore_stable). A line whose configurationItems is an empty array also exists on a configurable product's order (seen on vcptcore_stable).

What crosses with each configuration item, and what the storefront can read back, depends on the build:

- 3.1000.x support line (vcptcore_stable in September: Orders 3.1000.4, XOrder 3.1000.1): the stored order keeps productId, sku, quantity, imageUrl, catalogId, categoryId, type, customText and files per item, but NOT sectionId - an order written then reads back today with sectionId null on every item. Losing sectionId means two Product sections give two indistinguishable Product items whose array order is not stable across surfaces. The storefront GraphQL OrderConfigurationItemType exposed only id, name, type, customText and files and refused productId, quantity, sectionId, sku, catalogId, categoryId and imageUrl, so a buyer-facing order page had only the option's display name.
- From XOrder 3.1005.0 (vc-module-x-order #39): OrderConfigurationItemType exposes id, sectionId (non-null String!), name, productId, sku, imageUrl, quantity, type, customText, price, salePrice, extendedPrice, files and product. Observed live 2026-10-08 on XOrder 3.1005.0 / Orders 3.1009.0. Querying sectionId on an order written by the older build, whose stored sectionId is null, fails with "Cannot return null for a non-null type" and nulls every configuration item of that order.
- From XOrder 3.1013.0 (#44) with Orders 3.1016+: the stored order keeps sectionId and sectionName, each matching the section it was chosen in, and GraphQL adds sectionName (15 fields). Observed live 2026-10-08 on a fresh order on XOrder 3.1014.0 / Orders 3.1018.0. The REST array order of the items can differ from the cart's.

catalogId and categoryId are on the stored order but not on OrderConfigurationItemType on any of these builds.
