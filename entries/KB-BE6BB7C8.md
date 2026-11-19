---
id: KB-BE6BB7C8
subject: "A product-subscription renewal with a paid shipping method never leaves Scheduled: every run logs \"Validation failed: Shipment: The shipment method price is changed\"; the same flow with $0 pickup renews"
plane: experiential
question: Why does a product subscription renewal stay Scheduled with previewTotal 0 after POST /api/product-subscription/scheduler/run?
questions:
  - text: Does POST /api/product-subscription/renewals/{id}/process work for a subscription shipped with Fixed Rate?
  - text: What does the renewal Error event 'Processing error; will try again later.' mean in product subscriptions?
  - text: Can a subscription with a delivery shipping method renew?
status: active
appliesTo:
  - axis: module
    value: productsubscription
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/product-subscription/scheduler/run
  - coordinate: POST /api/product-subscription/renewals/{id}/process
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: ccb4570264d7de66
    conditions: module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-595d; shipping=FixedRate Ground rate 10; payment=MockStoredCardPaymentMethod
    defect: suspected
    at: 2026-11-16T16:50:25.032Z
    by: session:d9b8e380
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    build: ccb4570264d7de66
    conditions: module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-595d; shipping=FixedRate Ground rate 10
    at: 2026-11-16T17:06:25.056Z
    by: session:d9b8e380
    bodyHash: 3d21cd8dae95
    defect: suspected
    who: kutasinaelena
    note: "Re-seen on a second customer's FixedRate subscription (made due by PUT subscriptions/{id} nextOrderDate +2 days, then scheduler/run): renewal stays Scheduled, previewTotal 0, Error event 'Validation failed: Shipment: The shipment method price is changed'. Corroboration: a one-time xAPI cart with FixedRate shipping rejects addOrUpdateCartShipment with VALIDATION unless the command carries price=10."
  - method: observation
    deployment: vcptcore_qa1
    build: ccb4570264d7de66
    conditions: platform=3.1099.0; module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-595d; module:VirtoCommerce.Cart=3.1014.0; shipping=FixedRate Ground rate 10
    at: 2026-11-16T18:40:39.805Z
    by: session:d9b8e380
    bodyHash: 3d21cd8dae95
    defect: suspected
    who: kutasinaelena
    note: "Fresh repro 2026-11-16 18:24-18:25 UTC: new storefront subscription (FixedRate Ground, initial order shipping 10), PUT nextOrderDate, scheduler/run then renewals/{id}/process: renewal stays Scheduled, previewTotal 0, two Error events with the same 'shipment method price is changed' detail. Source line: RenewalCartBuilder.AddShipmentAsync sets code/option/currency/address but no Price."
  - method: observation
    deployment: vcptcore_qa1
    build: 9dfbb57eacb0bf2c
    conditions: module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-79cf; platform=3.1099.0; shipping=FixedRate Ground rate 10; payment=MockStoredCardPaymentMethod
    at: 2026-11-17T10:47:01.796Z
    by: session:d9b8e380
    bodyHash: 3d21cd8dae95
    who: kutasinaelena
    contradicts: true
    contested: module:VirtoCommerce.ProductSubscription 3.1000.0-pr-1-79cf vs 3.1000.0-pr-1-595d; virtocommerce.productsubscription 3.1000.0-pr-1-79cf vs 3.1000.0-pr-1-595d
    note: On ProductSubscription 3.1000.0-pr-1-79cf a FixedRate/Ground ($10) subscription made due by PUT subscriptions/{id} nextOrderDate +2d reached Notified on scheduler/run (preview 41.50 incl. shipping, reminder sent); renewals/{id}/process then placed the renewal order with the $10 FixedRate shipment and the payment Authorized. A two-subscription FixedRate group also renewed into one order with shipping charged once. The stuck-Scheduled behaviour is gone on this build.
  - method: observation
    deployment: vcptcore_qa1
    build: 1dfb1b0bb5149a92
    conditions: module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-cb70; shipping=FixedRate Ground rate 10; payment=MockStoredCardPaymentMethod
    at: 2026-11-19T15:10:28.039Z
    by: session:d9b8e380
    bodyHash: 3d21cd8dae95
    who: kutasinaelena
    contradicts: true
    contested: module:VirtoCommerce.ProductSubscription 3.1000.0-pr-1-cb70 vs 3.1000.0-pr-1-595d; virtocommerce.productsubscription 3.1000.0-pr-1-cb70 vs 3.1000.0-pr-1-595d; virtocommerce.xpickup 3.1010.0 vs 3.1008.0
    note: On ProductSubscription 3.1000.0-pr-1-cb70 two FixedRate/Ground ($10) subscriptions made due by PUT subscriptions/{id} nextOrderDate = tomorrow reached Notified on scheduler/run (preview 276.00 and 41.50, shipping included, reminder sent) and the next scheduler/run after the cutoff placed both renewal orders with the $10 shipment and payment Authorized. The stuck-Scheduled behaviour does not reproduce on this build.
---
Subscriptions created at storefront checkout (Subscribe & Save, Credit card (test)) and made due by PUT subscriptions/{id} nextOrderDate. With shipping FixedRate/Ground ($10): scheduler/run and renewals/{id}/process both leave the renewal in Scheduled, previewTotal 0, items listPrice 0, and add an Error event 'Processing error; will try again later.' with details 'Validation failed: -- VirtoCommerce.CartModule.Core.Model.Shipment: The shipment method price is changed'. No reminder, no order. With BuyOnlinePickupInStore ($0) the same steps reach Notified (preview total computed, reminder sent) and then Ordered. Source reading (feat branch): RenewalCartBuilder.AddShipmentAsync creates the shipment without its Price before AddShipmentAsync(shipment, rates), so the cart validator sees a price mismatch whenever the rate is non-zero.
