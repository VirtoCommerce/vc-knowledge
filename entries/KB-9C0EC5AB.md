---
id: KB-9C0EC5AB
subject: A storefront currency switch carries cart line items to the target-currency cart but not the selected delivery method; each currency's cart keeps its own shipment
plane: experiential
question: On /cart, when the user switches currency (e.g. USD to EUR), is the previously selected delivery method carried over to the new-currency cart?
status: active
appliesTo:
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
  - coordinate: Mutation.changeCartCurrency
evidence:
  - method: observation
    deployment: vcst_qa
    build: e00c89f4da0e9c98
    conditions: platform=3.1083.0; theme=2.60.0-alpha.2579; role=personal customer
    defect: suspected
    at: 2026-10-14T17:45:31.539Z
    by: session:6630b76f
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    at: 2026-10-28T09:55:37.153Z
    by: session:7e8ea7fb
    bodyHash: d2299d649f0c
    who: Lenajava1
    note: "Re-observed 2026-10-28 on /cart: after USD->EUR switch, the QA-MULTICUR-001 line re-priced to EUR22.49 (from $24.99), delivery method showed 'Select a delivery method' with shipping 0.00 in the EUR cart; selecting Fixed Rate (Ground) gave +EUR150.00 (same numeric as USD). Also a product with no EUR price (ALCE0128) showed EUR0.00 with 'The product is no longer available for purchase'."
  - method: observation
    deployment: vcst_qa
    build: 3b66ce25e7926ad6
    conditions: platform=3.1097.0; theme=2.62.0-alpha.2647; module:VirtoCommerce.XApi=3.1031.0-pr-70-108f
    at: 2026-11-12T05:38:57.148Z
    by: session:7e403256
    bodyHash: d2299d649f0c
    who: kutasinaelena
    note: "Re-observed 2026-11-12 (REG-2026-11-12-0714-W3): USD cart with Fixed Rate (Ground) $150 -> EUR: changeCartCurrency response shipments[0] has no method, delivery 'Select a delivery method', shipping EUR0.00; QA-MULTICUR-001 EUR22.49; ALCE0128 EUR0.00 + 'The product is no longer available for purchase'."
  - method: observation
    deployment: vcst_qa
    build: 1fe74fdd17a1b14e
    conditions: platform=3.1101.0; theme=2.63.0-alpha.2681; store=B2B-store; role=personal customer; browser=edge
    at: 2026-11-26T01:20:58.567Z
    by: session:779b94fc
    bodyHash: d2299d649f0c
    who: kutasinaelena
    note: "REG-2026-11-26-0357-W2 CART-049/051 (edge): USD cart with Fixed Rate (Ground) +$150 -> EUR showed 'Select a delivery method', shipping EUR0.00; selecting Ground gave +EUR150 (tax EUR39.00 = 20% of 44.98+150). Back in USD Ground persisted; switching USD to Air ($250) then to EUR showed the EUR cart's Ground EUR150, not Air."
  - method: observation
    deployment: vcst_dev
    build: 7fd3fb4cc4be53eb
    conditions: theme=2.63.0-pr-2674-fc38; role=personal customer
    at: 2026-11-18T20:22:13.270Z
    by: session:feee22e7
    bodyHash: d2299d649f0c
    note: "Re-observed 2026-11-18 on vcst_dev (theme 2.63.0-pr-2674): USD cart with Fixed Rate (Air) +$50 -> EUR: delivery 'Select a delivery method', shipping EUR0.00; QA-SUBFIVE-001 re-priced EUR3.19 (USD 3.49); WH-001 (no EUR price) EUR0.00 + 'The product is no longer available for purchase', stepper disabled; re-selecting Air in EUR gave +EUR50.00 (same numeric)."
  - method: observation
    deployment: vcst_dev
    build: 7fd3fb4cc4be53eb
    conditions: platform=3.1100.0; theme=2.63.0-pr-2674-fc38-fc383c4e; module:VirtoCommerce.XCart=3.1048.0; module:VirtoCommerce.Cart=3.1015.0; module:VirtoCommerce.Xapi=3.1032.0; store=B2B-store; role=personal customer
    at: 2026-11-18T20:57:57.378Z
    by: session:feee22e7
    bodyHash: d2299d649f0c
    defect: suspected
    note: "Re-observed 2026-11-18 (REG-2026-11-18-2014 CART-051), 3/3 switches. USD cart with QA-MULTICUR-001 x2 + Fixed Rate (Ground) $10 -> EUR: changeCartCurrency returned a different cart id (the EUR cart) with shipments [] (first time that cart existed), UI 'Select a delivery method', shipping EUR0.00, subtotal/tax/total re-priced correctly (EUR44.98/0.90/45.88). Chose Air in EUR, switched to USD: response = USD cart's own stale Ground $10, not Air. Switched to EUR again: EUR cart's own Air EUR50, not Ground. Source at XCart 3.1048.0 (ChangeCartCurrencyCommandHandler.Handle) clears and copies Items only — shipments, payments and coupons are never copied to the target-currency cart."
---
Signed-in personal user, B2B-store, consolidated cart+checkout page. USD cart with QA-MULTICUR-001 x2 and 'Fixed Rate (Ground)' selected (shipping +$150). Switching the header currency to EUR showed the same line re-priced from the EUR pricelist (EUR22.49 x2) but Delivery method reset to 'Select a delivery method' and Shipping cost EUR0.00. After selecting Ground in EUR, switching back to USD still showed Ground; choosing 'Fixed Rate (Air)' in USD and switching to EUR again showed the EUR cart's earlier Ground (EUR150), not Air. So line items (and their removal — an emptied USD cart also empties the EUR view) follow the switch, while the shipment stays per-currency cart; an applied coupon was also not carried (EUR cart showed no coupon). Fixed-rate shipping is the same numeric amount in both currencies (150 / EUR150).
