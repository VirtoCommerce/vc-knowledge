---
id: KB-B66D0EE4
subject: The /cart Pick points dialog filters only by Country, State / Province and City facets plus a submit-only keyword search; there is no availability or distance filter
plane: experiential
question: Which filters does the /cart Pick points dialog offer, and does the keyword search fire while typing?
questions:
  - text: Is there an availability or distance filter in the pickup location picker?
  - text: Is the pickup location keyword search debounced or submit-only?
concepts:
  - id: pickup
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
    build: 1fe74fdd17a1b14e
    conditions: platform=3.1101.0; theme=2.63.0-alpha.2681; role=personal customer
    at: 2026-11-26T02:40:04.426Z
    by: session:779b94fc
    who: kutasinaelena
    note: "distinct: KB-A370A56A is about the shipping/billing Select address popup; this is the pickup-location Pick points dialog and its facet set (no availability/distance filter)."
---
Signed-in personal customer, B2B-store, map-mode Pick points dialog (115 active locations). The filter row has three facet dropdowns - Country, State / Province, City (values with counts) - and a 'Search pickup locations' box; nothing filters by availability or by distance. Typing 'Brooklyn' char by char sent no request; Enter sent one GetCartPickupLocations with keyword, first:50 and no after, replacing a 101-item list with 'Showing 5 of 5'. Facets narrow each other and combine with keyword by AND (USA + California + Sacramento + 'Museum' -> no match). 'Reset filters' restores the unfiltered first page; on mobile it is data-test-id reset-filters-mobile-button.
