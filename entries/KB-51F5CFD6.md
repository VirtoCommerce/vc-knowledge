---
id: KB-51F5CFD6
subject: A slow Sales Rep remoteEntry.js shows "Loading page…", then a slow notice with Reload page at about 8 s; a failed one shows "Part of the site could not be loaded…" with Reload page, which recovers. Both are inside an output (role=status) element
plane: experiential
question: What does a rep see on a hub deep link when the Sales Rep plugin remoteEntry.js is slow or fails to download?
status: active
appliesTo:
  - axis: feature
    value: sales-rep-hub-plugin
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: GET /modules/$(VirtoCommerce.SalesRep)/plugins/vc-frontend/remoteEntry.js
  - coordinate: /company/dashboard
evidence:
  - method: observation
    deployment: vcst_qa
    build: 7ec352df61232ca8
    conditions: platform=3.1083.0; module:VirtoCommerce.SalesRep=3.1014.0-pr-13-4fbb; theme=2.60.0-alpha.2579; plugin:sales-rep=0.2.5; method=client-side request interception; browser=firefox
    at: 2026-10-15T13:15:27.028Z
    by: session:6ee2ca49
    who: Lenajava1
---
On a cold /company/dashboard as a rep. remoteEntry held 15 s: "Loading page…" at about 1.3 s; at about 8.3 s "This page is taking longer than usual to load. You can keep waiting or reload the page." plus a Reload page button; the hub appears when the entry arrives (about 16.5 s). remoteEntry aborted once: at about 1.5 s "Part of the site could not be loaded. Please reload the page to continue." plus Reload page. Activating it does a full reload and recovers the hub in about 1.4-1.8 s. A11y: both messages sit in one <output> element (implicit role=status, polite, no accessible name). It is the same DOM node from Loading to the slow notice, so the text changes inside an existing live region. Reload page is reachable by Tab (about 53-55 tabs from the top, :focus-visible true) and Enter activates it. Console shows only a Firefox "Loading failed for the module" warning on abort.
