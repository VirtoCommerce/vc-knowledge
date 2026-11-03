---
id: KB-F490DA8B
subject: promotionCoupons and /account/coupons still list a coupon whose own expiration date has passed while its promotion is active, showing the promotion's endDate
plane: experiential
question: Is a coupon with a past coupon expiration date (promotion still active) listed on /account/coupons?
questions:
  - text: Does promotionCoupons filter on the coupon's expirationDate or only the promotion's endDate?
  - text: Why does an expired coupon still show on the account coupons page?
  - text: Which Expires date does the coupon card show — coupon or promotion?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.promotionCoupons
  - coordinate: /account/coupons
evidence:
  - method: observation
    deployment: vcst_qa
    build: e967c68919b4671e
    conditions: theme=2.62.0-pr-2596-e749-e749b47f; store=B2B-store
    at: 2026-11-03T17:48:35.316Z
    by: session:05e86aa3
    who: Lenajava1
    note: "distinct: KB-50CD6A34 covers the promotion-level filter (active, public, promotion endDate); this fact is about the COUPON's own expiration date, which that filter ignores, so a coupon expired at coupon level is still listed."
---
Admin: public promotion 'AGENT-TEST-SRCPN Coupon Expired' (B2B-store), promotion Expiration date Dec 4, 2026, one coupon AGENTTESTSREXPIRED whose Coupon expiration date is Nov 3, 2026 12:59:59 AM (already past at 2026-11-03T17:48Z). Storefront, signed-in buyer: GetPromotionCoupons (first 16, sort endDate:asc) returns the item with endDate 2026-12-03T23:59:59Z (the promotion's end) and errors[] absent, and /account/coupons renders the card 'Expires Dec 4, 2026' with a working 'Click to copy'. PromotionCouponType carries no coupon-level expiration field, so the list filters and dates by the promotion only; a coupon expired at coupon level is offered to the buyer. Whether that is intended was not checked against docs.
