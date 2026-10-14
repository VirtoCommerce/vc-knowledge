---
id: KB-49AF58AC
subject: With Subscription 3.1006.0-pr-128-5c31 the CreateSubscriptionsFromOrdersJob succeeds on the first attempt, creates exactly one subscription per plan-product order, and links it both ways (order.subscriptionId and subscription.customerOrdersIds)
plane: experiential
question: After Subscription PR #128 head 5c31, does placing an order for a product with a payment plan create exactly one subscription linked to the order without Hangfire retries?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.subscription@3.1006.0-pr-128-5c31
  - axis: surface
    value: background-job
anchors:
  - coordinate: VirtoCommerce.SubscriptionModule.Data.BackgroundJobs.CreateSubscriptionsFromOrdersJob
  - coordinate: GET /api/subscriptions/{id}
  - coordinate: GET /api/order/customerOrders/{id}/changes
  - coordinate: POST /api/subscriptions/plans
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-14T05:20:22.809Z
    by: session:fbaed2e9
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-14T09:40:18.216Z
    by: session:32f2c1b5
    who: kutasinaelena
    note: "Held on the RELEASED hotfix Subscription 3.1005.1 (BackgroundJobs 3.1052.1, Orders 3.1018.0), orders created via REST POST /api/order/customerOrders with a DefaultManualPaymentMethod in-payment: plan and mixed orders each got one CreateSubscriptionsFromOrdersJob (payload OrderIds only) Succeeded on attempt 1, exactly one Unpaid subscription linked both ways; no-plan order: job Succeeded, no subscription; two no-op jobs (prototype id, empty OrderIds) also Succeeded; NewSubscriptionEmailNotification built and Sent; change log 1 Added per order. Requeued legacy 3.1005.0-format failed jobs (full-order payload) Succeeded. POST /api/subscriptions/cancel -> 200 Cancelled + SubscriptionCanceledEmailNotification, plus one no-op empty-OrderIds job."
---
On Platform 3.1079.0 (PR build), BackgroundJobs 3.1053.0-pr-7, Orders 3.1018.0, Subscription 3.1006.0-pr-128-5c31: four storefront orders (xAPI createOrderFromCart, DefaultManualPaymentMethod) containing a product with a PaymentPlan (Months/1/trial 0), incl. one mixed with a no-plan line, each got one CreateSubscriptionsFromOrdersJob that went Created→Enqueued→Processing→Succeeded with no retry (0.3–8 s). Each order got exactly one Unpaid subscription; GET /api/subscriptions/{id} shows customerOrdersIds=[orderId] and a loaded isPrototype=true CustomerOrderPrototype; the order got subscriptionId/subscriptionNumber and nothing else changed except modifiedDate/rowVersion. NewSubscriptionEmailNotification was built per subscription (recipient = buyer email, en-US). A no-plan order: job Succeeded, no subscription. Requeuing a legacy 3.1005-format failed job also succeeded and linked the order, keeping the order's later Completed status. Each run also enqueues two no-op CreateSubscriptionsFromOrdersJob runs: one for the new prototype order id, one with empty OrderIds from the job's own order write-back. The subscriptionId write-back adds no entry to GET /api/order/customerOrders/{id}/changes (SubscriptionId is not [Auditable]), and modifiedBy stays the user from the job payload (UserName), not a system account. This supersedes, for this build, the NRE/duplicate behaviour observed on 3.1006.0-pr-128-7611 (KB-1E269E42).
