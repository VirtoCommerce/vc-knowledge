---
id: KB-2F255FCE
subject: POST api/product-subscription/import/legacy defaults to dry run, writes nothing on dryRun, creates one subscription per record on dryRun=false, and on re-import reports SkippedDuplicate by outerId legacy:{Id}
plane: experiential
question: Is the legacy subscription import dry run side-effect free, and is re-importing the same JSON idempotent?
questions:
  - text: What does POST api/product-subscription/import/legacy return without the dryRun parameter?
  - text: Does importing the old Subscription module JSON twice create duplicates?
  - text: Does the legacy import require the customer to exist?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.productsubscription
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/product-subscription/import/legacy
  - coordinate: POST /api/product-subscription/subscriptions/search
evidence:
  - method: observation
    deployment: vcptcore_qa1
    conditions: platform=3.1099.0; module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-595d; role=administrator
    at: 2026-11-16T15:59:39.874Z
    by: session:d9b8e380
    who: kutasinaelena
---
As admin on ProductSubscription 3.1000.0-pr-1-595d, posted the old VirtoCommerce.Subscription search-result shape ({totalCount, results:[{id, interval, intervalCount, subscriptionStatus, currentPeriodEnd, customerOrderPrototype:{items, shipments, inPayments}}]}) with 2 Active records on an existing product with no plan. dryRun=true and no dryRun parameter both returned 200 dryRun:true createdCount 2, and POST /subscriptions/search {outerIds:[legacy:{Id}]} stayed at 0. dryRun=false returned createdCount 2 and search found 2 (numbers SUB<yyMMdd>-NNNNN, outerId legacy:{Id}, planId null, discount None, storedPaymentMethodId null, message "No active plan offers this product in the store; imported without a plan, keeping the legacy frequency."). Re-import returned createdCount 0, skippedCount 2, outcome SkippedDuplicate "Already imported as subscription SUB...", search still 2. Customer ids that do not exist as contacts were accepted. Months/1 and Weeks/2 mapped to Month/1 and Week/2.
