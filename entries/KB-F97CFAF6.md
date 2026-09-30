---
id: KB-F97CFAF6
subject: The returnable-quantity limit is enforced at submitReturn, not when a return draft is created or updated
plane: experiential
question: Does createReturn refuse a return quantity above the order line's returnable quantity, or only submitReturn?
questions:
  - text: I tried to send back more items than I bought and got an error only when submitting - why was the draft saved?
  - text: Can a return draft hold a quantity above what is returnable, and at which step is it rejected?
  - text: Do createReturn and updateReturn validate the requested quantity against the returnable limit?
  - text: After a partial approval, how is the remaining returnable quantity of an order line calculated?
  - text: What message does the buyer see when submitting a return whose quantities exceed what can be returned?
concepts:
  - id: returnable-quantity
  - id: return
status: active
appliesTo:
  - axis: surface
    value: xapi
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: Mutations.createReturn
  - coordinate: Mutations.submitReturn
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-28T21:52:36.013Z
    by: session:p35464
    who: kutasinaelena
    splitFrom: KB-5A6C014E
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-29T05:46:26.252Z
    by: session:p5696
    who: kutasinaelena
    contradicts: true
    note: "Theme 2.59 pr-2500: on the details step (/account/returns/:id/edit) typing past the max (5 -> End -> '0') leaves the spinbutton DISPLAYING '50 of 5 available' after blur, while the saved draft quantity stays 5 (autosave clamps) and Submit becomes enabled once reason+photo exist; a reload shows 5. The select-items step likewise shows '50' in the field while the footer counts 5 items and Continue creates the draft with 5. So the stored value is clamped but the displayed value is not."
    splitFrom: KB-5A6C014E
---
On the Return module (3.1003 pr-27 build) with theme 2.59 pr-2500, the buyer mutation createReturn with an item quantity above the line's returnable quantity (13 of 12, 261 of 260) returned 200 and created a Draft carrying the over-limit quantity. submitReturn on it is refused and the storefront shows 'Some items are no longer available to return in the quantity you asked for. Adjust the quantities and try again.'; the return stays Draft. So the limit is enforced at submit, not at draft create/update. Returnable quantity after a partial approval is ordered minus held, where a decided line holds approvedQuantity: 500 ordered, 240 requested, 200 approved gave 300 returnable.
