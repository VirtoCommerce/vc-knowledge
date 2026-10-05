---
id: KB-F8F2CE8B
subject: A wishlist scope can be Private, AnyoneAnonymous or Organization, more than the schema doc names
plane: experiential
question: What values can a wishlist's scope actually be set to?
questions:
  - text: Can I share my shopping list so that people without an account can see it?
  - text: Which sharing options does the list settings dropdown offer, and what does each one store?
  - text: Is the scope field description on the create-wishlist input complete, or are there undocumented values?
  - text: Which wishlist scope value makes a list readable by anonymous callers?
concepts:
  - id: wishlist
  - id: list-sharing
status: active
appliesTo:
  - axis: surface
    value: xapi
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: InputCreateWishlistType.scope
  - coordinate: WishlistType.scope
evidence:
  - method: observation
    deployment: vcptcore_stable
    at: 2026-09-13T07:46:06.085Z
    by: session:6341e00a
    splitFrom: KB-E367DA11
  - method: observation
    deployment: vcst
    at: 2026-09-29T09:39:04.652Z
    by: session:p26336
    who: Lenajava1
    contradicts: true
    note: "Theme 2.59 on the rep's /account/lists: a Customer-scope list shows the badge 'Shared with customer', a third label besides Private; the badge is not limited to Private/Shared."
    splitFrom: KB-E367DA11
---
Three values, not the two the schema's doc string names. InputCreateWishlistType.scope is documented 'List scope (private or organization)', but the storefront's Sharing options dropdown offers Private, 'Anyone (readonly)' and Organization, and those persist as scope Private, AnyoneAnonymous and Organization - AnyoneAnonymous appears in no documentation string anywhere in the contract and is the one that exposes a list to callers with no account. Treat the gloss on that field as incomplete and read the value back after writing it.
