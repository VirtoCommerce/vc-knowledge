---
id: KB-EACB5F0A
subject: Query.promotionCoupons refuses an anonymous caller with extensions.code Unauthorized and data null
plane: experiential
question: Can an anonymous caller query promotionCoupons?
questions:
  - text: Why does promotionCoupons return Unauthorized in GraphiQL?
  - text: Does promotionCoupons need a signed-in user token?
  - text: What does promotionCoupons return without a bearer token?
status: active
appliesTo:
  - axis: principal
    value: anonymous
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.promotionCoupons
  - coordinate: POST /graphql
evidence:
  - method: observation
    deployment: vcst_qa
    build: e967c68919b4671e
    at: 2026-11-03T17:45:34.066Z
    by: session:05e86aa3
    who: Lenajava1
---
Sent { promotionCoupons(storeId: "B2B-store", cultureName: "en-US", first: 100) { totalCount items { id name couponCode } } } from the platform GraphiQL (/ui/graphiql, which sends no credential). The response was HTTP 200 with errors[0].message 'Anonymous access denied or access token has expired or is invalid.', extensions.code Unauthorized, path promotionCoupons, and data.promotionCoupons null. The same query from the signed-in storefront (GetPromotionCoupons with a bearer token and userId) returns the list. So a test of promotionCoupons needs an authenticated client; an anonymous GraphiQL run proves only the refusal.
