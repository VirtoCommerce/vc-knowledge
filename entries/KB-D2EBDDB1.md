---
id: KB-D2EBDDB1
subject: "Catalog REST: categories are created AND updated with POST /api/catalog/categories (PUT is 405), and DELETE /api/catalog/categories needs repeated ids= params; a comma list returns 204 and deletes nothing"
plane: experiential
question: How do I update or bulk-delete categories through the Catalog REST API?
status: active
appliesTo:
  - axis: module
    value: catalog
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/catalog/categories
  - coordinate: DELETE /api/catalog/categories
  - coordinate: DELETE /api/catalog/catalogs/{id}
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-07T19:38:43.432Z
    by: session:ed63bd34
    who: kutasinaelena
---
Catalog 3.1049.0-pr-910 build. PUT /api/catalog/categories with the full category object returns 405 and changes nothing; POST /api/catalog/categories with the full object (id included) updates it in place (seoInfos[0].isActive flipped, same seo record id, count unchanged). DELETE /api/catalog/categories?ids=<a>,<b> returns 204 but both categories still read 200 afterwards (the comma list binds as one unknown id); DELETE /api/catalog/categories?ids=<a>&ids=<b> deletes both (GET 404). DELETE /api/catalog/catalogs/{id} cascades to the categories inside it, which masks a failed category delete in teardown. A category can be created directly in a virtual catalog with POST /api/catalog/categories {catalogId, name, code, seoInfos:[{semanticUrl, languageCode, isActive, storeId?, objectType:"Category"}]}; store-scoped and non-default-language (de-DE, when the catalog lists it) seoInfos persist as sent.
