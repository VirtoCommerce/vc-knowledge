---
id: KB-A871AA8D
subject: changeWishlist legacy sharedWithId input becomes a one-element target set
plane: experiential
question: what does changeWishlist do with the old sharedWithId input on a Customer share
questions:
  - text: If a rep shares my private list with my company the old way, who does it end up shared with?
  - text: What happens to the legacy sharedWithId input when changing a list to Customer sharing?
  - text: Is an old single-recipient share call dropped or converted to the new multi-target model?
concepts:
  - id: list-sharing
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.changeWishlist
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-29T10:54:28.434Z
    by: session:p24564
    who: Lenajava1
    build: e66f7e540eab47f4
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    at: 2026-09-29T10:54:29.080Z
    by: session:p3684
    who: Lenajava1
    build: e66f7e540eab47f4
    buildFrom: deploy-history
---
On x-cart 3.1037 (pr-141), changeWishlist(scope Customer, sharedWithId <org>) on a Private list is applied as sharingSetting.targets = [that org] (a one-element set), not dropped; message stays null.
