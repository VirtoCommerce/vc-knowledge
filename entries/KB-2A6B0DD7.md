---
id: KB-2A6B0DD7
subject: On the flag-on storefront PDP, GetProductRecommendations is requested only when the 'Customers bought together' section scrolls into view
plane: experiential
question: Are the PDP recommendation sections below the fold lazy-loaded?
questions:
  - text: When does the PDP send GetProductRecommendations?
  - text: Does the product page lazy-load sections below the fold?
status: active
appliesTo:
  - axis: page
    value: pdp
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: Query.productRecommendations
  - coordinate: /consumer-electronics/televisions/lg/lg-eg9600-series-55-class-4k-smart-curved-ole
evidence:
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: theme=2.63.0-pr-2676; setting:site_builder_enabled=true; browser=chromium
    at: 2026-11-19T18:14:27.769Z
    by: session:51ac2fe3
    who: Lenajava1
---
Signed-in B2B buyer, Chromium. At a 1280x500 viewport a B2C PDP and a simple PDP sent no GetProductRecommendations in the 6 s after load (GetProduct, GetCustomerReviews, CanLeaveFeedback, GetProductPickupLocations did go); scrolling down then sent GetProductRecommendations twice plus the recommendation images. At 1920x1080, where the section starts inside the viewport (~957 px), the two calls go out on load.
