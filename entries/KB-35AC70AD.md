---
id: KB-35AC70AD
subject: SalesRep plugin 3.1023.0 imports the dashboard layout API (statBlocks, registerBlock, useLayout, LayoutSurface and 14 more) from the shared @vc-frontend/core singleton; a theme built without that API but declaring the same core version 0.2.11 is accepted by module federation and the plugin init throws, so every hub route shows the plugin placeholder
plane: experiential
question: Why does the SalesRep storefront plugin load all its assets but never register on a theme alpha build?
questions:
  - text: What does the sales-rep remote throw when loaded with loadRemote on theme 2.62.0-alpha.2642?
  - text: Which @vc-frontend/core exports does the SalesRep plugin 3.1023.0 require from the host?
  - text: Does module federation reject a host whose @vc-frontend/core lacks exports the plugin needs?
  - text: Why is there no console error when the Sales Rep hub shows 'Part of the site could not be loaded'?
status: active
appliesTo:
  - axis: feature
    value: sales-rep-hub-plugin
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/dashboard
  - coordinate: /company/my-customers
  - coordinate: /modules/$(VirtoCommerce.SalesRep)/plugins/vc-frontend/mf-manifest.json
  - coordinate: Query.store.plugins
evidence:
  - method: observation
    deployment: vcst_qa
    build: 4605db2ed710c357
    conditions: platform=3.1097.0; theme=2.62.0-alpha.2642; module:VirtoCommerce.SalesRep=3.1023.0; module:VirtoCommerce.XFrontend=3.1010.0; setting:SalesRep.Enabled=true; store=B2B-store
    defect: suspected
    at: 2026-11-11T17:03:20.560Z
    by: session:3b333ac1
    who: kutasinaelena
---
Signed in as a sales rep, /company/dashboard showed the host plugin-route placeholder with zero console errors; GetStorePlugins returned sales-rep 3.1023.0 and all its remote files returned 200. Host and plugin mf-manifest.json both declare @vc-frontend/core 0.2.11 (singleton, requiredVersion ^0.2.0), so the runtime binds the plugin to the host's lib. Calling the host MF instance's loadRemote('sales-rep/plugin') (read-only debug) threw "TypeError: B is not a function or its return value is not iterable" from the plugin index chunk, where B is statBlocks imported from @vc-frontend/core. The served core singleton (CORE_VERSION 0.2.11) lacks 18 exports the plugin imports: LayoutSurface, LayoutWidget, MIN_ROWS, buildStatCards, buildStatisticsWindows, formatSignedPercent, formatStatCount, formatStatMoney, knownHiddenTabs, registerBlock, statBlocks, statDataNeeds, toggleTabRule, unregisterBlock, useBlockChrome, useLayout, useStatDataNeeds, visibleTabRules. In source these were added to vc-frontend client-app/core-api/index.ts by the VCST-6078 dashboard change (merge 041168f1) without changing core-api/package.json version (0.2.11 before and after); the plugin moved to them in vc-module-sales-rep VCST-6078 (#24), shipped in 3.1023.0. The Passkeys plugin on the same page loads fine (it needs none of these exports). Implication: a plugin/theme pairing that differs on this API is not rejected by version negotiation; it fails silently at init.
