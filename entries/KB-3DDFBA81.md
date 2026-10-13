---
id: KB-3DDFBA81
subject: A Variation-type configuration section can be created via REST with empty options; createConfiguredLineItem prices it as base + the chosen variation's price
plane: experiential
question: Can a configurable product have a Variation-type section, and how does createConfiguredLineItem price a variation chosen in it?
status: active
appliesTo:
  - axis: feature
    value: configurable-products
  - axis: surface
    value: rest-api
  - axis: surface
    value: xapi
anchors:
  - coordinate: POST /api/catalog/products/configurations
  - coordinate: Query.productConfiguration
  - coordinate: Mutation.createConfiguredLineItem
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T20:40:01.621Z
    by: session:624d35a1
    who: kutasinaelena
---
2026-10-13, admin REST then anonymous xAPI. POST /api/catalog/products/configurations with a section {type:"Variation", options:[]} on a parent that has two real variations (mainProductId = parent, priced 120 and 200 USD; parent 100 USD) was accepted. Query.productConfiguration returns the section with type Variation and options [] (the variations are not listed as options), while product(id).variations lists both. createConfiguredLineItem(currencyCode USD) with configurationSections [{sectionId, type:"Variation", option:{productId: <variation 1>, quantity:1}}] returned id null and listPrice 220 = parent 100 + variation 120.
