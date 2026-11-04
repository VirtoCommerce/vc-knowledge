---
id: KB-E0B4D1F8
subject: POST /api/catalog/search does not exist (404, empty body); catalogs are found by name with POST /api/catalog/catalogs/search {keyword}
plane: experiential
question: Which REST route finds a catalog by its name?
questions:
  - text: Why does POST /api/catalog/search return nothing for a catalog that exists?
  - text: How do I look up a catalog by keyword through the admin REST API?
  - text: Is there a /api/catalog/search route for catalogs?
status: active
appliesTo:
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/catalog/search
  - coordinate: POST /api/catalog/catalogs/search
evidence:
  - method: observation
    deployment: vcst_qa
    build: 4fafc097f6a88f6c
    conditions: module:VirtoCommerce.Catalog=3.1051.0; role=platform administrator (REST)
    at: 2026-11-04T09:32:40.481Z
    by: session:322b37a0
    who: Lenajava1
---
2026-11-04, admin bearer token, Catalog 3.1051.0. POST /api/catalog/search with {keyword:"<exact catalog name>", take:50} returned HTTP 404 with no body and no content-type, for a catalog that exists. POST /api/catalog/catalogs/search with {keyword:"<exact catalog name>", take:50} (searchPhrase works too) returned 200 {totalCount, results[]} listing every catalog with that name. A find-or-create built on the first route never finds the catalog, so each run creates another copy and a name-based teardown deletes none; two same-name copies were left this way.
