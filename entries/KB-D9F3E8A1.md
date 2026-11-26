---
id: KB-D9F3E8A1
subject: On /account/coupons the regular coupon-item skeleton is shorter than the loaded coupon card, so the list grows when coupons load
plane: experiential
question: Does the /account/coupons coupon-item skeleton have the same height as the loaded coupon card?
questions:
  - text: Does the coupons page skeleton cause a layout shift when coupons load?
  - text: How tall is the coupon skeleton compared with the coupon card on the account coupons page?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/coupons
  - coordinate: /company/my-customers/{id}/coupons
evidence:
  - method: observation
    deployment: mac_local
    conditions: platform=3.1087.0; theme=vc-frontend local build (coupon-item compact branch, regular skeleton unchanged)
    defect: suspected
    at: 2026-10-21T19:53:06.120Z
    by: session:6eb5c164
---
Measured on the storefront at 375/768/1024/1280/1920 px viewports. On /account/coupons the regular CouponItemSkeleton row is 114 px at 375/768 and 50 px at ≥1024, while the loaded regular coupon cards are 158–210 px (375/768) and 94–154 px (≥1024). When the coupons load, every row grows, so the content below moves down. The skeleton's row count matches the card count. The same numbers appear on the sales-rep all-coupons page, which reuses the same host card. The regular skeleton styles were not changed by the coupon-item compact work, so this is the host card's existing behaviour.
