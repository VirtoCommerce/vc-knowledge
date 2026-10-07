---
id: KB-3EBB904D
subject: On a Long-link-type store a storefront hard load resolves category, parent/child category, category/product and content-page paths through one pageContext call whose permalink has no leading slash
plane: experiential
question: How does the storefront resolve a hard-loaded SEO URL, and what does pageContext return for category, nested category, product and content page paths?
status: active
appliesTo:
  - axis: identity
    value: anonymous
  - axis: seolinktype
    value: long
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: Query.pageContext
  - coordinate: /{category}/{subcategory}
  - coordinate: /{category}/{product}
  - coordinate: POST /api/seo/broken-links/search
evidence:
  - method: observation
    deployment: vcptcore_dev
    at: 2026-10-05T21:22:54.848Z
    by: session:f9a8159e
---
2026-10-05, theme 2.58.0, Platform 3.1077.0-alpha.13406, Catalog 3.1049.0-pr-910-04a5, Seo 3.1006.0-alpha.101 (PR builds of the Debug SEO rework), store B2B-store with Stores.SeoLinksType Long, anonymous visitor. Each hard load sent exactly one GetPageContext operation (Query.pageContext) with variables {domain, userId, permalink}, the permalink being the URL path without its leading slash. /seed-books returned entityInfo objectType Category; /seed-books/seed-books-fiction returned Category with outline parent/child; /seed-electronics/seed-cable-ties-100pack returned CatalogProduct; /contacts returned ContentFile whose semanticUrl is '/contacts' (the record keeps the slash, the request does not); /seed-test-fixtures, a slug shared by four categories, returned one Category. Every page rendered its object (category listing, nested listing, product page, content page), never the 404 view, with no console error and every GraphQL call under 1.4 s. Afterwards POST /api/seo/broken-links/search (keyword + storeId) found no broken-link record for any of the five permalinks: a resolved hard load records nothing.
