---
id: KB-5CEB51A1
subject: "Share dialog: Undo after Clear all restores recipients but Share stays disabled when nothing changed"
plane: experiential
question: is the Share button enabled after Clear all then Undo on an already-shared list
status: active
appliesTo:
  - axis: feature
    value: sales-rep-list-sharing
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/lists
  - coordinate: Mutation.changeWishlist
evidence:
  - method: observation
    deployment: vcptcore_qa
    at: 2026-09-29T12:22:06.269Z
    by: session:p20808
    who: Aleksandra-Mitricheva
---
On theme 2.59.0-pr-2476, reopening Share on a list already shared to one customer shows Share disabled (no changes). Clear all shows 'Cleared · 1' with Undo, the hint 'An empty list does not stop sharing — change who can access instead.' and Share disabled. Undo restores the row but Share remains disabled, because the form is back to its persisted state. Cancel after Clear all persists nothing. Toast copy: a save that adds recipients reads 'List shared with N customers.' (also on a second save that adds one more), a save that only removes or changes the message reads 'List saved. Now shared with N customer(s).'
