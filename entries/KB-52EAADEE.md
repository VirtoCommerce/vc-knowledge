---
id: KB-52EAADEE
subject: A catalog product property is indexed under its lowercased name in the shared Product index, so a property named like a typed field (Price → price double, Size → size integer) with a non-numeric value gets the whole document rejected
plane: experiential
question: Why does a Product index build reject a document with "failed to parse field [price] of type [double]" or "[size] of type [integer]" when the product has a text property called Price or Size?
questions:
  - text: How are product property names mapped to Elasticsearch field names in the Product index?
  - text: Can a catalog property named Price or Size break indexing of a product?
  - text: Is the Product index mapping shared across catalogs and stores?
status: active
appliesTo:
  - axis: module
    value: catalog
  - axis: subsystem
    value: search-index
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/search/indexes/index
  - coordinate: POST /api/catalog/products
evidence:
  - method: observation
    deployment: vcst_qa
    build: e967c68919b4671e
    conditions: platform=3.1093.0-pr-3154-4607; module:VirtoCommerce.ElasticSearch8=3.1012.0-pr-47-d205; module:VirtoCommerce.Catalog=3.1051.0
    at: 2026-11-03T21:12:45.949Z
    by: session:ef5e0d1c
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: 4fafc097f6a88f6c
    conditions: platform=3.1093.0-pr-3154; jobEngine=RabbitMQ; provider=ElasticSearch8
    at: 2026-11-04T08:28:25.570Z
    by: session:ef5e0d1c
    bodyHash: d2fc7315ce68
    who: Lenajava1
    note: "2026-11-04: a 7-type Delete and build from the admin Search index blade (POST /api/search/indexes/index, deleteExistingIndex true) ran in 4m49s and ended 'Indexation completed with errors' with exactly 2 error rows: 'ID: Elasticsearch8 Server, Error: ' and the [price] double parse rejection of the product whose ShortText property Price = 'PROPERTY-Price-not-the-field'. All 7 types got a new active index."
---
On vcst_qa (ES8 provider), a Product build via POST /api/search/indexes/index rejected: a product with an inline ShortText property `Price` = 'PROPERTY-Price-not-the-field' ("failed to parse field [price] of type [double]"), and 8 products with an inline ShortText `Size` = L / S / 27-inch / 34-inch ("failed to parse field [size] of type [integer]"). The catalog document builder adds each property value as a field named propertyName.ToLowerInvariant() with no prefix (CatalogDocumentBuilder.IndexCustomProperty), so a property shares the field namespace with built-in fields (price, sku, availability, ...) and with every other catalog's properties: there is one Product index for all catalogs, and a field's type is whatever was mapped first. Here `size` was mapped integer although no indexed product currently carries an integer Size (the store's virtual catalog defines a Size property of type Integer). Renaming the 8 products' inline property to a distinct name (AgentTestSize) made them index with no error on the next build. A text-valued property whose lowercased name equals a typed field cannot be indexed; a namespaced property name avoids it.
