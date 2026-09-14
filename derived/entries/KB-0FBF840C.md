---
id: KB-0FBF840C
subject: rest-api-subscriptions
plane: derived-first
question: Which endpoints does this deployment serve under /api/subscriptions, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.Subscription
    version: 3.1000.0
anchors:
  - coordinate: DELETE /api/subscriptions
    operationId: SubscriptionModule_DeleteSubscriptionsByIds
    hash: 40aa6f0900c3
  - coordinate: DELETE /api/subscriptions/plans
    operationId: SubscriptionModule_DeletePlansByIds
    hash: 93ed3e262bff
  - coordinate: GET /api/subscriptions
    operationId: SubscriptionModule_GetSubscriptionByIds
    hash: d2aa1367598a
  - coordinate: GET /api/subscriptions/{id}
    operationId: SubscriptionModule_GetSubscriptionById
    hash: 81b1477b2146
  - coordinate: GET /api/subscriptions/plans
    operationId: SubscriptionModule_GetPaymentPlanByIds
    hash: 10d120384ffa
  - coordinate: GET /api/subscriptions/plans/{id}
    operationId: SubscriptionModule_GetPaymentPlanById
    hash: 007b9e1e577f
  - coordinate: POST /api/subscriptions
    operationId: SubscriptionModule_CreateSubscription
    hash: c934a3ce510a
  - coordinate: POST /api/subscriptions/cancel
    operationId: SubscriptionModule_CancelSubscription
    hash: 44406dc4729a
  - coordinate: POST /api/subscriptions/order
    operationId: SubscriptionModule_CreateRecurrentOrderForSubscription
    hash: 63f4e5eeac42
  - coordinate: POST /api/subscriptions/plans
    operationId: SubscriptionModule_CreatePaymentPlan
    hash: 1ec3570a0026
  - coordinate: POST /api/subscriptions/plans/plenty
    operationId: SubscriptionModule_GetPaymentPlansByPlentyIds
    hash: e4ae99816e22
  - coordinate: POST /api/subscriptions/search
    operationId: SubscriptionModule_SearchSubscriptions
    hash: 35c77f211665
  - coordinate: PUT /api/subscriptions
    operationId: SubscriptionModule_UpdateSubscription
    hash: 2a69495b48e9
  - coordinate: PUT /api/subscriptions/plans
    operationId: SubscriptionModule_UpdatePaymentPlan
    hash: b5abf76eed21
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/subscriptions

14 operations under `/api/subscriptions`, served by module `VirtoCommerce.Subscription`, published under the tag "Subscriptions".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `DELETE /api/subscriptions`<br>`SubscriptionModule_DeleteSubscriptionsByIds` | Delete subscriptions | — | 204 |
| `DELETE /api/subscriptions/plans`<br>`SubscriptionModule_DeletePlansByIds` | Delete payment plans | — | 204 |
| `GET /api/subscriptions`<br>`SubscriptionModule_GetSubscriptionByIds` | — | — | `Subscription[]` |
| `GET /api/subscriptions/{id}`<br>`SubscriptionModule_GetSubscriptionById` | — | `id` (path) | `Subscription` |
| `GET /api/subscriptions/plans`<br>`SubscriptionModule_GetPaymentPlanByIds` | — | — | `PaymentPlan[]` |
| `GET /api/subscriptions/plans/{id}`<br>`SubscriptionModule_GetPaymentPlanById` | — | `id` (path) | `PaymentPlan` |
| `POST /api/subscriptions`<br>`SubscriptionModule_CreateSubscription` | — | body application/json (optional) | `Subscription` |
| `POST /api/subscriptions/cancel`<br>`SubscriptionModule_CancelSubscription` | — | body application/json (optional) | `Subscription` |
| `POST /api/subscriptions/order`<br>`SubscriptionModule_CreateRecurrentOrderForSubscription` | — | body application/json (optional) | `CustomerOrder` |
| `POST /api/subscriptions/plans`<br>`SubscriptionModule_CreatePaymentPlan` | — | body application/json (optional) | `PaymentPlan` |
| `POST /api/subscriptions/plans/plenty`<br>`SubscriptionModule_GetPaymentPlansByPlentyIds` | Gets plans by plenty ids | body `string[]` (optional) | `PaymentPlan[]` |
| `POST /api/subscriptions/search`<br>`SubscriptionModule_SearchSubscriptions` | Search subscriptions by given criteria | body application/json (optional) | `SubscriptionSearchResult` |
| `PUT /api/subscriptions`<br>`SubscriptionModule_UpdateSubscription` | — | body application/json (optional) | `Subscription` |
| `PUT /api/subscriptions/plans`<br>`SubscriptionModule_UpdatePaymentPlan` | — | body application/json (optional) | `PaymentPlan` |

Module `VirtoCommerce.Subscription` — Subscriptions: Represent recurrent subscriptions functionality

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-subscriptions.json`.
