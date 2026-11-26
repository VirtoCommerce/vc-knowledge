---
id: KB-FAB9B449
subject: On /cart the pickup-point row shows only the selected location's address line, never its name; the Pick points map modal reopens with the selected location first and checked
plane: experiential
question: What does the cart Pickup point row show after choosing a location in the Pick points modal, and how does the modal look when reopened?
concepts:
  - id: cart
status: active
appliesTo:
  - axis: feature
    value: bopis
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
  - coordinate: Query.cartPickupLocations
evidence:
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    conditions: theme=2.61.0-alpha.2605; role=personal customer; setting:Shipping.Bopis.GoogleMaps.Enabled=on (map modal observed)
    at: 2026-10-28T10:13:42.342Z
    by: session:7e8ea7fb
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: 3b66ce25e7926ad6
    conditions: theme=2.62.0-alpha.2647; platform=3.1097.0; module:VirtoCommerce.Xapi=3.1031.0-pr-70-108f; setting:Shipping.Bopis.GoogleMaps.Enabled=true; role=personal customer
    at: 2026-11-12T05:01:36.308Z
    by: session:7e403256
    bodyHash: f03fadf7cd33
    who: kutasinaelena
    note: Picked Barclays' Center (620 Atlantic Ave) via keyword search; Pickup point row reads '620 Atlantic Ave, New York, New York, 10054, United States of America', no name. AddOrUpdateCartShipment(pickupLocationId) errors[] empty, Shipping cost $0.00.
  - method: observation
    deployment: vcst_qa
    build: 1fe74fdd17a1b14e
    conditions: platform=3.1101.0; theme=2.63.0-alpha.2681; role=personal customer
    at: 2026-11-26T02:39:36.125Z
    by: session:779b94fc
    bodyHash: f03fadf7cd33
    who: kutasinaelena
    note: Picked Whole Foods Market Prospect Heights (115th) via Load more; row reads '57 7th Ave, New York, New York, 10018, United States of America', no name; reopen lists it first, checked, 'Showing 51 of 115'.
  - method: observation
    deployment: vcst_qa
    build: 99622678db88d8be
    conditions: platform=3.1099.0; theme=2.63.0-pr-2666-7e02-7e022f10; role=signed-in B2B buyer; store=B2B-store
    at: 2026-11-16T20:42:46.266Z
    by: session:9b516c10
    bodyHash: f03fadf7cd33
    note: "B2B buyer: picked a location in the Pick points modal; AddOrUpdateCartShipment(pickupLocationId) 200, no errors[], shipping $0.00; the cart Pickup point row shows the address line only, not the location name. Modal reopen not re-tested."
  - method: observation
    deployment: vcst_qa
    build: 99622678db88d8be
    conditions: platform=3.1099.0; theme=2.63.0-pr-2666-7e02-7e022f10; role=personal customer
    at: 2026-11-16T21:33:49.718Z
    by: session:9b516c10
    bodyHash: f03fadf7cd33
    note: Picked the first listed location (620 Atlantic Ave) and pressed 'Pick up here'; Pickup point row shows the address only, no name. AddOrUpdateCartShipment sent pickupLocationId; response shipments[0].pickupLocation carries the name; Shipping cost $0.00. Switching to Pickup first set a server-default pickup point before any choice.
---
Signed-in personal user, B2B-store, ALCOE2300 in cart, Pickup selected, map-mode Pick points modal (Load more, 50 per page, 100 after one Load more). After choosing Chelsea Market from the second batch and pressing Pick up here, the Pickup point row reads '75 9th Ave, New York, New York, 10005, United States of America' - the name Chelsea Market is not shown anywhere on the cart. Shipping cost stays $0.00. Reopening the modal lists Chelsea Market first with its radio checked (51 radios: the selected one prepended to the first 50), the search box empty and no chips; typing a term that matches nothing (AGENT-TEST-Westside Mall) leaves only the selected location in the list, and the field's Clear button restores the 51 entries with no Reset search button.
