---
id: KB-C37FE4F5
subject: A product property value saved without a property definition surfaces in xAPI as propertyType PRODUCT even when the body declares type Variation, so it never becomes a storefront variant-picker option
plane: experiential
question: What property data must variations carry for the storefront PDP to render the VcVariantPicker variant picker?
questions:
  - text: Why does a variation's Color property show as PRODUCT instead of VARIATION in the GraphQL product query?
  - text: Can inline (propertyId null) property values drive the B2C product options block?
  - text: What makes vc-frontend use the B2C product template with the options picker?
concepts:
  - id: catalog
status: active
appliesTo:
  - axis: entity
    value: catalog-product-property
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.product
  - coordinate: Query.products
  - coordinate: POST /api/catalog/properties
  - coordinate: POST /api/catalog/products
evidence:
  - method: observation
    deployment: vcptcore_regression
    build: 99880d06bbb9b4cf
    conditions: platform=3.1076.8; theme=2.59.0; store=B2B-store
    at: 2026-11-06T11:02:46.078Z
    by: session:35ec50d2
    who: Aleksandra-Mitricheva
    note: "distinct: KB-4C17EE4B records that no product on the stand rendered the picker; this records the data mechanism (property definition type vs inline values) that decides whether variations can drive it."
---
Seeded a master with 4 variations via POST /api/catalog/products. First attempt: Color/Size values written inline (propertyId null, property type 'Variation' in the body). Query.product(...).variations[].properties and Query.products(filter "productfamilyid:<master> is:product,variation") returned Color/Size with propertyType PRODUCT, so the theme's options block (which groups only VARIATION-type properties) would render no picker. Second attempt: created Color and Size as real catalog properties (POST /api/catalog/properties, type Variation, ShortText, scoped to the products' own category via categoryId) and bound the variation values to their ids; the same queries then returned propertyType VARIATION. The master flag VirtoFrontend_UI_Layout=B2C written inline (Product type) is exposed fine and is inherited into each variation's properties. Theme side (vc-frontend 2.59.0 source): pages/product.vue switches to the B2C template only when that property's value === "B2C" exactly, and the B2C variations search is in-stock only, so variations also need stock.
