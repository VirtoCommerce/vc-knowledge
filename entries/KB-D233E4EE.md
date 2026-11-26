---
id: KB-D233E4EE
subject: Sign-in cart merge keeps the raw summed quantity above stock and flags the line on /cart; lowering to stock clears the flag
plane: experiential
question: What does /cart show when the sign-in cart merge sums a guest line and an account line past available stock?
questions:
  - text: Does the guest-to-account cart merge cap quantity at available stock?
  - text: Is a merged cart line above stock flagged on /cart?
status: active
appliesTo:
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
  - coordinate: Mutation.mergeCart
evidence:
  - method: observation
    deployment: vcst_dev
    build: 7fd3fb4cc4be53eb
    at: 2026-11-18T20:39:18.961Z
    by: session:feee22e7
---
Personal customer with 6 of a stock-10 product in the account cart signed out; as guest added 6 of the same product; signed in again (storefront merges the guest cart). /cart showed one line with quantity 12 (not capped to 10), the quantity field marked invalid, the increase control disabled, 'In stock 10', and the line message 'You can order maximum 10 item(s)'; header badge 'Cart: 12'. Typing 10 (Mutation.changeCartItemsQuantity) removed the message at once. No 4xx/5xx and no visible error during the merge.
