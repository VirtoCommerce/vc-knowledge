---
id: KB-614BDEDE
subject: changeWishlist without scope silently drops a sharing message change
plane: experiential
question: Does changeWishlist save a new sharing message if the command carries message but no scope?
questions:
  - text: I changed the note on a list I had already shared, but customers still see the old one - why?
  - text: Does updating a shared list's message without resending the scope actually save the message?
  - text: Why does changeWishlist return success and bump modifiedDate yet keep sharingSetting.message unchanged?
  - text: Must the sharing scope be sent in the same command for a new list-share message to persist?
concepts:
  - id: list-sharing
  - id: wishlist
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.changeWishlist
  - coordinate: SharingSettingType.message
evidence:
  - method: observation
    deployment: vcst
    at: 2026-09-29T09:50:59.669Z
    by: session:p35844
    who: Lenajava1
---
No. On a list already shared with scope Customer, changeWishlist(listId, message) without scope returns 200 with no errors, bumps modifiedDate, and leaves sharingSetting.message at its previous value in both the echo and a fresh wishlist(listId) read. The same message is saved when scope Customer is sent in the same command. Observed on vc-module-x-cart 3.1037.0-pr-141 / cart 3.1011.0-pr-194.
