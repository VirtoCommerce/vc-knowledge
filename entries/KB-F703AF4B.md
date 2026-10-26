---
id: KB-F703AF4B
subject: On vcptcore_regression the BOPIS 'Pick points' map does not load (Google Maps NoApiKeys / ApiProjectMapError); the location list and search still work
plane: experiential
question: Does the storefront BOPIS pickup-location map render on vcptcore_regression?
status: active
appliesTo:
  - axis: feature
    value: bopis
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
  - coordinate: Query.productPickupLocations
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-15T00:13:23.921Z
    by: session:624d35a1
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_regression
    build: c55224c77967e334
    conditions: setting:Shipping.Bopis.GoogleMaps.Enabled=true; setting:Shipping.Bopis.GoogleMaps.ApiKey=key of a project without billing
    at: 2026-10-26T18:47:07.282Z
    by: session:624d35a1
    bodyHash: 87ac80114651
    who: kutasinaelena
    contradicts: true
    note: "2026-10-26: the map loads in Google's development mode (BillingNotEnabledMapError, 'For development purposes only' watermark, Google's overlay dismissed with OK); markers, list selection, the Pick point info card and 'Pick up here' all work — BOPIS-080/109 PASS in REG-2026-10-26-2044-W3R3. Earlier the same day: RefererNotAllowedMapError (key from another stand) and ApiNotActivatedMapError (key from an unbilled project with the API off). The map stays watermarked: the key's project has no billing."
---
2026-10-15, theme 2.59.0, signed-in buyer. Opening 'Check pickup locations' on a PDP shows the 'Pick points' modal with the location list on the left (cards with name, address, availability chip) and a grey map pane with Google's 'This page can't load Google Maps correctly. Do you own this website?' dialog. Console: 'Google Maps JavaScript API error: ApiProjectMapError' and 'warning: NoApiKeys'. So every map behaviour (markers, zoom, info windows, map/list toggle, marker sync) is not observable on this stand - environment configuration (no Maps API key / map id), not a storefront defect.
