---
id: KB-FBE1C7BC
subject: A renewal whose authorization expired is re-authorized and then captured when its order shipment is set to Sent; no void of the replaced authorization is recorded with the mock method
plane: experiential
question: What happens to a product subscription renewal payment when the order ships after ProductSubscription.Payments.AuthorizationLifetimeMinutes has passed?
questions:
  - text: Is a renewal payment captured at shipment?
  - text: Does an expired renewal authorization get re-authorized before capture?
  - text: Is the old authorization voided on re-authorization?
status: active
appliesTo:
  - axis: module
    value: productsubscription
  - axis: surface
    value: rest-api
anchors:
  - coordinate: PUT /api/order/customerOrders
  - coordinate: GET /api/product-subscription/renewals/{id}
evidence:
  - method: observation
    deployment: vcptcore_qa1
    conditions: module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-595d; payment=MockStoredCardPaymentMethod; setting:ProductSubscription.Payments.AuthorizationLifetimeMinutes=1
    at: 2026-11-16T17:30:03.198Z
    by: session:d9b8e380
    who: kutasinaelena
---
Global setting AuthorizationLifetimeMinutes set to 1 before the renewal was charged (the expiry is stamped at authorization: authorizationExpiresDate = authorizedDate + 1 min). After expiry, shipment of the renewal order set to Sent in the Admin Shell order blade and the order saved: within ~1 s the renewal history shows 'Re-authorization: Succeeded' (new transaction id), then 'Captured 42.50 USD after shipment', state Authorized -> Completed; payment Paid, outerId replaced by the new transaction id, no voidedDate and no void event for the old authorization. Source reading: RenewalProcessor.ReauthorizeAsync calls ProcessStoredPaymentAsync with Reason=Reauthorization and makes no void call — the void is left to the payment connector (the Authorize.Net PR claims it voids), the mock method does not void. Order status stays New after capture. The setting holds null by default; POST /api/platform/settings with value null did not clear a stored value (re-read still 1) — restore with the explicit value.
