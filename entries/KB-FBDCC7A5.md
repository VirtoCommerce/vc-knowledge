---
id: KB-FBDCC7A5
subject: On the deployed storefront theme the product and category pages build og:url from an escaped template literal, so the string is not the page URL
plane: experiential
question: What does the product or category page put in og:url?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: theme
    value: 2.58.0
anchors:
  - coordinate: Query.slugInfo
  - coordinate: Query.product
evidence:
  - method: static analysis of the deployed storefront bundle
    deployment: vcptcore_dev
    at: 2026-10-05T20:11:07.882Z
    by: session:f9a8159e
---
Read from the deployed storefront bundle (theme 2.58.0, static assets, not the DOM): the product-route chunk computes seoUrl as `${window.location.host}\${product.value?.seoInfo?.semanticUrl}` and the category chunk as `${window.location.host}\${currentCategory.value?.seoInfo?.semanticUrl` (note the backslash before the dollar and the missing closing brace); both feed useSeoMeta ogUrl. In a template literal the escaped dollar is not an interpolation, so the value is the host followed by literal text, without scheme or the resolved path. The same pattern exists in the vc-frontend 2.59.0 source (useCategorySeo.ts, product.vue). The rendered og:url in the DOM was not read (the head is client-set; reading it needs a script evaluation), so treat the exact rendered string as unverified. The static index.html carries no canonical link and no description; its title is the generic one and the real title is set client-side (a product page title is '<store name> <SEO page title>').
