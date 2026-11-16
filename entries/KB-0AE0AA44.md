---
id: KB-0AE0AA44
subject: For a variation parent and a configurable product with a subscription plan the PDP shows no Subscribe & Save; the variation-parent cart line offers it, the configured cart line does not, yet Mutation.setCartItemSubscription accepts the configured line and the order creates a subscription at the configured price
plane: experiential
question: Can a variation product or a configurable product be bought as a product subscription on the storefront?
questions:
  - text: Does the PDP show Subscribe & Save for products with variations?
  - text: Does setCartItemSubscription accept a configurable product line?
  - text: Does a product subscription keep the configuration of a configurable product?
status: active
appliesTo:
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /product/{id}
  - coordinate: /cart
  - coordinate: Mutation.setCartItemSubscription
  - coordinate: Query.subscriptionOffers
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: ccb4570264d7de66
    conditions: theme=2.63.0-pr-2602-399d; store=B2B-store
    at: 2026-11-16T16:25:23.405Z
    by: session:d9b8e380
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    build: ccb4570264d7de66
    conditions: theme=2.63.0-pr-2602-399d; module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-595d; store=B2B-store
    at: 2026-11-16T18:44:39.598Z
    by: session:d9b8e380
    bodyHash: 50faae73789c
    who: kutasinaelena
    note: "Re-observed the UI half: the variation parent PDP and the configurable PDP show no purchase options, and the variation parent's /cart line shows an unticked 'Subscribe & Save Save 5%'. The API half was not re-run; the PR source agrees: the setCartItemSubscription handler checks only that an offer and the interval exist, with no check on product type."
---
Theme 2.63.0-pr-2602, a PERCENT 5 plan covering a variation parent, one of its variations and a configurable product; Query.subscriptionOffers returns an offer for all three. Variation parent PDP (variations list, 'Variations in cart'): no purchase options. After adding the parent row, its /cart line shows an unticked 'Subscribe & Save Save 5%'; ticking it applies a 'Subscription discount' line promotion ($125 -> $118.75) and the order created a subscription on the parent productId at referencePrice 118.75. Configurable PDP, before and after picking an option: no purchase options. Its /cart line has no Subscribe & Save control. Calling Mutation.setCartItemSubscription on that configured line succeeded; /cart then showed the consent block and a 5% discount on the configured price ($280 -> $266), and the order created an Active subscription on the configurable product with referencePrice 266. The ProductSubscription type exposes no configuration fields, so whether renewals keep the chosen option cannot be read from xAPI.
