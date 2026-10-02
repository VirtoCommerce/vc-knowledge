---
id: KB-F7E4DB8E
subject: customerOrders/search keyword is a prefix match, results newest first
plane: experiential
question: Does POST /api/order/customerOrders/search with keyword=<order number> return only that order?
status: active
appliesTo:
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/order/customerOrders/search
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-02T05:35:39.327Z
    by: session:0cfc9f97
    who: kutasinaelena
---
No. The keyword matches order numbers by prefix: keyword AGENT-TEST-ORD-RET-D returned 11 orders (nine AGENT-TEST-ORD-RET-DEC-* plus two exact matches), and keyword AGENT-TEST-ORD-RET-A also returns AGENT-TEST-ORD-RET-ADM073 and AGENT-TEST-ORD-RET-A2. Results came back newest createdDate first, so an exact-number lookup with a small take (5) can miss the exact order behind newer prefix matches, and code that falls back to the first result picks a DIFFERENT order. Filter the results to number === the wanted number, and use a take large enough to reach it.
