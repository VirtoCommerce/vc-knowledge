---
id: KB-C7C99FCF
subject: POST api/product-subscription/plans rejects missing name/store/frequency, frequency outside 1-365, percent outside 0-100 with 400 {message}; two defaults or no default is silently normalised to one default (the first)
plane: experiential
question: What does POST api/product-subscription/plans validate, and what happens with two default frequencies?
questions:
  - text: Does the subscription plan API reject a plan with two default intervals?
  - text: What error does creating a product subscription plan without a store return?
  - text: Is a subscription plan for a nonexistent store accepted?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.productsubscription
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/product-subscription/plans
  - coordinate: DELETE /api/product-subscription/plans
evidence:
  - method: observation
    deployment: vcptcore_qa1
    conditions: platform=3.1099.0; module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-595d; role=administrator
    at: 2026-11-16T15:59:39.685Z
    by: session:d9b8e380
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    conditions: platform=3.1099.0; module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-595d; role=administrator
    at: 2026-11-16T18:24:19.096Z
    by: session:d9b8e380
    bodyHash: 5701e94a4fa0
    who: kutasinaelena
    note: "Re-run: two defaults and no default both stored first interval as default; only the second marked default was kept as marked. Source SubscriptionPlanService.Validate (595d89d7) documents this normalisation; storeId is checked for blank only, no existence lookup."
---
As admin via REST on ProductSubscription 3.1000.0-pr-1-595d: POST /api/product-subscription/plans answered 400 with {"message": ...} for no/empty name ("Enter a plan name."), no storeId ("Choose a store."), empty intervals ("Add at least one delivery frequency."), interval count 0 or 366 ("Each frequency must be between 1 and 365."), percent 101 or -1 ("A percent discount must be between 0 and 100."), FixedAmount -1 ("The discount can't be negative."), and a repeated unit+count ("Each frequency can be listed only once."). Count 1 and 365 (Day) and percent 0 and 100 were accepted (200). Two intervals both isDefault=true, or none default, were accepted with 200 and stored with only the FIRST interval as default. A storeId that does not exist (AGENT-TEST-nostore) was accepted with 200. DELETE /api/product-subscription/plans?ids=... returned 204 and GET /plans/{id} then 404. The Admin Shell plan editor uses a radio per frequency (one default only) and surfaces the server message as a toast.
