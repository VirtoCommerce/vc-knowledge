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
---
Signed-in personal user, B2B-store, consolidated cart+checkout page. USD cart with QA-MULTICUR-001 x2 and 'Fixed Rate (Ground)' selected (shipping +$150). Switching the header currency to EUR showed the same line re-priced from the EUR pricelist (EUR22.49 x2) but Delivery method reset to 'Select a delivery method' and Shipping cost EUR0.00. After selecting Ground in EUR, switching back to USD still showed Ground; choosing 'Fixed Rate (Air)' in USD and switching to EUR again showed the EUR cart's earlier Ground (EUR150), not Air. So line items (and their removal — an emptied USD cart also empties the EUR view) follow the switch, while the shipment stays per-currency cart; an applied coupon was also not carried (EUR cart showed no coupon). Fixed-rate shipping is the same numeric amount in both currencies (150 / EUR150).
