---
id: KB-DC4E2D6D
subject: An order placed by approving a quote carries no quote reference, and the quote shows no order reference
plane: experiential
question: After Approve on /account/quotes/{id}, does the resulting order on /account/orders/{id} show the quote number, and does the quote detail show the order it produced?
status: active
appliesTo:
  - axis: surface
    value: storefront
anchors:
  - coordinate: /account/quotes/{id}
  - coordinate: /account/orders/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: 7ec352df61232ca8
    conditions: platform=3.1083.0; theme=2.60.0-alpha.2579; role=B2B organization buyer
    at: 2026-10-15T13:53:18.082Z
    by: session:10db9d7d
    who: Lenajava1
---
As a B2B buyer, approved Proposal sent quotes; each Approve immediately placed an order and set the quote to Ordered. The order detail page showed no quote number or link back to the quote, and the quote detail and quotes list row showed no order number or link to the order. The only way to correlate them was by time and buyer. Observed on several approvals in a regression run on 2026-10-15. No documentation states that either cross-reference should exist.
