---
id: KB-59C8EB4D
subject: Admin shipping and payment method search filter by storeId, isActive and codes; payment search also returns unsaved methods with storeId null
plane: experiential
question: Which request fields of POST /api/shipping/search and POST /api/payment/search filter by store, and does the result contain only the store's saved methods?
questions:
  - text: How do I check whether a shipping method code is active in a store via REST?
  - text: Why does /api/payment/search return payment methods with storeId null?
  - text: How to verify a store's active payment method codes through the admin API?
status: active
appliesTo:
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/shipping/search
  - coordinate: POST /api/payment/search
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: fc83686d39d800c3
    conditions: platform=3.1090.0-alpha.13438; module:VirtoCommerce.Payment=3.1011.0-pr-78-2c48; module:VirtoCommerce.Shipping=3.1010.0-pr-73-2440
    at: 2026-10-27T16:17:52.052Z
    by: session:54733023
    who: kutasinaelena
---
On vcptcore_qa1, POST /api/shipping/search and POST /api/payment/search accept storeId, isActive and codes as filters. The payment search with storeId also returns registered-but-unsaved payment methods whose storeId is null, so a check of "is code X active in store S" must match result.storeId === S explicitly rather than trust the storeId filter. Used to verify that FixedRate and DefaultManualPaymentMethod are active in B2B-store and that AGENT-TEST-NoSuch* codes are absent.
