---
id: KB-95FA5EFB
subject: xAPI category(id, storeId) returns null, with no error, for a category outside the store's catalog
plane: experiential
question: What does the GraphQL category query return for a category that is not in the store's catalog?
status: active
appliesTo:
  - axis: module
    value: xcatalog
  - axis: surface
    value: graphql
anchors:
  - coordinate: Query.category
  - coordinate: GET /api/seoinfos/explain
evidence:
  - method: observation
    deployment: vcptcore_dev
    at: 2026-10-05T21:39:54.696Z
    by: session:f9a8159e
---
Query.category(id, storeId, cultureName) for a category owned by another store's catalog returned data.category null and no errors[]; the same id with the owning store returned the category with its slug and seoInfo. Explain on the first store rejects the same category's record with NotInStoreCatalog, so the catalog membership decides both answers.
