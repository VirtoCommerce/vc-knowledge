---
id: KB-2BA242BB
subject: On theme PR 2604 build, the language selector keeps the slug-owning culture in previousCultureSlug, so a de-DE-only product slug resolves on every en-GB/en-US hop (no 404)
plane: experiential
question: Does switching language twice on a product whose SEO URL exists in one language only still land on 404 with the PR 2604 fix?
questions:
  - text: What does sessionStorage previousCultureSlug hold after several language switches on a fallback-shown product page?
  - text: Which cultureName does GetSlugInfo use on an en-US page reached through the language fallback?
  - text: Is VCST-6501 fixed by vc-frontend PR 2604?
status: active
appliesTo:
  - axis: feature
    value: localization
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /products-with-options/dreamy-cakes/vintage-wedding-cake
  - coordinate: Query.slugInfo
evidence:
  - method: observation
    deployment: vcst_qa
    build: e282fb2cf601d5b6
    conditions: theme=2.61.0-pr-2604-149a; store=B2B-store; role=guest; browser=chromium; storefront=local container, backend vcst_qa
    at: 2026-10-26T09:51:37.781Z
    by: session:1c2d4240
    who: Lenajava1
---
Guest, Chromium, store B2B-store, local storefront container running theme 2.61.0-pr-2604-149a with /graphql proxied to the vcst_qa backend. Product c94d730a-a1e3-41d3-bba6-8e9eb9fe45e8 has an SEO URL only in de-DE. Start /product/{id} (en-US) -> Deutsch -> English (en-GB) -> English (en-US): every hop rendered the PDP, 4/4 fresh sessions. sessionStorage.previousCultureSlug after each hop: {en-US, product/{id}} -> {de-DE, products-with-options/dreamy-cakes/vintage-wedding-cake} -> {de-DE, same slug} (unchanged on the en-US hop). On the final /products-with-options/dreamy-cakes/vintage-wedding-cake page (html lang en-US) GetSlugInfo is sent with cultureName de-DE and returns the CatalogProduct entity. Also held for a single hop (de -> en-US), a longer chain (-> en-GB -> Deutsch, which lands natively on /de/...), and a reload of the final en-US page. Before each switch's full reload, a transient GetSlugInfo is sent with the OLD locale prefix inside permalink (e.g. en-GB/products-with-options/...) under the new culture and returns null; it is superseded by the reload and nothing visible depends on it.
