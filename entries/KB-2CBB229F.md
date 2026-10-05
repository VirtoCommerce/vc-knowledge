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
  - method: observation
    deployment: vcst
    at: 2026-09-28T14:11:41.410Z
    by: session:p44412
    who: Lenajava1
    note: GET /api/stores/{id}, merge one setting (Catalog.Search.BarcodeSearchFields), PUT whole body -> 204; default currency, language and URL kept.
  - method: observation
    deployment: vcst
    at: 2026-09-28T18:26:59.533Z
    by: session:p28232
    who: Lenajava1
    note: GET /api/stores/{id}, merged Catalog.Search.BarcodeSearchFields, PUT /api/stores whole body -> 204 three times; default currency, language, URL and catalog kept.
---
A store must carry a default currency, a default language and a Store URL; with any of them null the storefront is broken - prices blank or 0.00, no language fallback target, malformed links - rather than one feature failing. PUT /api/stores replaces the entity: fields omitted from the body are nulled, which is how a four-field body wiped a store's defaults and URLs. A partial body can also be refused with 500, which is a fail-safe, not something to work around. The safe form is GET, merge the one change, then PUT the whole body; success is 204.
