---
id: KB-DAA76AE2
subject: A loyalty OrderValueGoal mission adds the order's final total (after discounts, including tax and shipping) to its progress
plane: experiential
question: Does an OrderValueGoal loyalty mission count the order total after a coupon discount, or the pre-discount subtotal?
questions:
  - text: Is Reach target order value measured on the discounted order total?
  - text: What value does one order add to loyalty mission currentValue for an order value goal?
  - text: Does shipping and tax count toward a loyalty order value mission?
status: active
appliesTo:
  - axis: module
    value: loyalty
  - axis: store
    value: b2b-store
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/loyalty-mission-progress/search
  - coordinate: Query.loyaltyMissionProgress
evidence:
  - method: observation
    deployment: vcptcore_regression
    build: 0a0b8dac8ab16bcc
    conditions: platform=3.1076.8; module:VirtoCommerce.Loyalty=3.1009.0; setting:Loyalty.LoyaltyBalanceCalculationMode=Organization; setting:Loyalty.Mode=Mixed Cart
    at: 2026-11-09T00:20:04.922Z
    by: session:6eca33a7
    who: Aleksandra-Mitricheva
---
On vcptcore_regression (Loyalty 3.1009.0, store B2B-store, Organization balance mode), I read POST /api/loyalty-mission-progress/search records that are owned by one organization. Order CO…-00001 (subTotal 45, tax 29, shipping 100, total 174) moved every OrderValue progress record of that organization to currentValue 174, which is the full order total including shipping and tax. A later order by another member (subTotal 50, coupon -30, tax 4, total 24) raised the still-InProgress OrderValue records from 174 to 198, so it added +24: the discounted total, not 50 or 60. The source agrees: LoyaltyMissionLogicService.ApplyContribution returns order.Total for OrderValueGoal. The mission is processed by a background job after the order is added (OrderChangedEvent), and the total is read from the stored order. A progress record that is already Completed takes no further contribution.
