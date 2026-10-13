---
id: KB-66B6CABA
subject: Turning the store setting Stores.IsSpa off has no effect on vc-frontend navigation, which stays client-side
plane: experiential
question: Does switching off 'Single-page application (SPA)' (Stores.IsSpa) on a store make the vc-frontend storefront do full page loads?
status: active
appliesTo:
  - axis: module
    value: store
  - axis: surface
    value: admin-spa
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: GET /api/stores/{id}
  - coordinate: Stores.IsSpa
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T16:20:23.734Z
    by: session:624d35a1
    who: kutasinaelena
---
On B2B-store, Stores.IsSpa was set to false in Admin (Store > Settings, group Store > General, label 'Single-page application (SPA)', OK, then store Save). GET /api/stores/{id} read it back as false. On the storefront, clicking a catalog menu item and then a product changed the route without any new document request: only lazy JS chunks and GraphQL calls were fetched, and both pages rendered normally. The setting saves but does not change storefront behaviour. Restored to true afterwards.
