---
id: KB-8CFB57D0
subject: "salesRepCoupons: anonymous gets Unauthorized in HTTP 200, a non-rep buyer and an empty or unknown storeId get an empty list, and label is null when cultureName is omitted"
plane: experiential
question: What does Query.salesRepCoupons return to an anonymous caller, a non-rep buyer, an empty or unknown storeId, or a request without cultureName?
questions:
  - text: Is salesRepCoupons protected against anonymous callers?
  - text: Does salesRepCoupons with storeId empty return every store's coupons?
  - text: Why is label null on salesRepCoupons items?
  - text: How does salesRepCoupons localize name, description and label?
concepts:
  - id: coupon
  - id: sales-rep
  - id: graphql-api
status: active
appliesTo:
  - axis: module
    value: sales-rep
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.salesRepCoupons
  - coordinate: POST /graphql/sales-rep
evidence:
  - method: observation
    deployment: vcst_qa
    build: e967c68919b4671e
    conditions: module:VirtoCommerce.SalesRep=3.1017.0-pr-30-1e10; store=B2B-store
    at: 2026-11-03T16:53:36.259Z
    by: session:05e86aa3
    who: Lenajava1
    note: "distinct: Different operation (salesRepCoupons, not salesRepCustomerOrders), and it adds the anonymous, storeId and cultureName/label behaviour of that operation."
---
SalesRep 3.1017.0-pr-30-1e10, /graphql/sales-rep. No token, or an invalid bearer: HTTP 200, data.salesRepCoupons null, errors[0].extensions.code Unauthorized ("Anonymous access denied or access token has expired or is invalid."). Buyer token without sales-rep:access: HTTP 200, totalCount 0, items [], no errors[]. Rep token with storeId "", "   " or an unknown id: HTTP 200, totalCount 0, no errors. cultureName de-DE: name/description come from the promotion's de-DE localized values and fall back to the invariant name/description when absent; label is the de-DE localizedLabel and is null when that culture has none (no fallback). cultureName omitted or a culture with no values (nl-NL): name/description are the invariant values and label is null for every item. The buyer promotionCoupons query on /graphql behaves the same for label (null on all 27 items without cultureName).
