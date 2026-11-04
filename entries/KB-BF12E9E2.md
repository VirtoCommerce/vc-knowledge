---
id: KB-BF12E9E2
subject: A numeric inline product property named Price indexes without error, and xAPI price sort and price range filter keep using the product's list price, not the property value
plane: experiential
question: If a product has a numeric property literally named Price, does it break indexing or change the storefront price sort and price filter?
questions:
  - text: Does a Number property called Price collide with the price field in the Product search index?
  - text: Does a product property named Price change xAPI products sort price:asc?
  - text: Can a product property named Price be filtered with filter price:[a TO b] in xAPI?
status: active
appliesTo:
  - axis: module
    value: xcatalog
  - axis: subsystem
    value: search-index
  - axis: surface
    value: graphql
anchors:
  - coordinate: Query.products
  - coordinate: POST /api/search/indexes/index
evidence:
  - method: observation
    deployment: vcst_qa
    build: 4fafc097f6a88f6c
    conditions: platform=3.1093.0-pr-3154-4607; module:VirtoCommerce.Catalog=3.1051.0; module:VirtoCommerce.XCatalog=3.1024.0-pr-115-8339; module:VirtoCommerce.ElasticSearch8=3.1012.0-pr-47-d205; provider=ElasticSearch8
    at: 2026-11-04T08:52:00.944Z
    by: session:ef5e0d1c
    who: Lenajava1
---
On vcst_qa (ES8), a product in store B2B-store with list price 39.99 carried an inline Product property named Price with valueType Number and value 98765.43. A Product Delete and build (POST /api/search/indexes/index, deleteExistingIndex true) ended with errorCount 0, and anonymous xAPI Query.products found the product by its SKU, with properties[] listing Price as NUMBER 98765.43 and price.actual.amount 39.99. Over the 13 products of its fixture set, sort "price:asc" and "price:desc" placed it by 39.99, filter "price:[39 TO 40]" returned it and filter "price:[90000 TO 100000]" (also "Price:[...]") returned nothing. So xAPI's price sort and price range filter read the pricelist price fields, not the property. Whether the property value is also written into the raw index field "price" (per KB-52EAADEE the builder uses the lowercased property name) was not visible through REST or xAPI. A products facet:"price" request returned no range or term facet on this store, so the effect on a price facet was not observed. Text values under the same name are rejected (KB-52EAADEE).
