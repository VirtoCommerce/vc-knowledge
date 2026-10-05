---
id: KB-F0C5CA5E
subject: The account coupons page uppercases coupon codes in the card and in the copied clipboard value
plane: experiential
question: How does the account coupons page display and copy a coupon code?
questions:
  - text: Why does my coupon look all capitals in my account when I received it in lowercase?
  - text: When a buyer copies a code from the account coupons list, is the original letter case preserved?
  - text: Does the coupon card on the account page transform the code before showing or copying it?
  - text: Is the uppercase coupon code on the my-coupons page a defect that breaks redemption?
concepts:
  - id: public-coupon-list
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/coupons
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T11:16:59.019Z
    by: session:memimpor
    who: Lenajava1
    splitFrom: KB-001183CF
---
The coupon card on /account/coupons forces the code to uppercase in JavaScript, both in the display and in the copied clipboard value - a presentation-only issue, since the uppercased code still applies.
