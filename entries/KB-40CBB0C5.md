---
id: KB-40CBB0C5
subject: a promotion's reward is set by editing the BlockReward children array in its dynamicExpression
plane: experiential
question: How do I set a promotion's discount reward through PUT /api/marketing/promotions?
status: active
appliesTo:
  - axis: surface
    value: rest-api
anchors:
  - coordinate: PUT /api/marketing/promotions
  - coordinate: Promotion.dynamicExpression
evidence:
  - method: observation
    deployment: vcst-qa
    at: 2026-09-25T11:17:52.093Z
    by: session:memimpor
    who: Lenajava1
---
The reward lives in the promotion's dynamicExpression tree, whose children are four blocks - customer, catalog, cart conditions and BlockReward - each holding its active nodes in a children array and its menu in availableChildren. Create the promotion, GET it, set the BlockReward block's children to the reward node (for example RewardCartGetOfRelSubtotal for percent off cart, RewardCartGetOfAbsSubtotal for an amount off), and PUT the whole object back, which returns 204. children and availableChildren are arrays; sending them as index-keyed objects returns 500. Exclusivity is the boolean isExclusive, and date fields reject unparseable strings with 500.
