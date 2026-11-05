---
id: KB-1BF50BB9
subject: With a stale remoteEntry.js, the "Part of the site could not be loaded" Reload page button does not recover the hub (the same 404s return), and the notice covers the Account menu button
plane: experiential
question: Does the "Part of the site could not be loaded" Reload page button recover the Sales Rep hub when the plugin's lazy chunks 404 after a redeploy?
questions:
  - text: Why does Reload page not fix the Sales Rep hub after a module redeploy?
  - text: Why does the Sales Rep Hub header link not navigate?
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
  - coordinate: /company/sales-reps
  - coordinate: /company/my-customers
evidence:
  - method: observation
    deployment: vcst_qa
    build: 29f9d60e551ad495
    conditions: module:VirtoCommerce.SalesRep=3.1018.0-pr-33-8d4a; theme=2.62.0-pr-2638-1dba-1dbab4e2; cdn=cloudflare; browser=edge
    defect: suspected
    at: 2026-11-05T13:25:21.266Z
    by: session:ffa9e791
    who: Lenajava1
    note: "distinct: Entry KB-4358B2EC records the stale CDN object; this one records what the host's recovery notice does in that state (Reload page does not recover, notice blocks the Account menu, header link does not navigate), which contradicts the recovery KB-51F5CFD6 saw for a failed download."
  - method: observation
    deployment: vcst_qa
    build: d474e86137229de8
    conditions: theme=2.62.0-pr-2637-02f2-02f26436; browser=edge; cdn=cloudflare
    at: 2026-11-05T14:36:32.094Z
    by: session:ffa9e791
    bodyHash: d9e399cd2701
    who: Lenajava1
    note: "2026-11-05 ~14:30Z on /company/tasks as SR_REP_ACME2: the notice 'Part of the site could not be loaded. Please reload the page to continue.' with Reload page appeared; clicking it reloaded the document and the same 404 (assets/index-D2gre0Yj.js) and notice returned."
  - method: observation
    deployment: vcst_qa
    build: d474e86137229de8
    at: 2026-11-05T14:45:44.795Z
    by: session:ffa9e791
    bodyHash: d9e399cd2701
    who: Lenajava1
    note: "Cause found 2026-11-05 ~14:41Z: the stale remoteEntry.js was in the browser's own HTTP cache (max-age=14400, no version query), so Reload page re-uses it. Clearing the browser cache restored the hub at once (see the new browser-cache entry)."
---
Signed in as a rep with the browser holding a stale unhashed remoteEntry.js from the CDN (HIT, age 2642 s, last-modified hours before the redeploy), the hub routes /company/sales-reps, /company/my-customers, /company/activities and /company/documents stayed on "Loading page…" with chunk 404s and a red notice "Part of the site could not be loaded. Please reload the page to continue." appeared with a "Reload page" button. Pressing Reload page reloaded the document and the same 404 and the same notice returned, so the recovery that works for a failed or slow remoteEntry.js download (KB-51F5CFD6) does nothing when the entry itself is the stale object. While the notice is open it intercepts pointer events over the Account menu button (the click timed out until the notice was closed). The header "Sales Rep Hub" link, clicked on the home page, left the URL on "/" because its dashboard chunk 404ed. /company/tasks still rendered, so the hub can be partly working.
