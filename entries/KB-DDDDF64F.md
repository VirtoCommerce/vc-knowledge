---
id: KB-DDDDF64F
subject: Cloudflare keeps a separate cached remoteEntry.js for requests with an Origin header, so the browser's plugin loader gets the previous Sales Rep build while a plain curl gets the new one
plane: experiential
question: Why does a browser keep loading the old Sales Rep plugin build after a redeploy when curl shows the new remoteEntry.js?
questions:
  - text: How do I check from the command line whether the CDN serves the new Sales Rep plugin to browsers?
  - text: Why does clearing the browser cache not fix the stale Sales Rep hub after a redeploy?
  - text: Does the CDN cache remoteEntry.js differently for CORS requests?
status: active
appliesTo:
  - axis: feature
    value: sales-rep-hub-plugin
  - axis: layer
    value: cdn
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /modules/$(VirtoCommerce.SalesRep)/plugins/vc-frontend/remoteEntry.js
  - coordinate: /company/dashboard
evidence:
  - method: observation
    deployment: vcst_qa
    build: 33215a89701bbcde
    conditions: module:VirtoCommerce.SalesRep=3.1018.0-pr-33-ff13; theme=2.62.0-pr-2637-02f2-02f26436; cdn=cloudflare; browser=edge
    defect: VCST-6676
    at: 2026-11-05T15:39:33.172Z
    by: session:ffa9e791
    who: Lenajava1
    note: "distinct: KB-4358B2EC says the CDN can hold the stale entry and KB-C2202F86 says the browser cache can; this records that the CDN keys CORS requests (with Origin) to a separate cached object, which is why curl without Origin shows the new build while browsers get the old one, and how to probe it."
  - method: observation
    deployment: vcst_qa
    build: 33215a89701bbcde
    conditions: module:VirtoCommerce.SalesRep=3.1018.0-pr-33-ff13; module:VirtoCommerce.TaskManagement=3.1007.0-pr-33-14ff; cdn=cloudflare; browser=chromium
    at: 2026-11-05T16:02:02.379Z
    by: session:5b90a3e1
    bodyHash: ebfa79cc4ca3
    who: Lenajava1
    note: "16:00Z same day, Chrome lane: curl with Origin got HIT last-modified 12:23:20 (age 4715), without Origin 14:46:38. In the browser the dashboard and my-customers chunks 404'd, /company/dashboard showed 'Loading page…' + 'Part of the site could not be loaded' and ended on /account/dashboard, while /company/tasks still rendered on the 12:23 bundle."
  - method: observation
    deployment: vcst_qa
    build: 33215a89701bbcde
    conditions: module:VirtoCommerce.SalesRep=3.1018.0-pr-33-ff13; module:VirtoCommerce.TaskManagement=3.1007.0-pr-33-14ff; theme=2.62.0-pr-2637-02f2-02f26436; cdn=cloudflare; browser=chromium
    at: 2026-11-05T16:23:11.103Z
    by: session:5b90a3e1
    bodyHash: ebfa79cc4ca3
    who: Lenajava1
    note: "16:22Z same day, Chrome lane: CDP Network.clearBrowserCache then sign-in did not help: the browser's remoteEntry.js came back cf-cache-status HIT, last-modified 12:23:20, age 6038 (virtualExposes-CdDcSffn); dashboard-DbFRW9mH.js, settings-BNBTxuyO.js and useSalesRepOrderStatistics-DziWwS2G.js 404, /company/dashboard stayed on 'Loading page…' + 'Part of the site could not be loaded' after one reload. curl with Origin + Sec-Fetch-Mode cors (any Accept-Encoding) also got 12:23:20 age ~6080; without Origin 14:46:38 (virtualExposes-DgEd6DeA). Confirms point (2): a browser-cache clear only helps once the CDN CORS copy expires (~2.3 h left at max-age 14400) or is purged."
  - method: observation
    deployment: vcptcore_qa
    build: 9ad05b08a6408392
    conditions: module:VirtoCommerce.SalesRep=3.1020.0-pr-35-08fd; theme=2.62.0-alpha.2628; cdn=cloudflare; browser=chromium
    at: 2026-11-09T13:13:54.539Z
    by: session:70c29718
    bodyHash: ebfa79cc4ca3
    who: Aleksandra-Mitricheva
    note: "2026-11-09 13:12Z, fresh isolated Chrome context: browser remoteEntry.js = cf HIT, last-modified 2026-11-03 21:09:08, age 5843, virtualExposes-CtHu1o7w (origin 404). curl with only an Origin header reproduces the stale HIT; without Origin = BYPASS, last-modified 2026-11-09 09:53:00, virtualExposes-CxH9eB4b. customer-orders-ClgHjlsu.js + 5 more chunks 404; /company/customer-orders stays on 'Loading page…' with 'Part of the site could not be loaded'."
  - method: observation
    deployment: vcptcore_qa
    build: e8ff6ac5fbd4c99e
    conditions: module:VirtoCommerce.SalesRep=3.1023.0-pr-37-31e8; theme=2.62.0-alpha.2643; cdn=cloudflare; browser=chromium
    at: 2026-11-11T12:41:46.511Z
    by: session:2411e968
    bodyHash: ebfa79cc4ca3
    defect: VCST-6676
    who: Aleksandra-Mitricheva
    note: "2026-11-11 12:39Z, fresh isolated Chrome context: loader's remoteEntry.js = cf HIT, age 6996, last-modified 2026-11-04 14:22:44, imports index-BrOa4hLN / virtualExposes-BMpwOUpz; my-customers-BL3s5j4U.js, link-my-customers-CdzpMlnX.js + 7 more chunks 404; /company/my-customers stays on 'Loading page…'. Same URL with ?v=8DF3FE85C18EE00 (cache-key bust) = MISS, last-modified 2026-11-11 11:38:52, imports virtualExposes-CCH71ksq (matches mf-manifest). Manifest ?v= served the new build's index-COp4Qn18 -> mixed build."
  - method: observation
    deployment: vcptcore_qa
    build: e8ff6ac5fbd4c99e
    conditions: module:VirtoCommerce.SalesRep=3.1023.0-pr-37-31e8; theme=2.62.0-alpha.2643; cdn=cloudflare; browser=chromium
    at: 2026-11-11T12:41:10.730Z
    by: session:a3241c2b
    bodyHash: ebfa79cc4ca3
    who: Aleksandra-Mitricheva
    note: "2026-11-11 12:40Z, fresh Chrome context after a theme redeploy: browser remoteEntry.js = cf HIT, last-modified 2026-11-04 14:22:44, age 6993, virtualExposes-BMpwOUpz; curl with Origin reproduces it; without Origin or with ?cb= = BYPASS, last-modified 2026-11-11 11:38:52, virtualExposes-CCH71ksq (matches the manifest). my-customers-BL3s5j4U.js + 8 more chunks 404, /company/my-customers shows 'Part of the site could not be loaded'."
---
vcst 2026-11-05 15:40Z, SalesRep 3.1018.0-pr-33-ff13 deployed at 14:46:38. Same URL /modules/$(VirtoCommerce.SalesRep)/plugins/vc-frontend/remoteEntry.js on the storefront origin: curl without an Origin header → cf-cache-status HIT, last-modified 14:46:38, age 2427, body references virtualExposes-DgEd6DeA (ff13). curl WITH 'Origin: <storefront origin>' plus Sec-Fetch-Mode: cors (what the browser's Module Federation loader sends) → HIT, last-modified 12:23:20, age 3471, body references virtualExposes-CdDcSffn (the previous build). Response carries only 'vary: Accept-Encoding' and cache-control max-age=14400, so the CDN holds a separate object for CORS requests that was cached shortly before the deploy and outlives it by up to 4 h. In Edge, after clearing the HTTP cache and signing in, Performance entries show remoteEntry.js and virtualExposes-CdDcSffn.js re-fetched (then served from cache), the dashboard chunk 404s and /company/dashboard stays on 'Loading page…'. Consequences: (1) a curl check without Origin passes while browsers fail; probe with the Origin header to see what browsers get; (2) clearing the browser cache only helps once the CORS copy at the CDN has expired or been purged; (3) mf-manifest.json is fetched with ?v= and is DYNAMIC, so it can name the new index chunk while remoteEntry is old, a mixed build. This refines KB-C2202F86 (browser cache) and KB-4358B2EC (CDN copy).
