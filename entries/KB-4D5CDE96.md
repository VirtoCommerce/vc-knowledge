---
id: KB-4D5CDE96
subject: The /account/lists/{id} list detail toolbar offers Add all to cart, Buy now, Save changes, Rename and Share; there is no Request Quote / quote-from-list action
plane: experiential
question: Can a B2B buyer create a quote request directly from a saved list on /account/lists/{id}?
questions:
  - text: Is there a Request Quote button on the list details page?
  - text: Does the storefront support RFQ from a wishlist / saved list?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/lists/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: 3b66ce25e7926ad6
    conditions: theme=2.62.0-alpha.2647; platform=3.1097.0; store=B2B-store; role=Organization maintainer
    at: 2026-11-12T06:24:33.840Z
    by: session:7e403256
    who: kutasinaelena
---
Signed in as a B2B organization maintainer on B2B-store, a newly created private list opened at /account/lists/{id} showed a header toolbar with 'Add all to cart', 'Buy now', 'Save changes' (disabled), 'Rename' and 'Share' and no quote-related control. The list was empty when observed, so a control rendered only per product row was not ruled out; quote requests on this build start from /cart ('Add cart items to quote') or the 'Request Quote' button on /account/quotes.
