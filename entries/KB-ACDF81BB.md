---
id: KB-ACDF81BB
subject: On theme 2.59.0-pr-2476 a non-owner member of an Organization-scope list is offered only Rename — no Share, no Remove list — on the card menu and the list page
plane: experiential
question: which list actions does /account/lists offer a non-owner member of an Organization-scope list
status: active
appliesTo:
  - axis: actor
    value: non-owner-org-member
  - axis: list-scope
    value: organization
  - axis: surface
    value: storefront-ui
  - axis: theme
    value: 2.59.0-pr-2476
anchors:
  - coordinate: /account/lists
  - coordinate: /account/lists/{id}
  - coordinate: Mutation.changeWishlist
evidence:
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-05T12:20:35.545Z
    by: session:91e50fbd
    who: Aleksandra-Mitricheva
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-05T13:02:00.108Z
    by: session:bbd60c47
    who: Aleksandra-Mitricheva
    note: "Reproduced through the UI only: rep set scope My organization in the Share dialog (rep's active org); an Approved member of that org saw the list on /account/lists with card Actions = Rename only, and the list page with Save changes / Rename / Add all to cart / Buy now - no Share, no Remove list. A target-org reader on /shared-list sees no edit/share/remove controls at all."
---
Owner (sales rep, org-scoped token) created a list via createWishlist, added an item, changeWishlist scope=Organization. Signed in on the storefront as another Approved member of that org: /account/lists card Actions menu showed exactly one entry, Rename (3/3 runs with reload, also at 375px); the list page header showed Save changes, Rename, Add all to cart, Buy now — no Share, no Remove list. Rename (name max 25 chars client-side), qty change + Save changes, and Add to cart all worked for the non-owner; the owner's wishlist(listId) still returned sharingSetting.isOwner=true afterwards. For a list the same buyer owns (Private), the card menu shows Rename / Share / Remove list and Remove list (Confirm Delete) deletes it. Pre-fix (VCST-5707 RED) the non-owner was offered Remove list and Share.
