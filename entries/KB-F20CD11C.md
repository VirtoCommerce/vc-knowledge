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
  - method: observation
    deployment: vcst_qa
    at: 2026-09-20T18:16:54.445Z
    by: session:local_89
    note: "Independently re-observed on a second product and a second order. Order CO260920-00001, B2B-store, configurable line sku \"Configuration-YER-80407217\" (catalog code YER-80407217), isConfigured=true. All three configurationItems carried sectionId AND sectionName; the three sectionIds matched exactly the three sections returned by POST /api/catalog/products/configurations/search for productId 38dbe95c-3f46-48ff-bb9a-8bd96f475214 (Color/Product/required, Print/Product/optional, Your text/Text/optional), and each chosen productId was a declared option of its OWN section (the Color and Print option sets are disjoint), so a cross-section mis-binding would have been detectable. Rolled-up line price also re-confirmed: base 16 + option 2 + option 18 = placedPrice/extendedPrice 36, with no separate lines for the option products. Text-section customText survived byte-identical (29 chars, hex-compared)."
  - method: observation
    deployment: vcst_qa
    at: 2026-09-21T08:12:13.601Z
    by: session:local_e8
    note: Third independent observation. Order CO260921-00002 (id e63d3ee1-9437-4285-aa45-e030c507030d), B2B-store, Orders 3.1015.0 / Catalog 3.1044.0. GET /api/order/customerOrders/{id} returned the configurable line sku "Configuration-AGENT-TEST-CFG-032" (catalog code AGENT-TEST-CFG-032), isConfigured=true, and its single configurationItems[] element carried sectionId 6908b6c0-f048-44d5-a8d3-480484f9116a AND sectionName "Select one" alongside productId, sku, quantity, price, salePrice, extendedPrice, type, catalogId, categoryId. The sectionId matched exactly the one section returned by GraphQL productConfiguration(configurableProductId:"d020b5c7-3218-45de-9f72-8c6fec4a3755" storeId:"B2B-store"), and the stored productId abed9a58-14a1-42ce-8dc8-52c1c0a21653 was a declared option of that section. The "Configuration-<catalog code>" sku synthesis re-confirmed on a third distinct product.
  - method: observation
    deployment: vcst_qa
    at: 2026-09-21T08:12:13.601Z
    by: session:local_e8
    note: Third independent observation. Order CO260921-00002 (id e63d3ee1-9437-4285-aa45-e030c507030d), B2B-store, Orders 3.1015.0 / Catalog 3.1044.0. GET /api/order/customerOrders/{id} returned the configurable line sku "Configuration-AGENT-TEST-CFG-032" (catalog code AGENT-TEST-CFG-032), isConfigured=true, and its single configurationItems[] element carried sectionId 6908b6c0-f048-44d5-a8d3-480484f9116a AND sectionName "Select one" alongside productId, sku, quantity, price, salePrice, extendedPrice, type, catalogId, categoryId. The sectionId matched exactly the one section returned by GraphQL productConfiguration(configurableProductId:"d020b5c7-3218-45de-9f72-8c6fec4a3755" storeId:"B2B-store"), and the stored productId abed9a58-14a1-42ce-8dc8-52c1c0a21653 was a declared option of that section. The "Configuration-<catalog code>" sku synthesis re-confirmed on a third distinct product.
---
Read a real placed order on vcst-qa (VirtoCommerce.Orders 3.1015.0, Catalog 3.1044.0) via GET /api/order/customerOrders/{id}. Each element of the configurable line's configurationItems[] carried BOTH sectionId and sectionName alongside productId, sku, name, quantity, price, salePrice, extendedPrice, imageUrl, catalogId, categoryId, type, customText and files. The two sectionIds (7a93e6ad-... "Storage", ef5c885c-... "RAM") matched exactly the section ids returned by POST /api/catalog/products/configurations/search for that product, and each chosen productId was a declared option of its own section — so the full configuration was reconstructable from the order alone, with no ambiguity between two same-typed Product sections. This refines the DISPUTED entry KB-360127D0 and KB-59E4B5FC, both observed on vcptcore_stable, which state that sectionId is the one field lost when a cart configuration item crosses to the order. On this deployment it is not lost, and sectionName is additionally present. The likely explanation is a module-version difference, so the "sectionId is lost" claim should be read as version-bound rather than general; check the Orders module version before relying on either shape. Separately observed on the same order: the configurable line's own sku is synthesized as "Configuration-" + the catalog product code (order line sku "Configuration-AGENT-TEST-CFG-013" vs catalog code "AGENT-TEST-CFG-013"), so matching an order line sku against a catalog code fails for configurable lines; and the line's price is the rolled-up base + options (999 + 150 + 100 = 1249) with isConfigured=true and no separate lines for the option products.
