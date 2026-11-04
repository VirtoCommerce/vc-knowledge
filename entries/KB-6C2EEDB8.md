---
id: KB-6C2EEDB8
subject: Query.backInStockSubscriptions keyword "userId:<other>" cannot widen scope on the VCST-6251 fixed build — only the caller's subscriptions return
plane: experiential
question: Can a buyer list another user's back-in-stock subscriptions with keyword "userId:<id>" in Query.backInStockSubscriptions?
questions:
  - text: Does the backInStockSubscriptions keyword override the caller's userId?
  - text: Which subscriptions does Query.backInStockSubscriptions return for a signed-in buyer?
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.backInStockSubscriptions
evidence:
  - method: observation
    deployment: vcst_qa
    build: 4fafc097f6a88f6c
    conditions: module:VirtoCommerce.BackInStock=3.1004.0-pr-13-6f41; store=B2B-store
    at: 2026-11-04T08:15:12.695Z
    by: session:7c4c844c
    who: Lenajava1
---
On vcst_qa (BackInStock 3.1004.0-pr-13-6f41), a signed-in buyer with 2 active subscriptions called Query.backInStockSubscriptions(storeId, keyword: "userId:<another buyer's id>"): totalCount 2, both the caller's own, none of the other buyer's (3 runs). Narrowing still works: productIds:[p] -> 1; keyword = product id -> 1; keyword = product code -> 1 for a subscription created via activateBackInStockSubscription (productCode is stored on it; subscriptions created through admin REST without productCode do not match a code keyword). On pre-fix BackInStock 3.1003.0 (vcptcore) the same keyword returned exactly the other buyer's subscription.
