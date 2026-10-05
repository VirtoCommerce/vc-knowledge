---
id: KB-B38AEE58
subject: The lists page badge collapses organization-scoped and world-readable lists into one Shared state
plane: experiential
question: Can a list owner tell an organization-shared list from a public one on the lists page?
questions:
  - text: On my lists page, how do I know whether a shared list is visible to my company or to everyone?
  - text: Which badge labels can the account lists page and add-to-list dialog show for a list's sharing state?
  - text: Does the lists roster badge distinguish organization, public and customer-shared scopes?
  - text: Where must a tester read a list's real scope when the roster badge only says Shared?
concepts:
  - id: wishlist
  - id: list-sharing
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: GET /account/lists
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
The roster loses the scope distinction: /account/lists and the Add-to-list dialog render a badge with exactly two states, Private and 'Shared', so an organization-scoped list and a world-readable one are indistinguishable on the page where an owner would go looking. The scope is only legible in the List settings dialog, one click deeper, or in WishlistType.scope.
