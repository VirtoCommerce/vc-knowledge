---
id: KB-FBB0ED79
subject: xAPI product search results (ExpProduct) take the product model, variation ids, min variation prices, relevance score and key from index fields through [BindIndexField] binders, not from the database
plane: experiential
question: Which ExpProduct / ExpCategory fields in xCatalog are bound from the search index via BindIndexField, and from which index field?
questions:
  - text: Where does Query.products get product properties and prices from — the index or the database?
  - text: Which binder fills minVariationPrice in xCatalog?
  - text: What index field does CatalogProductBinder read?
  - text: Does xCatalog category binding use the indexed __object field?
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.products
  - coordinate: Query.product
  - coordinate: Query.categories
evidence:
  - method: source
    deployment: VirtoCommerce.XCatalog@3.1027.0
    at: 2026-11-18T10:17:20.078Z
    by: session:25a30bd6
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: f75b254d87d474d1
    conditions: module:VirtoCommerce.XCatalog=3.1027.0-pr-107-5ca9; module:VirtoCommerce.Xapi=3.1033.0-pr-79-4dfa; setting:Catalog.Search.UseFullObjectIndexStoring=false
    at: 2026-11-18T10:27:43.199Z
    by: session:25a30bd6
    bodyHash: a12480e78d14
    who: Lenajava1
    contradicts: true
    note: The product-model half holds only when Catalog.Search.UseFullObjectIndexStoring=true. On vcst_qa the setting is false (default false); Product index documents (GET /api/search/indexes/index/Product/{id}) carry no __object, so CatalogProductBinder binds null and xCatalog EnsureCatalogProductLoadedMiddleware loads the product from the database. Query.product data then comes from the DB, not the index — admin REST is not an independent reference there. __variations, __minvariationprice and __score ARE bound from the index as stated. See KB-AE10CEB6.
---
Read in vc-module-x-catalog source (PR 107 head 5ca93cfa, XCatalog 3.1027.0) src/VirtoCommerce.XCatalog.Core/Models/ExpProduct.cs: IndexedProduct ← index field "__object" via CatalogProductBinder; IndexedVariationIds ← "__variations" via VariationsBinder; IndexedMinVariationPrices ← "__minvariationprice" via MinVariationPriceBinder; RelevanceScore ← SearchModule RelevanceScore field via DefaultPropertyIndexBinder; Key via KeyBinder. ExpCategory.cs: Category ← "__object" via CategoryBinder; Key via KeyBinder; RelevanceScore via DefaultPropertyIndexBinder. The binders are resolved in vc-module-x-api TypeExtensions (GenericModelBinder). So the product data Query.products / Query.product return is the index document's stored __object, deserialized per request — admin REST /api/catalog/products reads the database instead, which makes it an independent reference for xAPI product data. Not observed live by this capture.
