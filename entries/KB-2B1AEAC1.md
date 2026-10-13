---
id: KB-2B1AEAC1
subject: Recently browsed products are populated by the pushHistoricalEvent "click" mutation, so they can be seeded without a browser
plane: experiential
question: How is the cart page's Recently browsed products list populated, and can it be set up headlessly?
status: active
appliesTo:
  - axis: surface
    value: graphql
anchors:
  - coordinate: Mutation.pushHistoricalEvent
  - coordinate: Query.recentlyBrowsed
  - coordinate: Mutation.moveToSavedForLater
evidence:
  - method: observation
    deployment: vcst_qa
    conditions: module:VirtoCommerce.XCart=3.1041.0; store=B2B-store
    at: 2026-10-13T13:36:04.415Z
    by: session:aa76d86f
    who: Lenajava1
    build: 942f354b4df8bb56
    buildFrom: deploy-history
---
As a signed-in storefront customer (token from /connect/token with the storeId form field), calling Mutation.pushHistoricalEvent(command: {storeId, productId, eventType: "click"}) for 6 products made Query.recentlyBrowsed(storeId, maxProducts: 6) return exactly those 6 products, in place of the account's earlier history. That is the same event the PDP pushes on view, so any PDP visit by the account rewrites the list. Anonymous calls to recentlyBrowsed and getSavedForLater answer Unauthorized. Saved-for-later items can be set up the same way: addItem, then moveToSavedForLater(lineItemIds).
