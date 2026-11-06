---
id: KB-2ECC0C3A
subject: A category given the same store-scoped semanticUrl in every store language resolves through Query.slugInfo in each culture, including a store language its catalog does not declare
plane: experiential
question: Does a category slug resolve via slugInfo in every store culture when each language has its own SEO record with the same slug, even a language the catalog does not list?
questions:
  - text: Does Query.slugInfo resolve a category slug in pt-PT when the physical catalog has only en-US, de-DE and fr-FR?
  - text: Does a new root category linked into the store virtual catalog appear in Query.childCategories maxLevel 1 for every culture?
status: active
appliesTo:
  - axis: entity
    value: catalog-category-seo
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.slugInfo
  - coordinate: Query.childCategories
  - coordinate: POST /api/catalog/categories
evidence:
  - method: observation
    deployment: vcptcore_regression
    build: 99880d06bbb9b4cf
    conditions: platform=3.1076.8; store=B2B-store; store-languages=en-US,fr-FR,de-DE,pt-PT
    at: 2026-11-06T11:02:36.507Z
    by: session:35ec50d2
    who: Aleksandra-Mitricheva
---
Created a root category in a physical catalog whose languages are en-US, de-DE, fr-FR, with four seoInfos (storeId B2B-store, languageCode en-US / de-DE / fr-FR / pt-PT, all the same semanticUrl), linked it into the root of the store's virtual catalog (POST /api/catalog/listentrylinks) and reindexed the Category document. Within one poll: Query.childCategories(maxLevel:1) listed it top-level for cultureName en-US, de-DE, fr-FR and pt-PT, and Query.slugInfo(permalink=<slug>) returned this category with entityInfo.languageCode equal to the requested culture in all four, pt-PT included although the catalog declares no pt-PT. The category has no products; childCategories lists it anyway.
