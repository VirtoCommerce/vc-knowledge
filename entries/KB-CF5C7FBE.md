---
id: KB-CF5C7FBE
subject: When the Sales Rep plugin manifest fails (404, HTML body with 200, or requiredHostVersion mismatch) every hub route and /company/sales-reps renders the host 404 "Page not found" with no Reload offer, and the Corporate "Sales reps" link and the "Sales Rep hub" rail are withdrawn
plane: experiential
question: What does the storefront show on Sales Rep hub routes when the plugin mf-manifest.json cannot be used (404, HTML instead of JSON, or host version mismatch)?
status: active
appliesTo:
  - axis: layer
    value: plugin-delivery
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/dashboard
  - coordinate: /company/my-customers
  - coordinate: /company/sales-reps
  - coordinate: GET /modules/$(VirtoCommerce.SalesRep)/plugins/vc-frontend/mf-manifest.json
evidence:
  - method: observation
    deployment: vcst_qa
    build: e00c89f4da0e9c98
    conditions: platform=3.1083.0; module:VirtoCommerce.SalesRep=3.1014.0-pr-13-4fbb; module:VirtoCommerce.XFrontend=3.1007.0; theme=2.60.0-alpha.2579; fault=client-side request interception
    at: 2026-10-15T11:35:42.813Z
    by: session:6ee2ca49
    who: Lenajava1
---
Client-side interception of GET /modules/$(VirtoCommerce.SalesRep)/plugins/vc-frontend/mf-manifest.json, signed in as a sales rep and as an org buyer. (a) 404: /company/dashboard, /company/my-customers and /company/sales-reps render the host 404 "Page not found / An error occurred. Should we take you back to home page?" inside the account layout; the "Sales Rep hub" rail section and the Corporate "Sales reps" link are absent; one console error (the 404 resource). (b) 200 text/html (the storefront index document, as a missing /modules ingress would return): the same 404 page and withdrawn link, and ZERO console errors. (c) manifest JSON rewritten to metaData.requiredHostVersion "^0.3.0" (real value "^0.2.3"): remoteEntry.js and no plugin chunk is ever requested (fail-closed), same 404 page, no console message. In all three the home page and a product page boot and stay usable, no uncaught error, no Reload offer. Browser telemetry (Application Insights) records only the manifest fetch as a RemoteDependency — success:false for 404, success:TRUE for the HTML case — plus a Pageview named after the plugin route (SalesRepDashboard / SalesRepMyCustomers) although the user sees a 404; no exception or custom event is sent. Contrast: when the manifest is fine but remoteEntry.js itself fails to download, the route shows "Part of the site could not be loaded. Please reload the page to continue." with a Reload page button, and clicking it with the network restored loads the hub.
