---
id: KB-6C3FA2B7
subject: Storefront cart keeps its applied coupon across sign-out / sign-in
plane: experiential
question: Does the storefront cart keep an applied coupon when the shopper signs out and signs back in?
status: active
appliesTo:
  - axis: api
    value: xapi-graphql
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: Mutations.mergeCart
  - coordinate: Mutations.addCoupon
  - coordinate: Query.cart
evidence:
  - method: observation
    deployment: vcst
    at: 2026-09-19T13:55:48.318Z
    by: session:local_5a
---
Yes - on every path. Sign-out issues NO cart mutation (signMeOut = unauthorize + Apollo clearStore + localStorage cleanup), so the registered user's server-side cart, coupon row included, is untouched. On sign-in vc-frontend calls mergeCart ONLY when the anonymous cart has line items (guard: cart.value?.id && cart.value.items?.length), so a plain round-trip with an empty guest cart performs no merge at all. When a merge DOES fire, the coupon survives from BOTH sides and its discount RE-PRICES against the post-merge subtotal rather than staying frozen. Measured 2026-09-19 via xAPI with coupon VALID10 (10% off cart): S1 plain re-auth - sub 378 / disc 37.8 unchanged; S2 coupon on the user cart + guest cart with items - disc 37.8 -> 45.3 (10% of merged 453); S3 coupon on the GUEST cart only, user cart had none - coupon carried onto the user cart, disc 5 -> 23.9 (10% of merged 239); S4b empty guest cart merged explicitly - coupon and discount unchanged. All four ended coupons=[VALID10:isAppliedSuccessfully=true]. Storefront side verified against the DEPLOYED bundle /assets/index-DYzexmEK.js, byte-equivalent to useSignMeIn.ts / useSignMeOut.ts. Corroborates BL-CART-008 and adds the storefront-side merge guard, which BL-CART-008 does not state. Incidental: Query.cart with no cartName returns the most-recently-touched cart, NOT the one named 'default'.
