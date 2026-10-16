---
id: KB-ACB009C9
subject: Below the lg breakpoint /account/orders shows no active-filter chips or Reset filters chip; the applied filter is only visible inside the filter sidebar
plane: experiential
question: At mobile width, does /account/orders show which filters are applied after the filter panel closes?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: viewport
    value: mobile
anchors:
  - coordinate: /account/orders
evidence:
  - method: observation
    deployment: vcst_qa
    build: e00c89f4da0e9c98
    conditions: platform=3.1083.0; theme=2.60.0-alpha.2579; viewport=375x812; browser=firefox
    at: 2026-10-14T14:42:41.275Z
    by: session:6630b76f
    who: Lenajava1
---
At 375x812 (Firefox), opened the orders filter sidebar via the filter icon button, ticked Processing, tapped Apply: the sidebar closed and the list showed only the 4 Processing orders, but no chip, badge or Reset filters control appeared above the list and the filter icon button looked identical to its unfiltered state. At 1920 wide the same filter renders a 'Processing' chip plus a 'Reset filters' chip. vc-frontend orders.vue styles .orders__chips as hidden below the lg breakpoint, so this is by design; the only way to see or clear the filter on mobile is to reopen the sidebar (its Reset applies immediately and closes it). The mobile sidebar applies on Apply, not live while open.
