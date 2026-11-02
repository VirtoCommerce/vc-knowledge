---
id: KB-9DB0A0A2
subject: slugInfo for a Page Builder page published only in en-US resolves to that en-US page for de-DE and fr-FR too
plane: experiential
question: What does slugInfo return in another culture for a Page Builder page that exists in en-US only?
questions:
  - text: Does a single-language Page Builder page 404 in other storefront languages?
  - text: Which languageCode does slugInfo report for a page with no translation?
status: active
appliesTo:
  - axis: module
    value: pagebuilder
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.slugInfo
  - coordinate: POST /api/page-builder-pages/grouped
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 023779f193646d5e
    at: 2026-11-02T22:38:22.695Z
    by: session:75d360f6
    who: Aleksandra-Mitricheva
    note: "distinct: KB-08DE22F1 is about two catalog PRODUCTS sharing a slug resolving to different products per culture; this is a single Page Builder PAGE with no translation, which resolves to itself in every culture."
---
2026-11-03, store B2B-store: created ONE Page Builder group (cultureName en-US, Published, no organization/user-group restriction) at /agent-test-single-language-en with no sibling in any other culture. As a signed-in buyer, Query.slugInfo(storeId, cultureName, permalink) returned the same entityInfo (objectType Pages, languageCode en-US) for cultureName en-US, de-DE and fr-FR. So the API falls back to the default-language page rather than returning nothing; whether the storefront then renders it under a /de/ prefix was not checked here.
