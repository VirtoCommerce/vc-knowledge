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
    deployment: vcst
    at: 2026-09-19T13:55:48.318Z
    by: session:local_5a
    splitFrom: KB-6C3FA2B7
---
Query.cart with no cartName returns the most-recently-touched cart, NOT the one named 'default'.
