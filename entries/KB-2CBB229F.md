---
id: KB-2CBB229F
subject: PUT /api/stores replaces the whole store, and a null default currency, language or URL breaks the storefront
plane: experiential
question: What happens if a partial body is sent to PUT /api/stores, and which store fields must never be null?
questions:
  - text: Why did prices go blank and links break across the whole shop after a store update?
  - text: Is PUT on the stores API a partial update or a full replacement of the store?
  - text: Which store fields must never be null for the storefront to keep working?
  - text: What is the safe way to change one store setting through the REST API?
concepts:
  - id: store
  - id: rest-api
status: active
appliesTo:
  - axis: surface
    value: rest
anchors:
  - coordinate: PUT /api/stores
  - coordinate: Store.defaultCurrency
  - coordinate: Store.defaultLanguage
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T11:18:43.420Z
    by: session:memimpor
    who: Lenajava1
    build: e7c4faf10f640e7c
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    at: 2026-09-28T14:11:41.410Z
    by: session:p44412
    who: Lenajava1
    note: GET /api/stores/{id}, merge one setting (Catalog.Search.BarcodeSearchFields), PUT whole body -> 204; default currency, language and URL kept.
    build: 452b315ee3a31e8a
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    at: 2026-09-28T18:26:59.533Z
    by: session:p28232
    who: Lenajava1
    note: GET /api/stores/{id}, merged Catalog.Search.BarcodeSearchFields, PUT /api/stores whole body -> 204 three times; default currency, language, URL and catalog kept.
    build: 452b315ee3a31e8a
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T15:55:44.943Z
    by: session:624d35a1
    who: kutasinaelena
    note: Three PUT /api/stores calls, each with the complete body from GET /api/stores/B2B-store and only one setting value changed, returned 204. Afterwards the store record (all fields, 108 settings, payment/shipping, SEO, dynamic properties) was identical to the pre-run snapshot, so a full-body PUT is safe.
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-14T08:08:58.965Z
    by: session:cae69cc8
    who: kutasinaelena
    note: GET /api/stores/New-super, changed only mainFulfillmentCenterId, PUT whole body -> 204; a by-name diff of every other field (125 settings included) against the GET before was empty. Done three times (set, restore, set).
---
A store must carry a default currency, a default language and a Store URL; with any of them null the storefront is broken - prices blank or 0.00, no language fallback target, malformed links - rather than one feature failing. PUT /api/stores replaces the entity: fields omitted from the body are nulled, which is how a four-field body wiped a store's defaults and URLs. A partial body can also be refused with 500, which is a fail-safe, not something to work around. The safe form is GET, merge the one change, then PUT the whole body; success is 204.
