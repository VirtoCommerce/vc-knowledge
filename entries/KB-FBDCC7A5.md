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
  - method: observation
    deployment: vcptcore_dev
    at: 2026-10-05T20:11:07.882Z
    by: session:f9a8159e
    note: "how: static analysis of the deployed storefront bundle"
    build: 8db313999cee1432
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_qa
    build: 706d29d69470a72c
    conditions: theme=2.61.0-pr-2576-211e3e15; store=B2B-store
    at: 2026-10-23T13:02:05.715Z
    by: session:0204f22f
    bodyHash: 0c724d4fc85c
    who: Aleksandra-Mitricheva
    contradicts: true
    contested: virtocommerce.fileexperienceapi 3.1008.0 vs 3.1006.0-alpha.96; virtocommerce.marketingexperienceapi 3.1004.0 vs 3.1004.0-alpha.103; virtocommerce.pages 3.1011.0-pr-23-4bcd vs 3.1010.0-alpha.82; virtocommerce.profileexperienceapimodule 3.1021.0 vs 3.1020.0-alpha.383; …
    note: "Rendered head (post-hydration DOM, read by a Playwright script) on theme 2.61.0-pr-2576: PDP og:url = https://<store url>/alcoholic-drinks/beer/<product> (the page's own absolute URL; with /fr/ prefix on /fr/...), category og:url = https://<store url>/alcoholic-drinks/beer. So on this build the rendered og:url IS the page URL. The entry's static read was on theme 2.58.0; this may be a fix between builds rather than a wrong reading."
---
Read from the deployed storefront bundle (theme 2.58.0, static assets, not the DOM): the product-route chunk computes seoUrl as `${window.location.host}\${product.value?.seoInfo?.semanticUrl}` and the category chunk as `${window.location.host}\${currentCategory.value?.seoInfo?.semanticUrl` (note the backslash before the dollar and the missing closing brace); both feed useSeoMeta ogUrl. In a template literal the escaped dollar is not an interpolation, so the value is the host followed by literal text, without scheme or the resolved path. The same pattern exists in the vc-frontend 2.59.0 source (useCategorySeo.ts, product.vue). The rendered og:url in the DOM was not read (the head is client-set; reading it needs a script evaluation), so treat the exact rendered string as unverified. The static index.html carries no canonical link and no description; its title is the generic one and the real title is set client-side (a product page title is '<store name> <SEO page title>').
