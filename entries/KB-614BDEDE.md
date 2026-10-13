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
    deployment: vcst_qa
    at: 2026-09-29T09:50:59.669Z
    by: session:p35844
    who: Lenajava1
    build: 0d97b54f6d8c4ddd
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-05T12:55:38.123Z
    by: session:bbd60c47
    who: Aleksandra-Mitricheva
    note: Still holds on x-cart 3.1038.0-pr-141 2026-10-05 for owner on Customer and Organization lists, and for a non-owner Write member (200 no-op, not Forbidden).
    build: c84cd38eec6a2c5b
    buildFrom: deploy-history
---
No. On a list already shared with scope Customer, changeWishlist(listId, message) without scope returns 200 with no errors, bumps modifiedDate, and leaves sharingSetting.message at its previous value in both the echo and a fresh wishlist(listId) read. The same message is saved when scope Customer is sent in the same command. Observed on vc-module-x-cart 3.1037.0-pr-141 / cart 3.1011.0-pr-194.
