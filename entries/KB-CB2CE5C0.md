---
id: KB-CB2CE5C0
subject: With store setting SalesRep.Enabled=false the storefront still receives the sales-rep plugin from GetStorePlugins but fetches nothing from its plugin folder, renders no hub rail and no Corporate 'Sales reps' link, and shows the host 404 on /company/dashboard, /company/my-customers and /company/sales-reps
plane: experiential
question: What does the storefront do for the Sales Rep Hub plugin when the store setting SalesRep.Enabled is off?
status: active
appliesTo:
  - axis: feature
    value: sales-rep-hub
  - axis: setting
    value: salesrep.enabled:false
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: Query.store.plugins
  - coordinate: /company/dashboard
  - coordinate: /company/my-customers
  - coordinate: /company/sales-reps
  - coordinate: /account/dashboard
evidence:
  - method: observation
    deployment: vcst_qa
    build: 7ec352df61232ca8
    conditions: platform=3.1083.0; module:VirtoCommerce.SalesRep=3.1014.0; theme=2.60.0-alpha.2579; setting:SalesRep.Enabled=false; store=B2B-store
    at: 2026-10-15T13:40:19.535Z
    by: session:6ee2ca49
    who: Lenajava1
---
B2B-store with SalesRep.Enabled=false (GetPageContext store.settings.modules VirtoCommerce.SalesRep -> SalesRep.Enabled=false). Signed in, fresh session, as a sales rep holding the Sales Representative role (sales-rep:access in permissions) and separately as an organization buyer; full document load of /account. The GetStorePlugins query still returned the sales-rep entry (version 3.1014.0, entry /modules/$(VirtoCommerce.SalesRep)/plugins/vc-frontend/remoteEntry.js, contributions.when = {setting: SalesRep.Enabled, module: VirtoCommerce.SalesRep}), yet the network log held ZERO requests under /modules/ for either user, and the account rail showed only Purchasing / Marketing / Corporate (Company info, Company members) / User: no 'Sales Rep hub' section, no 'Sales reps' link. Cold loads of /company/dashboard and /company/my-customers (rep) and /company/sales-reps (rep, buyer, and anonymous after sign-out) kept the URL and rendered the host '404 Page not found' page (after a Builder.io CMS lookup for the path), with no hub content and no plugin fetch. The first observation, about 2.5 min after the setting change, already showed the off state; no reload or polling was needed. The when: gate is evaluated client-side from the store settings before the plugin remoteEntry is fetched.
