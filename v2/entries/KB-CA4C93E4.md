---
id: KB-CA4C93E4
subject: discount-rate-not-persisted-on-order
plane: experiential
question: can I tell from a placed order what percentage a promotion took off it
status: active
appliesTo:
  - axis: surface
    value: storefront-xapi
  - axis: surface
    value: rest
  - axis: principal
    value: customer
anchors:
  - coordinate: OrderDiscountType.amount
  - coordinate: OrderDiscountType.promotionId
  - coordinate: Discount.discountAmount
evidence:
  - method: observation
    deployment: vcptcore_stable
    at: 2026-09-10T19:56:35.741Z
    by: session:82e15c99
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-14T08:40:17.155Z
    by: session:0a2d9431
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-16T17:25:32.592Z
    by: session:26f59771
    note: "Both discounted orders on B2B-store store money and no rate: discounts[0] carries promotionId, name, description, coupon null and discountAmount, with no percentage anywhere; the rate is only in the free-text description a human typed. taxPercentRate exists on the same records, so the asymmetry with tax holds here too."
---

A placed order stores no discount rate: the discount entry carries only a money amount plus promotionId, promotion name, description and coupon, so the percentage is recoverable only by dividing the amount by the base it was taken from, or by following promotionId back to the promotion definition - which is mutable and can be edited or deleted after the order exists. The free-text description is the only place a rate may appear, and only because a human happened to type it there. By contrast the same schema does keep a rate for tax, in taxPercentRate.
