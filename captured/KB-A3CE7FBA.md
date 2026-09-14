---
id: KB-A3CE7FBA
subject: recovering a percentage rate from a discount amount
plane: experiential
question: can I divide a cart-level discount amount by the subtotal to recover the percentage
status: active
refutableBy: observation
appliesTo:
  - axis: surface
    value: storefront-xapi
  - axis: reward
    value: percentage-off-cart-subtotal
anchors:
  - coordinate: CartType.subTotal
  - coordinate: LineItemType.discountTotal
  - coordinate: DiscountType.amount
evidence:
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-14T08:59:59.575Z
---

Two things have to hold, and neither is announced. FIRST, the base. A percentage-off-cart-subtotal reward is computed against the subtotal AFTER every item-level reward has been taken off, at full precision, and that base is published in no field. subTotal is the LIST subtotal and does not move when a line item is discounted; an item reward appears only as that line own discountTotal and a reduced placedPrice, never as an entry in the discounts collection, which holds cart-level rewards alone. Worked case: list subtotal 236.27, a 10 percent item reward on a 106.28 line takes 10.628, leaving 225.642, and a 25 percent cart reward on THAT is reported as 56.4105 - so 56.4105 over the published 236.27 gives 23.88 percent, not 25. SECOND, the digits. The division is exact only for a reader that is served the unrounded amount, which on an order means REST or the Admin Discounts grid; the storefront projection rounds to currency precision first, and 35.44 over 236.27 is 15.0006 percent where 35.4405 over 236.27 is exactly 15 - see KB-4982C91F for the split itself. So divide only when the discounts collection holds exactly one entry, no line item carries a discount, and the amount still has its trailing digits. Otherwise the rate is not recoverable from the money at all, and the only record of it is whatever a human typed into the promotion description.
