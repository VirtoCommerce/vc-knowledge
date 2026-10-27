---
id: KB-D89CB35F
subject: With the Sales Rep plugin delivered by Module Federation, /company/customer-orders renders the plugin's own customer-orders chunk, so a theme-only build of a vc-frontend sales-rep change does not reach that page
plane: experiential
question: Does a vc-frontend theme build that changes client-app/modules/sales-rep change what the sales-rep orders page renders, or does the page come from the SalesRep module plugin?
questions:
  - text: Why does a PR theme build not show a sales-rep hub fix?
  - text: Which customer-orders chunk loads on /company/customer-orders when the sales-rep plugin is present?
  - text: Can a sales-rep storefront fix be verified with a theme-only PR artifact?
status: active
appliesTo:
  - axis: feature
    value: module-federation-plugin-delivery
  - axis: module
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/customer-orders
  - coordinate: /modules/$(VirtoCommerce.SalesRep)/plugins/vc-frontend/mf-manifest.json
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 956cfff4cb0a606f
    conditions: theme=2.61.0-pr-2609-52e9-52e9649c; plugin:sales-rep=0.2.5; setting:module_federation_enabled=true
    at: 2026-10-27T11:15:27.746Z
    by: session:37167127
    who: Aleksandra-Mitricheva
    note: "distinct: KB-847622A8 covers the 404 when /modules is not routed; this fact is about which code renders once the plugin DOES load: the plugin's chunk, not the theme's bundled copy, so theme PR builds cannot deliver hub fixes."
---
Theme 2.61.0-pr-2609-52e9-52e9649c served locally (frontend-only nginx, backend proxied to the vcptcore_qa platform with SalesRep plugin pluginVersion 0.2.5). The theme's /assets/ holds its own customer-orders chunk (with the PR change, compiled "value-field":"id"), but with /modules proxied the page loaded mf-manifest.json, remoteEntry.js and then customer-orders-*.js from /modules/$(VirtoCommerce.SalesRep)/plugins/vc-frontend/assets/, the plugin's copy, and never requested the theme's chunk. The Created-date filter behaved exactly as on the deployed storefront. A change under vc-frontend client-app/modules/sales-rep therefore takes effect on the hub only through a rebuilt SalesRep module plugin (or a theme with the built-in hub enabled), not through a theme PR artifact alone.
