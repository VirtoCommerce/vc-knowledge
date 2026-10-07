---
id: KB-4DDC303D
subject: POST /api/stores accepts settings:[{name:"Stores.SeoLinksType",value:"Long"}] in the create body and the store honours it; Seo.BrokenLinkDetection.Enabled reads value null with defaultValue true when unset
plane: experiential
question: Can a store be created with SEO links = Long through POST /api/stores, and how do I read whether broken-link detection is on?
status: active
appliesTo:
  - axis: module
    value: seo
  - axis: module
    value: store
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/stores
  - coordinate: DELETE /api/stores
  - coordinate: GET /api/platform/settings/Seo.BrokenLinkDetection.Enabled
  - coordinate: DELETE /api/seo/broken-links
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-07T19:38:50.991Z
    by: session:ed63bd34
    who: kutasinaelena
---
Store 3.1009.0 / Seo 3.1006.0-pr-22. POST /api/stores {id, name, catalog, storeState:"Open", defaultLanguage, languages, defaultCurrency, currencies, settings:[{name:"Stores.SeoLinksType", value:"Long"}]} returns 200; GET /api/stores/{id} lists Stores.SeoLinksType value Long (default Collapsed), and explain on that store produced Long-link PermalinkMismatch paths (parent/child). DELETE /api/stores?ids=<id> returns 204 and a later GET /api/stores/{id} answers 200 with an empty body (not 404). GET /api/platform/settings/Seo.BrokenLinkDetection.Enabled returned value null, defaultValue true, and detection was demonstrably on (an unresolved slugInfo created a broken-link record within 10 s), so a guard must use value ?? defaultValue. Broken-link records can be removed with DELETE /api/seo/broken-links?ids=<id>; POST /api/seo/broken-links/search keyword matches a permalink prefix.
