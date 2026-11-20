---
id: KB-E6C3DA15
subject: Admin store Settings blade 'Reset to default' + Save writes the default value as a store override (itHasValues stays true) instead of removing the override
plane: experiential
question: Does 'Reset to default' in the Admin store Settings blade make a store setting inherit its default again?
questions:
  - text: After Reset to default and Save, does GET /api/stores/{id} report itHasValues false for the setting?
  - text: How do I remove a store-level setting override from the Admin UI?
  - text: Why does a store setting still show itHasValues true after Reset to default?
status: active
appliesTo:
  - axis: objecttype
    value: store
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: PUT /api/stores
  - coordinate: GET /api/stores/{id}
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 6cf3e54e8335fd8d
    conditions: store=B2B-store; module:VirtoCommerce.ProductComparison=3.1000.0-pr-1-7403
    defect: suspected
    at: 2026-11-20T11:07:25.027Z
    by: session:fcdb91ae
    who: kutasinaelena
---
B2B-store, ProductComparison.MaxItems (Integer, defaultValue 5). Set to 4 in Admin > Stores > store > Settings widget > Product Comparison group, Save (PUT /api/stores 204) -> GET /api/stores/B2B-store value 4, itHasValues true. Refresh, click 'Reset to default: 5' on the row, Save -> GET reads value 5 but itHasValues still true. After a blade Refresh the row no longer shows the Modified chip or the Reset to default button, so the UI looks inherited while the store still holds an explicit row equal to the default. The same was seen for the Boolean settings ProductComparison.Enabled and ProductComparison.AnonymousListEnabled. Consequence: a later change of the module default will not reach this store; Reset to default cannot return a store to inheritance.
