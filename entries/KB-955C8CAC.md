---
id: KB-955C8CAC
subject: Unsubscribing in /account/back-in-stock toggles the button to 'Stock alert' but leaves the product in the list, also after reload
plane: experiential
question: Is a product removed from /account/back-in-stock when the shopper unsubscribes?
status: active
appliesTo:
  - axis: account
    value: personal
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/back-in-stock
evidence:
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    conditions: theme=2.61.0-alpha.2605
    defect: suspected
    at: 2026-10-28T10:47:08.216Z
    by: session:7e8ea7fb
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    conditions: platform=3.1090.0; theme=2.61.0-alpha.2605; browser=chromium; account=personal
    at: 2026-10-28T20:59:06.134Z
    by: session:ccf68ec7
    bodyHash: c1a6f87da2a5
    defect: VCST-6612
    who: Lenajava1
    note: Re-observed on Chromium with a different personal account. The list query GetBackInStockSubscriptions sends no isActive (sort isActive:asc;createdDate:desc) and returns the deactivated row with isActive:false; REST subscriptions/search shows isActive=false stored.
---
Subscribed to QA-OOS-001 from its PDP (Stock alert -> 'You'll be notified'), opened /account/back-in-stock (row with name, image, price and the 'You'll be notified' button), clicked that button: it flips to 'Stock alert' and the PDP shows the unsubscribed state, but the row remains in the list immediately and after a full reload. A list with zero subscriptions shows 'Your list is empty'.
