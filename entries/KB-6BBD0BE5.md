---
id: KB-6BBD0BE5
subject: Anonymous Query.products without currencyCode fails with INVALID_OPERATION on a store whose default currency is not inferred
plane: experiential
question: Does anonymous xAPI products(storeId) work without currencyCode?
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.products
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T18:40:34.957Z
    by: session:624d35a1
    who: kutasinaelena
---
On vcptcore_regression (XCatalog 3.1023.0, B2B-store), anonymous products(storeId:"B2B-store", first:2) with no currencyCode and no cultureName returned HTTP 200 with errors[0] "Error trying to resolve field 'products'." extensions.code INVALID_OPERATION and data.products null. Adding currencyCode:"USD" (with or without cultureName) returned totalCount 344. currencyCode:"GBP" also worked but price.actual was 0. A case or runner that calls products(storeId, query) without currencyCode therefore fails before any search behaviour is exercised; pass {{CURRENCY_CODE}}.
