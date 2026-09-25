---
id: KB-4DDAE053
subject: /product/{sku} never resolves on the storefront and returns a soft 404 with HTTP 200
plane: experiential
question: Does the storefront PDP resolve at /product/{sku}?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /product/{sku}
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T11:18:16.567Z
    by: session:memimpor
    who: Lenajava1
---
The storefront product route does not resolve a SKU: /product/{sku} renders the client-side 404 Page not found view for any SKU, catalog or seeded. A product page resolves at /product/{product id} or at its SEO slug path. The 404 is soft - the storefront is a single-page app, so the shell returns HTTP 200 and the not-found state is rendered client-side - which means an HTTP status check cannot validate a storefront route; only rendered content can.
