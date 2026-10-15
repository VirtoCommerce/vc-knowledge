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
---
2026-10-15, theme 2.59.0, signed-in buyer. Opening 'Check pickup locations' on a PDP shows the 'Pick points' modal with the location list on the left (cards with name, address, availability chip) and a grey map pane with Google's 'This page can't load Google Maps correctly. Do you own this website?' dialog. Console: 'Google Maps JavaScript API error: ApiProjectMapError' and 'warning: NoApiKeys'. So every map behaviour (markers, zoom, info windows, map/list toggle, marker sync) is not observable on this stand - environment configuration (no Maps API key / map id), not a storefront defect.
