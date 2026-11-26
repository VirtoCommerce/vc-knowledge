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
  - method: observation
    deployment: vcptcore_qa
    build: 36b0ead443a99f6c
    conditions: theme=2.63.0-pr-2635-8318; module:VirtoCommerce.SalesRep=3.1026.0-pr-38-5280; setting:SalesRep.Enabled=false; store=B2B-store; role=Sales Representative
    at: 2026-11-23T23:43:28.733Z
    by: session:2d819cd0
    bodyHash: f2d4d16c3691
    who: kutasinaelena
    note: "The off state was already in effect at the first fresh sign-in, about 35 s after the store PUT. GetPageContext returned SalesRep.Enabled=false. GetStorePlugins still listed sales-rep (when: SalesRep.Enabled), and nothing was fetched from /modules/$(VirtoCommerce.SalesRep)/. There was no Sales Rep Hub section in the account rail. /company/my-customers and /company/my-customers/<orgGuid> both rendered the host 404 with no Sales statistics block. After the setting was restored to true and the user signed in fresh, the plugin loaded and the profile rendered the Sales statistics block."
  - method: observation
    deployment: vcst_qa
    build: c67268c734322d94
    conditions: theme=2.63.0-alpha.2681; setting:SalesRep.Enabled=false; store=B2B-store; role=Sales Representative
    at: 2026-11-26T14:50:01.836Z
    by: session:d2ee4088
    bodyHash: f2d4d16c3691
    who: Lenajava1
    note: "Observed about 2 minutes after the store PUT, in a fresh password sign-in as a Sales Representative serving 12 orgs. /account/dashboard: the rail had no Sales Rep Hub section and no Corporate 'Sales reps' link. No request was made under /modules/$(VirtoCommerce.SalesRep)/; the Passkeys and ProductComparison plugins did load. A direct load of /company/my-customers kept the URL and rendered the host '404 Page not found', with no console errors. Control: SalesRep.Enabled was restored to true and the same account signed in fresh. The rail then showed Sales Rep Hub (Dashboard, Reporting, My customers 12, Tasks, My activity) plus Corporate 'Sales reps', and /company/my-customers rendered the customers table."
  - method: observation
    deployment: vcst_qa
    build: c67268c734322d94
    conditions: theme=2.63.0-alpha.2681; setting:SalesRep.Enabled=false; store=B2B-store; role=Sales Representative
    at: 2026-11-26T15:04:40.446Z
    by: session:d2ee4088
    bodyHash: f2d4d16c3691
    who: Lenajava1
    note: "Fresh password sign-in about 40 s after the store PUT. Direct load of /company/dashboard kept the URL and rendered the host '404 Page not found' with no hub widgets; no request under /modules/$(VirtoCommerce.SalesRep)/ (Passkeys and ProductComparison plugins loaded); console 0 errors. /account/dashboard rail had no Sales Rep Hub section and no Corporate 'Sales reps' link; only the buyer dashboard widgets (Orders placed tiles, Sales Statistics, My recent orders). Control after restoring true and signing in fresh: /company/dashboard rendered H1 Dashboard with My customers, Top sellers and Tasks & due dates, and the rail showed Sales Rep Hub plus Corporate 'Sales reps'."
  - method: observation
    deployment: vcst_qa
    build: 56f07d69d90abb81
    conditions: theme=2.63.0-alpha.2681; setting:SalesRep.Enabled=false; store=B2B-store; role=Sales Representative; signin=password
    at: 2026-11-26T15:35:06.634Z
    by: session:d2ee4088
    bodyHash: f2d4d16c3691
    who: Lenajava1
    note: "Off state was in effect at the first home reload, about 10 s after the store PUT; two consecutive loads agreed. Baseline with the setting on, same rep: rail had Sales Rep Hub plus Corporate 'Sales reps', and about 20 requests under /modules/$(VirtoCommerce.SalesRep)/plugins/vc-frontend/ (mf-manifest.json, remoteEntry.js, assets). Off, rep and org buyer: 0 requests under that folder (the mf-manifest.json was not fetched either), no hub rail, no Corporate 'Sales reps' link. Direct loads of /company/dashboard and /company/sales-reps rendered the host 404. /company/sales-reps held the URL for at least 8 s, then a later read showed /account/dashboard with no new document load (trigger not isolated). No console errors and no /graphql 4xx/5xx."
---
B2B-store with SalesRep.Enabled=false (GetPageContext store.settings.modules VirtoCommerce.SalesRep -> SalesRep.Enabled=false). Signed in, fresh session, as a sales rep holding the Sales Representative role (sales-rep:access in permissions) and separately as an organization buyer; full document load of /account. The GetStorePlugins query still returned the sales-rep entry (version 3.1014.0, entry /modules/$(VirtoCommerce.SalesRep)/plugins/vc-frontend/remoteEntry.js, contributions.when = {setting: SalesRep.Enabled, module: VirtoCommerce.SalesRep}), yet the network log held ZERO requests under /modules/ for either user, and the account rail showed only Purchasing / Marketing / Corporate (Company info, Company members) / User: no 'Sales Rep hub' section, no 'Sales reps' link. Cold loads of /company/dashboard and /company/my-customers (rep) and /company/sales-reps (rep, buyer, and anonymous after sign-out) kept the URL and rendered the host '404 Page not found' page (after a Builder.io CMS lookup for the path), with no hub content and no plugin fetch. The first observation, about 2.5 min after the setting change, already showed the off state; no reload or polling was needed. The when: gate is evaluated client-side from the store settings before the plugin remoteEntry is fetched.
