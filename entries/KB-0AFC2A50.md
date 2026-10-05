---
id: KB-0AFC2A50
subject: A shipping-reward coupon is rejected as not valid when Pickup is selected
plane: experiential
question: What happens when a free-shipping coupon is applied to a cart with Pickup delivery?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
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
