---
id: KB-DFD8AB55
subject: Adding a product to a NEW list from the PDP Add to list dialog sends GA4 add_to_wishlist twice for one Save
plane: experiential
question: How many GA4 add_to_wishlist events does one Save in the PDP Add to list dialog send when the list is created in the same dialog?
questions:
  - text: Duplicate add_to_wishlist when creating a new list from Add to list?
  - text: Does Add new list + Save in the wishlist dialog double-report add_to_wishlist?
status: active
appliesTo:
  - axis: event
    value: add_to_wishlist
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /product/{id}
  - coordinate: window.dataLayer
evidence:
  - method: observation
    deployment: vcst_qa
    build: 3b66ce25e7926ad6
    conditions: theme=2.62.0-alpha.2647; platform=3.1097.0; store=B2B-store; role=personal customer
    defect: suspected
    at: 2026-11-11T18:33:30.850Z
    by: session:7e403256
    who: kutasinaelena
---
Signed-in personal user with no shopping lists, PDP of a simple product (SKU LT-001). Add to list -> dialog 'Please select list' (no lists) -> Add new list (checked, renamed) -> Save. window.dataLayer received two identical add_to_wishlist events (USD, value 1299.99, items [LT-001]) and two /g/collect hits en=add_to_wishlist about 600 ms apart, for one product added to one list. Not checked whether adding to an existing list sends one.
