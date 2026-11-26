---
id: KB-6AAABBBD
subject: "coupons/add silently drops totalUsesCount: a coupon's use count cannot be seeded through REST"
plane: experiential
question: Can a coupon's used count be set by sending totalUsesCount to POST /api/marketing/promotions/coupons/add?
questions:
  - text: How do I seed a used-up coupon for a test?
  - text: Does coupons/add persist totalUsesCount?
  - text: Where does a coupon's totalUsesCount come from?
status: active
appliesTo:
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/marketing/promotions/coupons/add
  - coordinate: POST /api/marketing/promotions/coupons/search
evidence:
  - method: observation
    deployment: mac_local
    conditions: platform=3.1087.0; module:VirtoCommerce.Marketing=3.1010.0
    at: 2026-10-21T19:13:48.970Z
    by: session:6eb5c164
    note: "distinct: KB-84E540DD says WHERE coupons are added; this entry says the totalUsesCount field in that body is not persisted, so usage cannot be seeded through REST."
---
POST /api/marketing/promotions/coupons/add with coupons carrying maxUsesNumber 5/3/1 and totalUsesCount 2/2/1 returned 204; POST /api/marketing/promotions/coupons/search for those promotions then returned totalUsesCount 0 for each while maxUsesNumber and expirationDate were stored as sent. The value is not persisted (CouponEntity.TotalUsesCount is NotMapped in vc-module-marketing): a coupon's use count is computed from PromotionUsage rows, which are recorded from orders whose discounts carry the coupon code (an order-changed handler queues a background job). There is no REST endpoint that writes promotion usages, so a used-up or partly used coupon can only be produced by placing orders with that coupon. Downstream, xAPI salesRepCoupons also reported totalUsesCount 0 for these coupons.
