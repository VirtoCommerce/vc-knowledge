---
id: KB-CDB2FE9A
subject: "With the Product Comparison module, My Account › Purchasing lists a Compare link to /compare, and the header Compare entry carries a count badge (desktop main menu; mobile: top-level hamburger item)"
plane: experiential
question: Where is the Compare link and its count in the storefront account menu and header (/account, /compare) with the Product Comparison module, on desktop and mobile?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /compare
  - coordinate: /account/dashboard
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: c2cbb357a76b771a
    conditions: theme=2.63.0-pr-2635-63a1; module:VirtoCommerce.ProductComparison=3.1000.0-pr-1-98d6; setting:Stores.AllowAnonymousUsers=true; store=B2B-store
    at: 2026-11-17T14:07:58.714Z
    by: session:fcdb91ae
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa
    conditions: theme=2.63.0-pr-2635-8318; module:VirtoCommerce.ProductComparison=3.1000.0-pr-1-7403
    at: 2026-11-19T19:12:39.476Z
    by: session:fcdb91ae
    bodyHash: 2418a3fd767b
    who: kutasinaelena
    contradicts: true
    note: On theme 2.63.0-pr-2635-8318 a guest on /compare at 390 px opening the hamburger gets the top-level main menu (Home, Catalog, Bulk order, Cart, Contact us, Compare highlighted, Sign in), not the Purchasing submenu; a signed-in buyer still lands in Purchasing with Compare highlighted. The entry's last sentence describes the earlier build 63a1.
  - method: observation
    deployment: vcptcore_qa
    build: 6cf3e54e8335fd8d
    conditions: theme=2.63.0-pr-2635-8318-83187397; module:VirtoCommerce.ProductComparison=3.1000.0-pr-1-7403; setting:ProductComparison.MaxItems=5; setting:ProductComparison.AnonymousListEnabled=true; store=B2B-store
    at: 2026-11-23T06:32:25.384Z
    by: session:fcdb91ae
    bodyHash: 2418a3fd767b
    who: kutasinaelena
    note: "Desktop 1920 guest only (mobile part not re-checked): header link 'Compare' with no count at 0; 'Compare: 1' / 'Compare: 2' after adds; 'Compare: 2' -> 'Compare: 1' after Remove from compare on /compare; back to 'Compare' at 0."
  - method: observation
    deployment: vcst_qa
    build: 6feb83b80e6f1f12
    conditions: theme=2.63.0-alpha.2681; module:VirtoCommerce.ProductComparison=3.1000.0; store=B2B-store
    at: 2026-11-26T09:32:51.291Z
    by: session:779b94fc
    bodyHash: 2418a3fd767b
    note: "2026-11-26 at 390x844: signed-in buyer on /compare opens the hamburger into the Purchasing submenu (Dashboard, Orders, Returns, Lists, Compare, Quote requests, Saved for later, Back-in-stock list) with only Compare lit. A guest gets the top-level main menu (Home, Catalog, Bulk order, Cart, Contact us, Compare lit, Sign in / Sign up now), not Purchasing - the entry's last sentence (guest lands in Purchasing) does not hold on this build, as the existing dispute says."
---
On theme 2.63.0-pr-2635 with ProductComparison 3.1000.0-pr-1: the signed-in buyer's account sidebar section Purchasing lists Dashboard, Orders, Returns, Lists, Compare, Quote requests…; Compare navigates to /compare. On desktop (1920) the header main menu has "Compare" with a count badge (accessible name "Compare: N") equal to the list size after every add and remove; at 0 items no count is drawn. At 390 px the header row has no compare icon; Compare is the last top-level item of the hamburger menu with the same badge, and the Purchasing submenu also has a Compare link without a count. A guest on /compare at 390 px opening the hamburger lands in the Purchasing submenu (account pages) because the menu opens the submenu holding the current route.
