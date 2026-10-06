---
id: KB-0AFC2A50
subject: A shipping-reward coupon is rejected as not valid when Pickup is selected
plane: experiential
question: What happens when a free-shipping coupon is applied to a cart with Pickup delivery?
questions:
  - text: Why does my free shipping promo code say 'This code is not valid' when I chose pickup in the cart?
  - text: addCoupon /cart free shipping coupon rejected Pickup BuyOnlinePickupInStore
  - text: Is a shipping-reward coupon (RewardShippingGetOfAbsShippingMethod for FixedRate) accepted when the cart delivery is Pickup instead of Shipping?
  - text: Does applying a free-shipping coupon change the totals when the delivery option is pickup and shipping cost is already 0.00?
  - text: Which delivery option must be selected in the cart for a FixedRate shipping discount coupon to be accepted?
concepts:
  - id: coupon
  - id: pickup
  - id: shipping-method
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
anchors:
  - coordinate: /cart
  - coordinate: Mutations.addCoupon
evidence:
  - method: observation
    deployment: vcst
    at: 2026-10-05T20:23:15.386Z
    by: session:51fcfb97
    who: kutasinaelena
---
On theme 2.59 / vcst, coupon FREESHIP (promotion reward RewardShippingGetOfAbsShippingMethod 999 off, shippingMethod FixedRate) entered in the cart Custom code field shows This code is not valid when Pickup (BuyOnlinePickupInStore) is selected, and is accepted when Shipping is selected. Pickup shipping stays 0.00 and totals do not change.
