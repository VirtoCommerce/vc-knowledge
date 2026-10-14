---
id: KB-2EDC661F
subject: Query.cart without a cart name returns the most recently touched cart, not the default one
plane: experiential
question: Which cart does Query.cart return when no cartName is passed?
questions:
  - text: Why does the API show me a different basket than the one I expected to be my main cart?
  - text: A user has several carts - which one comes back from the cart query when no cart name is given?
  - text: Does omitting cartName on Query.cart resolve the cart named default or the last modified one?
concepts:
  - id: cart
  - id: context-argument
status: active
appliesTo:
  - axis: api
    value: xapi-graphql
  - axis: store
    value: b2b-store
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.cart
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-19T13:55:48.318Z
    by: session:local_5a
    splitFrom: KB-6C3FA2B7
    build: 8169d5e903339ded
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-14T22:29:44.618Z
    by: session:624d35a1
    who: kutasinaelena
    note: "XCart 3.1039.0: with a parallel session creating named carts on the same user, Query.cart (no cartName, cartType null/empty) returned the freshly created named cart (empty) instead of the 'default' cart; /cart on the storefront showed 'Your cart is empty' while the default cart still held items added from a list."
---
Query.cart with no cartName returns the most-recently-touched cart, NOT the one named 'default'.
