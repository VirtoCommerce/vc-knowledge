---
id: KB-8FE69FD1
subject: "A Sales Rep plugin manifest that is 404, HTML, or declares an incompatible requiredHostVersion makes the host skip the plugin: no remoteEntry.js request, no hub rail or Corporate Sales reps link, and hub routes show the host 404 page with no Reload offer"
plane: experiential
question: What does the storefront do when the sales-rep plugin manifest mf-manifest.json is unusable (404, HTML body, or requiredHostVersion the host does not satisfy)?
status: active
appliesTo:
  - axis: aspect
    value: plugin-delivery
  - axis: feature
    value: sales-rep-hub
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: GET /modules/$(VirtoCommerce.SalesRep)/plugins/vc-frontend/mf-manifest.json
  - coordinate: /company/dashboard
  - coordinate: /company/sales-reps
evidence:
  - method: observation
    deployment: vcst_qa
    conditions: platform=3.1083.0; theme=2.60.0-alpha.2579; module:VirtoCommerce.SalesRep=3.1014.0; theme:module_federation_enabled=true
    at: 2026-10-15T14:22:23.946Z
    by: session:6ee2ca49
    who: Lenajava1
  - method: observation
    deployment: vcptcore_qa
    build: e8ff6ac5fbd4c99e
    conditions: platform=3.1096.0; theme=vc-frontend-next-3.3.0-pr-7-7bd1; module:VirtoCommerce.SalesRep=3.1023.0-pr-37-31e8; store=B2B-store
    at: 2026-11-11T12:25:48.442Z
    by: session:2411e968
    bodyHash: 85757450adc8
    who: Aleksandra-Mitricheva
    note: "Natural (not injected) occurrence: real SalesRep manifest declares requiredHostVersion ^0.2.9 (pluginVersion 0.2.5, shares @vc-frontend/core 0.2.9); host shared map declares @vc-frontend/core 0.2.5. Passkeys manifest (^0.2.5) loads its remoteEntry.js; SalesRep remoteEntry.js is never requested; /company/my-customers and /company/dashboard render host 404 for a signed-in rep; no console warning is logged about the skip."
---
Client-side interception of /modules/$(VirtoCommerce.SalesRep)/plugins/vc-frontend/mf-manifest.json: answered 404, or 200 text/html with the storefront index.html, or the real manifest with metaData.requiredHostVersion rewritten from ^0.2.3 to ^0.3.0. In all three, for a rep and for a corporate buyer: the home page and a product page render normally, no uncaught error, no remoteEntry.js is requested (version-gate case verified explicitly), the Sales Rep hub rail section and the Corporate 'Sales reps' link are absent from /account, and /company/dashboard, /company/my-customers and /company/sales-reps keep their URL but render the host '404 Page not found' page without a 'Reload page' button. By contrast a manifest that loads but a remoteEntry.js that fails to download shows 'Part of the site could not be loaded' with Reload page.
