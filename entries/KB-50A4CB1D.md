---
id: KB-50A4CB1D
subject: On /cart the Pick points map keeps only the first page's markers after Load more; markers for locations loaded via Load more are not added
plane: experiential
question: After Load more in the /cart Pick points dialog, does the Google map add markers for the newly loaded locations?
questions:
  - text: Do pickup map pins accumulate across Load more pages?
  - text: Why are page-2 pickup locations missing from the map?
  - text: Does GetCartPickupLocations after:50 update the map markers?
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
    build: 3b66ce25e7926ad6
    conditions: theme=2.62.0-alpha.2647; platform=3.1097.0; module:VirtoCommerce.Xapi=3.1031.0-pr-70-108f; setting:Shipping.Bopis.GoogleMaps.Enabled=true; role=personal customer
    defect: suspected
    at: 2026-11-12T05:04:30.352Z
    by: session:7e403256
    who: kutasinaelena
  - method: observation
    deployment: vcst_qa
    build: 3b66ce25e7926ad6
    conditions: theme=2.62.0-alpha.2647; platform=3.1097.0; module:VirtoCommerce.Xapi=3.1031.0; module:VirtoCommerce.XCart=3.1046.0; module:VirtoCommerce.XPickup=3.1008.0; role=personal customer; viewport=1920x1080 and 375x667
    at: 2026-11-12T07:34:47.462Z
    by: session:7e403256
    bodyHash: 40f710f03212
    defect: suspected
    who: kutasinaelena
    note: "Re-verified 1920px and 375px Map tab. After Load more (after:\"50\", 51 items all with geoLocation) selecting page-2 location Fulton Mall pans the map to 40.6839,-73.9789 z17 and there is no marker there; markers in view are page-1 only. No PinElement is constructed after Load more (gmp-pin deprecation warning count unchanged). Source: vc-frontend google-map-marker.vue creates its marker only in watch(map, immediate:false); since PR #2348 (VCST-4646, first in 2.53.0) the map is not remounted on append, so appended markers never init. Not new in 2.62."
---
Signed-in personal user, B2B-store, one simple product in cart, Pickup selected, map-mode Pick points dialog (115 active locations). Initial GetCartPickupLocations first:50 -> 50 list entries; Load more sends after:"50" (no errors[], hasNextPage true, endCursor 100) and the list grows to 100 with counter 'Showing 100 of 115', no duplicates, focus moves to the first new radio. The map, however, still shows only page-1 markers: at world zoom the clusters are identical before and after Load more (NYC cluster of 19), and zooming into NYC shows 19 markers, all page-1 locations, while 61 loaded locations fall in that area; page-2 locations (Carnegie Hall, Central Park Zoo, Chelsea Market) have geoLocation in the response but no marker.
