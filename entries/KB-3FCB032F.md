---
id: KB-3FCB032F
subject: raising a price list assignment's priority makes its list win over another unconditional list at the same scope
plane: experiential
question: how do I make one price list win over another at the same scope
questions:
  - text: Two price lists apply to the same store -- how do I choose which one shoppers get?
  - text: Does raising a pricing assignment's priority change which price list wins in price evaluation?
  - text: Can I change only the priority of a price list assignment with a GET-then-PUT round trip?
concepts:
  - id: price-list
status: active
appliesTo:
  - axis: surface
    value: rest
anchors:
  - coordinate: PUT /api/pricing/assignments
evidence:
  - method: observation
    deployment: virtostart
    at: 2026-09-25T20:12:18.174Z
    by: session:p26612
    who: Lenajava1
    splitFrom: KB-24AA6296
---
Raising an assignment's priority via GET + PUT /api/pricing/assignments with only priority changed made that list win over another unconditional list at the same scope, as seen in POST /api/pricing/evaluate.
