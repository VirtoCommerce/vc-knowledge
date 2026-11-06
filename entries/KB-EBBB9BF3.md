---
id: KB-EBBB9BF3
subject: POST /api/loyalty-missions accepts an OrderValueGoal currencyCode that is not a known currency or is lowercase, and stores it verbatim
plane: experiential
question: Does POST /api/loyalty-missions validate the OrderValueGoal currencyCode beyond presence?
questions:
  - text: Is an unknown currency code like XYZ rejected on a loyalty mission order value goal?
  - text: Is a lowercase currency code normalised on a loyalty mission?
  - text: What currency validation does the loyalty mission validator do?
status: active
appliesTo:
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/loyalty-missions
evidence:
  - method: observation
    deployment: vcptcore_regression
    build: 99880d06bbb9b4cf
    conditions: platform=3.1076.8; module:VirtoCommerce.Loyalty=3.1009.0; role=Administrator
    defect: suspected
    at: 2026-11-06T10:48:15.732Z
    by: session:6eca33a7
    who: Aleksandra-Mitricheva
---
On Loyalty 3.1009.0, POST /api/loyalty-missions with an OrderValueGoal currencyCode "XYZ" returned 200, and with "usd" returned 200; GET /api/loyalty-missions/{id} read back "XYZ" and "usd" unchanged. Only an empty or null currencyCode is rejected (400 "Currency code is required for the order value goal"). The 3.1009.0 LoyaltyMissionValidator has only the presence rule. A mission with an unknown or wrong-case code is persisted as a normal Draft.
