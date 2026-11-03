---
id: KB-B1DBAD5D
subject: The Sales Rep hub customer coupons page and widget list store-wide public single-coupon promotions via salesRepCoupons (not per customer), paged by first/after offset, with endDate = earlier of coupon expiry and promotion end, rendered as a date-only value in the browser time zone
plane: experiential
question: What does /company/my-customers/<org>/coupons (salesRepCoupons) list, how is it paged, and what does its Expires date show?
questions:
  - text: Is the Sales Rep Available coupons widget personalized to the customer organization?
  - text: Which date does Expires show on the sales rep coupon card when the coupon and the promotion end on different days?
  - text: How does the sales rep promotion coupons page paginate?
  - text: Why does a coupon with a past coupon-level expiry not appear on the sales rep coupons page?
status: active
appliesTo:
  - axis: role
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/my-customers/{id}/coupons
  - coordinate: Query.salesRepCoupons
evidence:
  - method: observation
    deployment: vcst_qa
    build: e967c68919b4671e
    conditions: module:VirtoCommerce.SalesRep=3.1017.0-pr-30-1e10; theme=2.62.0-pr-2596-e749-e749b47f; module:VirtoCommerce.Marketing=3.1010.0; platform=3.1093.0-pr-3154-4607; store=B2B-store; browserTimeZone=Central Europe Standard Time
    at: 2026-11-03T16:56:41.424Z
    by: session:05e86aa3
    who: Lenajava1
---
On vcst_qa (SalesRep 3.1017.0-pr-30, theme 2.62.0-pr-2596), signed in as a sales rep: the customer profile widget sends SalesRepCoupons {storeId, cultureName, first: 5} and the "All promotion coupons" page sends {first: 10, after: "0"/"10"/"20"} — no organizationId, so every served customer (and an unserved org's URL) gets the same store-wide list; the URL does not carry the page, so reload or re-entry starts on page 1. Items: public, active, started, single-coupon promotions; a coupon whose own expirationDate is past, one at its maxUsesNumber, one bound to a member (memberId), and one whose promotion starts in the future are absent (all four visible on /account/coupons). endDate = the earlier of coupon expirationDate and promotion endDate (coupon 2026-11-13T23:59:59Z vs promotion 2027-01-02 -> 2026-11-13T23:59:59Z; promotion 2026-11-23 vs coupon 2026-12-13 -> 2026-11-23). Order: endDate ascending, undated last, ties by coupon id. Uses line: "N use(s) left out of M" (maxUsesNumber - totalUsesCount) or "Unlimited"; de-DE "N Einlösungen von M übrig" / "1 Einlösung von 2 übrig". The Expires date is formatted in the browser time zone: in Central Europe time 2026-11-13T23:59:59Z shows "Nov 14, 2026" and 2027-05-31T22:00:00Z shows "Jun 1, 2027". The card shows the code uppercased (agenttestlc062 -> AGENTTESTLC062) and copies the uppercased value.
