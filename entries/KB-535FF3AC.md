---
id: KB-535FF3AC
subject: The payment methods a B2B cart offers are the store's active payment methods from availablePaymentMethods, and the set differs per deployment
plane: experiential
question: Which payment methods does the storefront offer a signed-in B2B buyer, and where does that list come from?
questions:
  - text: Which ways to pay can I choose from when buying as a company account?
  - text: Why does the B2B store offer a different number of payment options on one environment than on another?
  - text: Does availablePaymentMethods on the cart return exactly the store's active payment method records?
  - text: Does deactivating a payment method in the store remove it from the buyer's cart options?
  - text: How do the cart's payment options relate to the payment method search results in the platform API?
concepts:
  - id: payment-method
  - id: store
status: active
appliesTo:
  - axis: store
    value: b2b-store
  - axis: user
    value: authenticated-b2b-buyer
  - axis: surface
    value: xapi
  - axis: surface
    value: rest
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: Query.cart.availablePaymentMethods
  - coordinate: POST /api/payment/search
  - coordinate: PaymentMethodType.priority
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-22T07:00:05.746Z
    by: session:2ca7d89e
    splitFrom: KB-44B7B71E
  - method: observation
    deployment: virtostart
    at: 2026-09-23T10:12:46.911Z
    by: session:cdb27d99
    who: Dan-BV
    note: "The ORDERING half holds on virtostart, but the SET is smaller. The B2B-store cart on virtostart (storefront Ver. 2.58.0) offers FOUR payment methods, not six: \"Bank card (Authorize.Net)\", \"Bank card (CyberSource)\", \"Manual\", \"Bank card (Skyflow)\" — Datatrans and \"Pay with points\" are absent on this stand. The rendered order is still alphabetical by method code (AuthorizeNet < CyberSource < DefaultManual < Skyflow), not by configured priority, so the entry's central claim survives. Nothing is preselected, the control reads \"Select a payment method\", and Place order stays disabled with \"Complete all required information to proceed.\" until a method is chosen — all as the entry describes. So the offered set is per-deployment while the sort key is not."
    splitFrom: KB-44B7B71E
---
Observed on store B2B-store, signed in as an organization buyer with one physical item in the cart. The storefront offered six options: "Bank card (Authorize.Net)", "Bank card (CyberSource)", "Bank card (Datatrans)", "Manual", "Pay with points", "Bank card (Skyflow)". Query.cart.availablePaymentMethods returns exactly those six. Their codes/types/priorities: AuthorizeNetPaymentMethod (PreparedForm, priority 1), CyberSourcePaymentMethod (Standard, 0), DatatransPaymentMethod (Standard, 0), DefaultManualPaymentMethod (Unknown, 1), LoyaltyPaymentMethod (Unknown, 0), SkyflowPaymentMethod (PreparedForm, 0). Behind that, POST /api/payment/search returns 21 rows system-wide across all stores; B2B-store has exactly six rows, all isActive=true, with priorities that match the GraphQL priorities one-for-one. Six distinct method codes exist in the whole system, so B2B-store enables all of them. On a second deployment the same store's cart offers FOUR methods, not six: "Bank card (Authorize.Net)", "Bank card (CyberSource)", "Manual", "Bank card (Skyflow)" - Datatrans and "Pay with points" are absent - so the offered set is per-deployment. Every method row on the observed store is isActive=true, so this does NOT demonstrate that setting isActive=false removes a method from the cart - only that the enabled set and the offered set coincide.
