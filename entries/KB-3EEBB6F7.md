---
id: KB-3EEBB6F7
subject: Query.subscriptionOffers returns one offer per product that has a subscription plan, with price, discountAmount and subscriptionPrice computed per discount type, and nothing for a product without a plan
plane: experiential
question: What does Query.subscriptionOffers return for products with a PERCENT, FIXED_AMOUNT or NONE plan, and for a product without a plan?
questions:
  - text: How is the Subscribe & Save price calculated in xAPI?
  - text: Does subscriptionOffers return an entry for a product with no subscription plan?
  - text: What discountAmount does a NONE-discount subscription plan return?
status: active
appliesTo:
  - axis: store
    value: b2b-store
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.subscriptionOffers
evidence:
  - method: observation
    deployment: vcptcore_qa1
    conditions: theme=2.63.0-pr-2602-399d; store=B2B-store; currency=USD
    at: 2026-11-16T15:57:02.044Z
    by: session:d9b8e380
    who: kutasinaelena
---
Signed-in buyer, store B2B-store, USD. subscriptionOffers(storeId, productIds:[4 ids], currencyCode, cultureName) returned exactly one offer for each of the 3 products bound to a plan and none for the 4th product (no plan). PERCENT 10 on a $7.50 list price: discountAmount $0.75, subscriptionPrice $6.75. FIXED_AMOUNT 1 on $5.40: discountAmount $1.00, subscriptionPrice $4.40. NONE on $4.40: discountAmount $0.00, subscriptionPrice $4.40. Each offer carries its plan's intervals list (3, 2 and 2 entries). Querying 3 other products without plans returned an empty list. The offers were returned even though all 4 products had zero stock, so the offer is independent of availability.
