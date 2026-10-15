---
id: KB-A0EC64CD
subject: Approving a quote with an out-of-stock line places the order without any stock warning
plane: experiential
question: On /account/quotes/{id}, when a Proposal sent quote contains a line whose product is out of stock, does Approve warn or block, or place the order anyway?
status: active
appliesTo:
  - axis: surface
    value: storefront
anchors:
  - coordinate: /account/quotes/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: 7ec352df61232ca8
    conditions: platform=3.1083.0; theme=2.60.0-alpha.2579; role=B2B organization buyer
    defect: suspected
    at: 2026-10-15T13:52:23.631Z
    by: session:10db9d7d
    who: Lenajava1
---
As a B2B buyer, opened a Proposal sent quote with two lines, one of them a product with zero stock at the time (and still zero when checked). The quote detail showed no stock warning on that line. Clicking Approve placed an order immediately, with both lines, including the out-of-stock one, and no warning, dialog or availability check was shown. The approve path does not pass through the checkout stock validation. Observed twice in regression runs on 2026-10-15. Whether quote lines should bypass the stock check is not documented.
