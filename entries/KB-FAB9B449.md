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
---
Signed-in personal user, B2B-store, ALCOE2300 in cart, Pickup selected, map-mode Pick points modal (Load more, 50 per page, 100 after one Load more). After choosing Chelsea Market from the second batch and pressing Pick up here, the Pickup point row reads '75 9th Ave, New York, New York, 10005, United States of America' - the name Chelsea Market is not shown anywhere on the cart. Shipping cost stays $0.00. Reopening the modal lists Chelsea Market first with its radio checked (51 radios: the selected one prepended to the first 50), the search box empty and no chips; typing a term that matches nothing (AGENT-TEST-Westside Mall) leaves only the selected location in the list, and the field's Clear button restores the 51 entries with no Reset search button.
