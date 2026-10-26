---
id: KB-2EFE023F
subject: "On vcptcore_qa1 the /cart 'Pick points' map modal loads Google Maps with BillingNotEnabledMapError: tiles and markers render dimmed with a 'For development purposes only' watermark, and clicking a list location still opens its 'Pick point info' card"
plane: experiential
question: Does the Google Maps map in the /cart pickup-point picker work on vcptcore_qa1?
questions:
  - text: Which Google Maps error does the BOPIS pick points map show on vcptcore_qa1?
  - text: Is vcptcore_qa1 usable for testing the map-mode pickup picker?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
evidence:
  - method: observation
    deployment: vcptcore_qa1
    conditions: theme=2.61.0-pr-2564; role=anonymous
    at: 2026-10-26T17:36:19.758Z
    by: session:624d35a1
    who: kutasinaelena
    note: "distinct: Different stand (vcptcore_qa1, not vcptcore_regression), different Maps error (BillingNotEnabledMapError) and different outcome: the map does render in degraded mode with markers and the info card works."
---
2026-10-26, theme 2.61.0-pr-2564, guest. Added an in-stock simple product with 4 pickup locations to the cart, chose Pickup on /cart and clicked 'select a pickup address': the map-mode 'Pick points' modal opened (Country/State/City filters, search, location list 'Showing 4 of 4', Google map). Console: 'Google Maps JavaScript API error: BillingNotEnabledMapError'; Google shows the 'This page can't load Google Maps correctly' overlay. After dismissing it the map is usable in degraded mode: dimmed tiles with 'For development purposes only' watermark, markers placed for each location; clicking a list card selects it, centres the map on its marker and opens the 'Pick point info' card (address, working hours, phone, email, description, Cancel / Pick up here). The Maps script key is the stand's own (last 4 '67lo'), so the failure is the Google Cloud project's billing, not the referrer.
