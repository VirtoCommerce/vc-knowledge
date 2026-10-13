---
id: KB-5EE7F47E
subject: recentlyBrowsed(maxProducts:N) returns the newest N event ROWS de-duplicated after the take, so a product browsed in two sessions makes it return fewer than N products
plane: experiential
question: "Why does Query.recentlyBrowsed(maxProducts: 6) return fewer than 6 products, and how do you seed it deterministically?"
status: active
appliesTo:
  - axis: surface
    value: graphql
anchors:
  - coordinate: Query.recentlyBrowsed
  - coordinate: Mutation.pushHistoricalEvent
evidence:
  - method: observation
    deployment: vcst_qa
    conditions: module:VirtoCommerce.XRecommend=3.1002.0; store=B2B-store
    at: 2026-10-13T14:08:04.702Z
    by: session:aa76d86f
    who: Lenajava1
    build: 942f354b4df8bb56
    buildFrom: deploy-history
---
xRecommend 3.1002.0 keeps one click row per (product, user, store, eventType, sessionId) and orders by ModifiedDate desc; GetRecentlyBrowsedQueryHandler takes the newest maxProducts ROWS and then loads products, so duplicates collapse after the take. The storefront pushes pushHistoricalEvent with sessionId = cart id. A push with NO sessionId matches every session's row for that product and stamps them all with one ModifiedDate. Observed live: after a browser run had visited seeded products, re-pushing them without a session made maxProducts 2 return 1 product and maxProducts 6 return 5 (each product's two rows sat together at the top). Pushing under one fixed sessionId instead touches exactly one row per product: after browser-session rows were created for 2 of the 6 products and 6 foreign PDP events were pushed on top, 6 pushes under the fixed session made maxProducts 6 return exactly those 6. Refines KB-2B1AEAC1, which holds only while no product has rows in two sessions.
