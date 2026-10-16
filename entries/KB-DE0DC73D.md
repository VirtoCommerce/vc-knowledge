---
id: KB-DE0DC73D
subject: When the Sales Rep plugin manifest is unusable (404, HTML body, or requiredHostVersion not satisfied), the storefront withdraws the hub and the Corporate Sales reps link and shows the host 404 on hub routes, with no Reload offer
plane: experiential
question: What does the storefront do when the Sales Rep plugin mf-manifest.json returns 404, returns HTML, or declares a requiredHostVersion the host does not satisfy?
status: active
appliesTo:
  - axis: feature
    value: sales-rep-hub-plugin
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: GET /modules/$(VirtoCommerce.SalesRep)/plugins/vc-frontend/mf-manifest.json
  - coordinate: /company/dashboard
  - coordinate: /company/sales-reps
evidence:
  - method: observation
    deployment: vcst_qa
    build: 7ec352df61232ca8
    conditions: platform=3.1083.0; module:VirtoCommerce.SalesRep=3.1014.0-pr-13-4fbb; theme=2.60.0-alpha.2579; plugin:sales-rep=0.2.5; method=client-side request interception
    at: 2026-10-15T13:15:26.932Z
    by: session:6ee2ca49
    who: Lenajava1
---
Faults were injected client-side on mf-manifest.json under /modules/$(VirtoCommerce.SalesRep)/plugins/vc-frontend/, as a rep and as a plain buyer. In all three cases (404; 200 with the storefront index.html; real manifest with metaData.requiredHostVersion rewritten from ^0.2.3 to ^0.3.0), home and PDP keep working. The Sales Rep hub rail section and the Corporate "Sales reps" link are absent. /company/dashboard, /company/my-customers and /company/sales-reps render the host 404 "Page not found", with no Reload offer. In the version-gate case remoteEntry.js is never requested (fail-closed). Console: no error in any case (Firefox does not log the 404). App Insights sends a RemoteDependency for the manifest (success=false rc=404 in the 404 case; success=true rc=200 in the HTML case) and a Pageview named "SalesReps" while the user sees a 404. No exception telemetry is sent.
