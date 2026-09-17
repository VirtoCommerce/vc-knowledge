---
id: KB-D7BD6FF7
subject: BL-PAY-003 Successful card payment creates a paid order with a recorded transaction
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
  - coordinate: /account/orders
  - coordinate: Mutations.createOrderFromCart
  - coordinate: /cart
evidence:
  - method: observation
    at: 2026-09-17T15:21:24.581Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-PAY-003: Successful card payment creates a paid order with a recorded transaction `[P0-revenue]`

- **Rule:** On a successful tokenized card payment the order is created, the cart is cleared, the user reaches the confirmation page with an order number, and the order persists the payment-method label and the processor transaction id (visible in `/account/orders` and admin). Raw PAN never appears in storefront network payloads (SDK tokenization).
- **Verify:** Complete a valid card payment → confirmation page with order number; cart badge empty; order in `/account/orders` and admin shows the processor method + a transaction id; no raw card number in any request body; `createOrderFromCart` `errors[]` empty.
- **Violation signal:** Stuck on `/cart` or payment page after submit; no confirmation/order number; cart not cleared; missing transaction id; raw PAN present in network POST bodies.
- **Agents:** qa-frontend-expert, qa-backend-expert
- **Source:** suite 040b PAY-AN-014 (+ deprecated 004/005 admin transaction-record shape); VCST-5162; backend transaction-record change (Status=short enum, ResponseCode=TransactionResponseCode). See BL-ORD-006 (payment state machine).
- **Promoted:** 2026-06-15.
