---
id: KB-A671CFDA
subject: With the SalesRep plugin header link, a sales rep sees a 'Sales Rep Hub' (briefcase-business icon) item first in the desktop header icon row and last in the mobile menu nav list; non-reps and anonymous see none, and sign-in/sign-out reloads the document so the item follows the user without a manual refresh
plane: experiential
question: Where does the Sales Rep Hub header link to /company/dashboard appear on desktop and mobile, who sees it, and does it appear after sign-in without a manual refresh?
questions:
  - text: Is the Sales Rep Hub icon the leftmost item in the storefront header icon row?
  - text: Does a buyer or org maintainer see the Sales Rep Hub header icon?
  - text: Where is the Sales Rep Hub link in the mobile hamburger menu?
  - text: Does the Sales Rep Hub header icon need a page refresh after a rep signs in?
  - text: Does the Sales Rep Hub header icon disappear after sign-out and stay gone on browser Back?
status: active
appliesTo:
  - axis: feature
    value: sales-rep-hub-plugin
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/dashboard
  - coordinate: POST /connect/token
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 7b487e2da4bd961b
    conditions: theme=2.62.0-pr-2627-92b1-92b10c04; module:VirtoCommerce.SalesRep=3.1017.0-pr-31-efa5; store=B2B-store; browser=chromium
    at: 2026-11-03T14:14:10.350Z
    by: session:571829f4
    who: Aleksandra-Mitricheva
    note: "distinct: KB-ACD62417 covers where a non-rep lands on a deep link; this entry is about the new header icon-row link itself: its position on desktop and in the mobile menu, its styling parity, who sees it, and that sign-in reloads the document so it appears without a manual refresh."
  - method: observation
    deployment: vcptcore_qa
    build: eccc5217d9e5af7e
    conditions: theme=vc-frontend-next 3.2.0; module:VirtoCommerce.SalesRep=3.1017.0-pr-31-efa5; store=B2B-store; browser=chromium
    at: 2026-11-03T15:11:19.795Z
    by: session:571829f4
    bodyHash: e2b00f9729da
    who: Aleksandra-Mitricheva
    note: "Holds on vc-frontend-next 3.2.0 too (host loads the plugin's mf-manifest.json and remoteEntry.js). Desktop row on Next reads Sales Rep Hub, Bulk order, Compare, Lists, Orders, Alerts, Cart (Next labels the notifications item 'Alerts'); same computed colour/hover/stroke and icon centres as siblings in light and dark (Next's own Appearance switch Light/Dark/System and prefers-color-scheme emulation both apply). Mobile: last item of the menu nav list (Dashboard, Catalog, Compare, Bulk order, Cart, Contact us, Sales Rep Hub), 270x40, plus a 'Sales Rep Hub' accordion in the account part of the menu. Plain and Advanced rep see it; buyer, org maintainer, personal account and anonymous do not. Password sign-in posts /connect/token then reloads the document; logout lands on /sign-in?returnUrl=<path> and Back does not bring the item back. Exception on Next: translated captions longer than 128 px are clipped (see the dispute on KB-5E83E458)."
---
vc-frontend host 0.2.7 with the SalesRep plugin. Signed in as a plain Sales Representative and as an Advanced rep: desktop header row (1920, 1280, 1024) reads Sales Rep Hub, Bulk order, Compare, Lists, Orders, Notifications, Cart; the link (href /company/dashboard, lucide briefcase-business icon, caption text as accessible name) has the same computed colour, hover colour, icon stroke and vertical centres as its siblings in light and dark mode. At 375 px the item is not in the top bar but is the last entry of the mobile menu nav list (Dashboard, Catalog, Compare, Bulk order, Cart, Contact us, Sales Rep Hub), 288x40, and opens /company/dashboard. A Purchasing-agent buyer, an Organization maintainer, a no-org personal account and an anonymous visitor see no item on desktop or in the mobile menu. Password sign-in posts /connect/token and then re-GETs the returnUrl document (full reload), so the item is present on first render after sign-in; logout lands on /sign-in?returnUrl=<path> with the item gone, and browser Back stays on the sign-in gate without it.
