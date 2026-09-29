---
id: KB-A871AA8D
subject: changeWishlist legacy sharedWithId input becomes a one-element target set
plane: experiential
question: what does changeWishlist do with the old sharedWithId input on a Customer share
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.changeWishlist
evidence:
  - method: observation
    deployment: vcst
    at: 2026-09-29T10:54:28.434Z
    by: session:p24564
    who: Lenajava1
  - method: observation
    deployment: vcst
    at: 2026-09-29T10:54:29.080Z
    by: session:p3684
    who: Lenajava1
---
On x-cart 3.1037 (pr-141), changeWishlist(scope Customer, sharedWithId <org>) on a Private list is applied as sharingSetting.targets = [that org] (a one-element set), not dropped; message stays null.
