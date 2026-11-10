---
id: KB-DD40D93D
subject: Clearing a storefront quantity stepper digit by digit and blurring restores the last half-deleted value and commits it to the cart (PDP and catalog card)
plane: experiential
question: On the PDP or the /search catalog card quantity stepper, what happens when the quantity is deleted with Backspace digit by digit and the field is blurred, and which mutation is sent?
questions:
  - text: What quantity does the PDP show after I clear the quantity field with Backspace and click away?
  - text: Does the catalog card quantity stepper send a cart mutation for half-deleted values?
  - text: Why does the cart hold 2 after I deleted 234 in the quantity field?
status: active
appliesTo:
  - axis: store-type
    value: b2b
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /search
  - coordinate: Mutation.updateCartQuantity
evidence:
  - method: observation
    deployment: vcst_qa
    build: 01d892bf697c7bac
    conditions: platform=3.1096.0; theme=2.62.0-pr-2629-8fcc-8fccd4be; store=B2B-store; actor=anonymous (PDP) and signed-in buyer (card)
    defect: VCST-6768
    at: 2026-11-10T13:45:07.775Z
    by: session:e07c7870
  - method: observation
    deployment: vcst_qa
    build: 01d892bf697c7bac
    conditions: platform=3.1096.0; theme=2.62.0-pr-2629; store=B2B-store; browser=firefox
    at: 2026-11-10T14:01:18.157Z
    by: session:e07c7870
    bodyHash: 12d35fca6d21
    defect: VCST-6768
    who: Lenajava1
    note: "Independent re-observation on a second browser engine (firefox): PDP anonymous 234 -> three Backspaces -> empty and focused, nothing sent, 'in Cart' stays 234 -> blur -> field 2, UpdateShortCartItemQuantity 2, 'in Cart' 2, /cart 2. Signed-in /search card 345 -> blur -> 3, UpdateShortCartItemQuantity 3, 'in Cart' 3, /cart 3. On the /cart line the same clearing does not wait for blur: ChangeFullCartItemsQuantity 2 is sent about 1 s after the last Backspace while the field is still focused, and the empty field then shows 2 (seen twice). Select-all (double-click) + one Backspace + blur on /cart: back to 234, nothing sent, reload 234."
---
On vcst_qa (B2B-store), with the stepper at 234 (anonymous on the PDP) or 345 (signed-in on the /search card), Backspace three times and a click outside leaves the field, the 'in Cart' indicator and the persisted cart quantity at the first digit (2, or 3 on the card), never 0 and never the pre-edit value, and sends UpdateShortCartItemQuantity (mutation updateCartQuantity) with that digit; /cart holds it after a reload. With the three presses issued back to back nothing is sent while the field is empty and focused, the commit happens on blur. With about 1.2 s between presses the partial values 23 and then 2 are each committed through UpdateShortCartItemQuantity before the blur (the PDP then also showed 'in Cart' 23 and 2 while editing). Selecting the whole content and pressing Backspace once, then blurring, instead put the old value back on /cart and sent nothing.
