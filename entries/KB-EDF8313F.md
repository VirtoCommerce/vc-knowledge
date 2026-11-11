---
id: KB-EDF8313F
subject: The /cart 'Recently browsed' carousel is shown only to signed-in users; an anonymous guest never gets it however many PDPs they visit
plane: experiential
question: Is the cart page 'Recently browsed' carousel shown to anonymous guests?
questions:
  - text: Why is there no Recently browsed block on /cart for a guest?
  - text: What does the Recently browsed carousel on the cart need to render?
status: active
appliesTo:
  - axis: component
    value: recently-browsed-products
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
evidence:
  - method: observation
    deployment: vcst_qa
    conditions: theme=2.62.0-alpha.2647; store=B2B-store
    at: 2026-11-11T19:09:03.839Z
    by: session:7e403256
    who: kutasinaelena
---
On vcst as a guest I opened several PDPs and then /cart (scrolled to the end): no 'Recently browsed' block. Signed in as a B2B buyer, after opening two PDPs /cart rendered a 'Recently browsed' block with those products first (image, name, price, quantity stepper) and clicking an item opened its PDP in a new tab. vc-frontend cart.vue loads it only when isAuthenticated && the xRecommend module is enabled (and the block is not hidden).
