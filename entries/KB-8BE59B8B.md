---
id: KB-8BE59B8B
subject: promotionCoupons lists a coupon whose own expirationDate has passed while its promotion is active, and omits a promotion that has two coupons
plane: experiential
question: Does the buyer's promotionCoupons query hide a coupon whose own expiration date has passed, and does it list promotions with more than one coupon?
questions:
  - text: Why does /account/coupons show an expired coupon?
  - text: Is a multi-coupon promotion listed by promotionCoupons?
  - text: How does promotionCoupons differ from salesRepCoupons?
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.promotionCoupons
evidence:
  - method: observation
    deployment: mac_local
    conditions: platform=3.1087.0; module:VirtoCommerce.MarketingExperienceApi=3.1004.0; module:VirtoCommerce.Marketing=3.1010.0; store=B2B-store
    at: 2026-10-21T19:13:39.630Z
    by: session:6eb5c164
    note: "distinct: KB-50CD6A34 covers promotion-level filters (active, public, promotion endDate); this entry is about coupon-level expiry and the number of coupons per promotion."
---
As a B2B buyer, promotionCoupons(storeId: B2B-store, first: 200) on /graphql was read at the same moment as the rep's salesRepCoupons. A public active promotion whose single coupon has expirationDate = yesterday WAS listed by promotionCoupons (salesRepCoupons hid it); that was the only difference between the two sets (buyer 24, rep 23). A public active promotion with two coupons was listed by neither. Each promotionCoupons item id equalled its promotion id. So promotionCoupons filters at promotion level only, not on the coupon's own expiry; whether it hides used-up coupons was not exercised (use counts cannot be seeded through REST).
