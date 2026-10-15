---
id: KB-E86A5FDE
subject: The Sales Rep storefront plugin manifest is served at /modules/$(VirtoCommerce.SalesRep)/plugins/vc-frontend/mf-manifest.json (literal "$(…)"), not at /modules/VirtoCommerce.SalesRep/…
plane: experiential
question: At what storefront URL is the Sales Rep Module Federation plugin manifest served, and what does it declare?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.salesrep
  - axis: surface
    value: storefront-runtime
anchors:
  - coordinate: GET /modules/$(VirtoCommerce.SalesRep)/plugins/vc-frontend/mf-manifest.json
evidence:
  - method: observation
    deployment: vcst_qa
    build: e00c89f4da0e9c98
    conditions: platform=3.1083.0; module:VirtoCommerce.SalesRep=3.1014.0-pr-13-4fbb; module:VirtoCommerce.XFrontend=3.1007.0; module:VirtoCommerce.Xapi=3.1027.0
    at: 2026-10-15T11:02:21.975Z
    by: session:6ee2ca49
    who: Lenajava1
---
Anonymous GET on the storefront host of /modules/$(VirtoCommerce.SalesRep)/plugins/vc-frontend/mf-manifest.json (the "$(" and ")" are literal characters in the path) returned 200 application/json: id/name "sales-rep", type app, remoteEntry "remoteEntry.js" (type module), ssrRemoteEntry "remoteEntry.ss r.js", pluginVersion 0.2.5, requiredHostVersion ^0.2.3, shared singletons vue 3.5.40, vue-router 4.6.4, vue-i18n. The same path without the "$(…)" wrapper, /modules/VirtoCommerce.SalesRep/plugins/vc-frontend/mf-manifest.json, returned 404. The /modules prefix is routed to the platform by the environment's ingress config. Whether the storefront actually loads the hub from it (GetStorePlugins) was not established in this observation.
