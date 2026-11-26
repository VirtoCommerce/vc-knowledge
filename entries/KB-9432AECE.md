---
id: KB-9432AECE
subject: On the /cart line quantity field, clearing digit by digit commits the last half-deleted digit on blur, and slow typing commits every partial value
plane: experiential
question: On /cart, what does the line quantity field send when the quantity is cleared with Backspace digit by digit, or typed one digit at a time slowly?
questions:
  - text: Does the cart line stepper send partial quantities while typing?
  - text: What quantity is committed when a cart line quantity is backspaced to empty and blurred?
  - text: Does select-all plus Backspace on a /cart line revert the quantity?
status: active
appliesTo:
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
  - coordinate: Mutation.changeCartItemsQuantity
evidence:
  - method: observation
    deployment: vcst_dev
    build: 7fd3fb4cc4be53eb
    conditions: theme=2.63.0-pr-2674-fc38; role=personal customer
    defect: VCST-6768
    at: 2026-11-18T20:29:45.101Z
    by: session:feee22e7
---
Signed-in personal customer, B2B-store, theme 2.63.0-pr-2674. Line at 234: ArrowRight to the end, three Backspaces back to back -> field empty and focused, nothing sent for 2 s and totals unchanged; clicking outside sets the field to 2 and sends ChangeFullCartItemsQuantity quantity 2, which persists after reload. Line at 1: double-click select, then typing 2, 3, 4 about 1.5 s apart with focus kept sends ChangeFullCartItemsQuantity 2, 23 and 234 before any blur; final 234 persists. Double-click select + one Backspace + blur reverts to 234 and sends nothing.
