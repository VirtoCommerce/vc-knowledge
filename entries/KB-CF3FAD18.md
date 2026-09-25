---
id: KB-CF3FAD18
subject: loyalty cart validation errors show on /cart before Place order, which is disabled
plane: experiential
question: When a loyalty points cart has insufficient balance, does /cart warn and block Place order before submission?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
  - coordinate: CartType.validationErrors
  - coordinate: Mutation.changeCartItemsQuantity
evidence:
  - method: observation
    deployment: vcst-qa
    at: 2026-09-25T11:14:44.450Z
    by: session:memimpor
    who: Lenajava1
---
Loyalty cart-validation errors surface pre-emptively on /cart: the message renders inline directly above Place order and Place order is disabled, with no placement attempt needed. LOYALTY_INSUFFICIENT_BALANCE reads like 'Not enough points - needs N, you have M' (errorParameters required and available), and LOYALTY_ONLY_POINT_PRODUCTS_NOT_ALLOWED shows the compact 'Add a regular product to check out.' The errors arrive as cart.validationErrors on the mutation that changes the cart, inside an HTTP 200. The balance check is strict: a points total equal to the balance is allowed, one point more is blocked. An earlier build showed these only as a toast after clicking Place order; that is superseded.
