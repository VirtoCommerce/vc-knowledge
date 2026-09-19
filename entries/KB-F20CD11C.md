---
id: KB-F20CD11C
subject: On Orders 3.1015.0 the order's configuration items DO carry sectionId and sectionName, so the section a choice answered is recoverable
plane: experiential
question: does a configurable product's order line record which configuration section each chosen option answered
status: active
appliesTo:
  - axis: entity
    value: configurable-product-order-line
  - axis: module
    value: virtocommerce.orders
  - axis: surface
    value: rest-api
  - axis: version
    value: 3.1015.0
anchors:
  - coordinate: GET /api/order/customerOrders/{id}
  - coordinate: POST /api/catalog/products/configurations/search
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-19T16:29:30.169Z
    by: session:local_08
---
Read a real placed order on vcst-qa (VirtoCommerce.Orders 3.1015.0, Catalog 3.1044.0) via GET /api/order/customerOrders/{id}. Each element of the configurable line's configurationItems[] carried BOTH sectionId and sectionName alongside productId, sku, name, quantity, price, salePrice, extendedPrice, imageUrl, catalogId, categoryId, type, customText and files. The two sectionIds (7a93e6ad-... "Storage", ef5c885c-... "RAM") matched exactly the section ids returned by POST /api/catalog/products/configurations/search for that product, and each chosen productId was a declared option of its own section — so the full configuration was reconstructable from the order alone, with no ambiguity between two same-typed Product sections. This refines the DISPUTED entry KB-360127D0 and KB-59E4B5FC, both observed on vcptcore_stable, which state that sectionId is the one field lost when a cart configuration item crosses to the order. On this deployment it is not lost, and sectionName is additionally present. The likely explanation is a module-version difference, so the "sectionId is lost" claim should be read as version-bound rather than general; check the Orders module version before relying on either shape. Separately observed on the same order: the configurable line's own sku is synthesized as "Configuration-" + the catalog product code (order line sku "Configuration-AGENT-TEST-CFG-013" vs catalog code "AGENT-TEST-CFG-013"), so matching an order line sku against a catalog code fails for configurable lines; and the line's price is the rolled-up base + options (999 + 150 + 100 = 1249) with isConfigured=true and no separate lines for the option products.
