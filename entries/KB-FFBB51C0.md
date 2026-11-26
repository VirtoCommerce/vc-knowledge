---
id: KB-FFBB51C0
subject: salesRepCoupons answers Unauthorized to an anonymous caller and an empty result without error to a buyer or a rep with no served organizations
plane: experiential
question: What does salesRepCoupons return to anonymous callers, buyers without the sales rep role, and reps who serve no organization?
questions:
  - text: Can a buyer token read salesRepCoupons?
  - text: Does salesRepCoupons error for a rep with zero customers?
  - text: Does a rep with a locked membership still get salesRepCoupons?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.salesRepCoupons
  - coordinate: /company/my-customers/{id}/coupons
evidence:
  - method: observation
    deployment: mac_local
    conditions: platform=3.1087.0; module:VirtoCommerce.SalesRep=3.1015.0-local-feat/VCST-6158; store=B2B-store
    at: 2026-10-21T19:13:39.798Z
    by: session:6eb5c164
---
On both /graphql and /graphql/sales-rep (HTTP 200 each time): with no token, errors[0] 'Anonymous access denied or access token has expired or is invalid.' (extensions.code Unauthorized) and data.salesRepCoupons null. With a B2B buyer token (no sales rep role): totalCount 0, items [], no errors. A rep holding the sales rep role with zero organization memberships: totalCount 0, no errors. A rep whose membership in one organization is locked but who still serves another: the full store-wide list. On the storefront, that locked rep's /company/my-customers/<locked org id>/coupons shows 'Customer not found or not in your customers.' with no coupon rows (SalesRepCoupons is still requested in parallel with SalesRepCustomer), and the served organization's coupons page lists coupons.
