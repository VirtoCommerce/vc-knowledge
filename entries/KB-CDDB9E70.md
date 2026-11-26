---
id: KB-CDDB9E70
subject: A signed-in user outside a list's share scope opening /shared-list/<key> or /account/lists/<id> is redirected to /403 'Access denied'; the read returns errors[0].extensions.code=Forbidden with a null payload
plane: experiential
question: What does a signed-in non-member see when opening another organization's shared list by URL, and what does the GraphQL read return?
questions:
  - text: Can a user from another organization open a customer-scoped shared list link?
  - text: Does Query.sharedWishlist leak list items to a non-recipient?
  - text: What does /account/lists/<id> show for a list owned by another organization?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
anchors:
  - coordinate: /shared-list
  - coordinate: /account/lists
  - coordinate: Query.sharedWishlist
  - coordinate: Query.wishlist
evidence:
  - method: observation
    deployment: vcst_qa
    build: 1fe74fdd17a1b14e
    conditions: theme=2.63.0-alpha.2681
    at: 2026-11-26T07:32:52.637Z
    by: session:779b94fc
    who: kutasinaelena
---
On theme 2.63.0-alpha.2681, a personal account (not a member of the list owner's organization nor of any recipient organization) signed in and opened (a) /shared-list/<key> of a Customer-scope list shared with two other organizations and (b) /account/lists/<id> and /shared-list/<key> of an Organization-shared list of another org. In all cases the storefront redirected to /403 with headings '403' / 'Access denied' and 'You do not have permissions to access the requested page'; no list name, row or subtotal rendered. Query.sharedWishlist (case a) and Query.wishlist (case b) returned HTTP 200 with errors[0].message 'Access denied.', extensions.code 'Forbidden', and data.sharedWishlist / data.wishlist = null - no item data in the response.
