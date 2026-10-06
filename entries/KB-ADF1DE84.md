---
id: KB-ADF1DE84
subject: The cart gift row's checkbox carries a wrong accessible name copied from the product table's select-all
plane: experiential
question: What accessible name does the gift checkbox in the cart's "+ Add a gift" panel expose?
questions:
  - text: What does a screen reader announce for the gift checkbox on the cart page?
  - text: Does the add-a-gift checkbox have a correct accessible label for a WCAG audit?
  - text: Which aria-label is set on the gift row checkbox in the cart gift panel?
  - text: Is the gift selector on the basket page labelled as a vendor toggle for assistive technology?
concepts:
  - id: accessibility
  - id: gift
status: active
appliesTo:
  - axis: currency
    value: usd
  - axis: promotion
    value: auto gift (no coupon)
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: Query.cart
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-21T08:02:27.505Z
    by: session:local_e8
    splitFrom: KB-0B27E984
  - method: observation
    deployment: vcst_qa
    at: 2026-09-21T08:13:08.841Z
    by: session:local_e8
    note: "Corroborated from the BACKEND, and the stored record shows the gift line is well-formed rather than junk: GET /api/order/customerOrders/e63d3ee1-9437-4285-aa45-e030c507030d (CO260921-00002) has items[0] productId 08c33cfc9f664426a52fac8882da2df0, sku 566903892, quantity 1, price/placedPrice/extendedPrice all 0, taxTotal 0, isGift=true, and discounts[0] = {promotionId 782b9d77-fb67-4a4b-a80f-5e86998fdfb3, name \"Auto Gift (no coupon)\", discountAmount 0}. It correctly contributes nothing to money: subTotal 526 is the other line alone and total 811.2 reconciles without it. GET /api/marketing/promotions/782b9d77-fb67-4a4b-a80f-5e86998fdfb3 shows why it always fires — isActive true, hasCoupons false, storeIds [\"B2B-store\"], 2026-07-06 to 2026-12-31, dynamicExpression = BlockCustomerCondition{ConditionIsEveryone} + an EMPTY BlockCatalogCondition + an EMPTY BlockCartCondition + BlockReward{RewardItemGiftNumItem productId 08c33cfc…, quantity 1}: an unconditional gift reward for everyone, so the backend behaviour is correct per the promotion as configured. Two additions to the entry's scope. (1) It is not specific to this order or to configurable products: POST /api/order/customerOrders/search {storeIds:[\"B2B-store\"], take:12, sort:\"createdDate:desc\"} returns a gift line on 11 of the last 12 orders, across three different shoppers and both configured and plain carts. (2) The gift IS visible over the API — GraphQL Query.order items{isGift} returns true for it — so a storefront order page that omits it from the product table is choosing to, not missing the data."
    splitFrom: KB-0B27E984
  - method: observation
    deployment: vcst_qa
    at: 2026-09-21T08:13:08.841Z
    by: session:local_e8
    note: "Corroborated from the BACKEND, and the stored record shows the gift line is well-formed rather than junk: GET /api/order/customerOrders/e63d3ee1-9437-4285-aa45-e030c507030d (CO260921-00002) has items[0] productId 08c33cfc9f664426a52fac8882da2df0, sku 566903892, quantity 1, price/placedPrice/extendedPrice all 0, taxTotal 0, isGift=true, and discounts[0] = {promotionId 782b9d77-fb67-4a4b-a80f-5e86998fdfb3, name \"Auto Gift (no coupon)\", discountAmount 0}. It correctly contributes nothing to money: subTotal 526 is the other line alone and total 811.2 reconciles without it. GET /api/marketing/promotions/782b9d77-fb67-4a4b-a80f-5e86998fdfb3 shows why it always fires — isActive true, hasCoupons false, storeIds [\"B2B-store\"], 2026-07-06 to 2026-12-31, dynamicExpression = BlockCustomerCondition{ConditionIsEveryone} + an EMPTY BlockCatalogCondition + an EMPTY BlockCartCondition + BlockReward{RewardItemGiftNumItem productId 08c33cfc…, quantity 1}: an unconditional gift reward for everyone, so the backend behaviour is correct per the promotion as configured. Two additions to the entry's scope. (1) It is not specific to this order or to configurable products: POST /api/order/customerOrders/search {storeIds:[\"B2B-store\"], take:12, sort:\"createdDate:desc\"} returns a gift line on 11 of the last 12 orders, across three different shoppers and both configured and plain carts. (2) The gift IS visible over the API — GraphQL Query.order items{isGift} returns true for it — so a storefront order page that omits it from the product table is choosing to, not missing the data."
    splitFrom: KB-0B27E984
  - method: observation
    deployment: virtostart
    at: 2026-09-23T08:13:47.060Z
    by: session:70b7199f
    who: Dan-BV
    note: Reproduced on the smoke run. The cart's "+ Add a gift" block was pre-checked with a Canon printer; the cart product table, item count and $200.00 subtotal never mentioned it, yet createOrderFromCart returned it as a second line item with isGift:true, price $0.00 and discounts[{promotionName:"Auto Gift (no coupon)"}], and the GA4 purchase event carried it as a second entry in items[] with items_count 2. So a smoke case that asserts "confirmation line items == cart line items" or an items[] length will see an extra row it never added.
    splitFrom: KB-0B27E984
---
On the cart page's "+ Add a gift" panel, the gift row's checkbox carries aria-label "Toggle vendor select", copied from the product table's select-all.
