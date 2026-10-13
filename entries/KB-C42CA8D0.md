---
id: KB-C42CA8D0
subject: UCP checkout_and_handoff applies the agent's shipping_address to the cart, and the storefront /cart handoff landing shows it as the shipping and billing address
plane: experiential
question: After a UCP checkout_and_handoff with a shipping_address, which shipping address does the buyer see on /cart/{id}?ucp_handoff=1 — the agent's or the buyer's saved one?
status: active
appliesTo:
  - axis: buyer
    value: authenticated-b2b
  - axis: module
    value: virtocommerce.ucp 3.1010.0
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /ucp/mcp
  - coordinate: /cart/{id}
  - coordinate: /ucp/v1/internal/handoff/restore
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T13:13:55.635Z
    by: session:624d35a1
    who: kutasinaelena
    build: ad2e031743ee94a0
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T14:54:31.258Z
    by: session:624d35a1
    who: kutasinaelena
    note: "Re-observed on a fresh cart: shipping and billing (same-as-shipping flag) show the agent's 123 Market St; the cart stores only a Shipping address; the buyer contact's saved addresses are byte-identical before and after the handoff (read via /api/members/{memberId})."
---
2026-10-13, UCP 3.1010.0, theme 2.59.0, B2B-store: an authenticated org buyer whose contact holds one saved address (456 QA Avenue, New York, not default) called /ucp/mcp create_cart then checkout_and_handoff with shipping_address 123 Market St, Los Angeles CA 90012. The handoff payload's cart shipments[].delivery_address and Platform GET /api/carts/{id} shipments both carry 123 Market St. Opening continue_url (already signed in) landed on /cart/{id}?ucp_handoff=1 showing Shipping address = 123 Market St, Los Angeles, California, 90012, billing 'Same as shipping address' with the same line, Delivery method and Payment method unselected and Place order disabled. The buyer's saved contact address list was not modified. Matches the UCP Web API doc (shipping_address/billing_address are applied to the cart through xCart addOrUpdateCartAddress). An SBTM on UCP 3.1006.0-pr-7 (2026-09-22) had recorded the opposite (buyer's own address shown), so suite cases written from that observation are stale.
