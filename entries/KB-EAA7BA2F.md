---
id: KB-EAA7BA2F
subject: storefront Lists page is an owner-only roster
plane: experiential
question: Will a list a colleague scoped to the organization show up on my Lists page?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: Query.wishlists
  - coordinate: GET /account/lists
evidence:
  - method: observation
    deployment: vcptcore_stable
    at: 2026-09-13T07:45:53.276Z
    by: session:6341e00a
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T11:14:43.229Z
    by: session:memimpor
    who: Lenajava1
    note: The owner's own lists are keyed by store and user with no organization argument, so switching the active organization does not change the owner's list set; only lists shared with Organization scope are org-bounded for other members.
  - method: observation
    deployment: vcst
    at: 2026-09-29T09:51:56.899Z
    by: session:p62432
    who: Lenajava1
    note: wishlists(storeId,userId) without scope, as a member of the target org of a Customer-scope share, does not include that list (totalCount 0).
  - method: observation
    deployment: vcst
    at: 2026-09-29T09:39:03.515Z
    by: session:p24664
    who: Lenajava1
    note: A member of the target organization of a Customer-scope list has an empty /account/lists; the list is reachable only through /shared-list/<key>.
  - method: observation
    deployment: vcptcore_qa
    at: 2026-09-29T12:22:06.178Z
    by: session:p20808
    who: Aleksandra-Mitricheva
    note: Target-org member of a Customer-scope share sees 'You have not created any lists yet' on /account/lists.
  - method: observation
    deployment: vcptcore
    at: 2026-09-29T12:22:33.959Z
    by: session:p35948
    who: Aleksandra-Mitricheva
    note: recipient of a rep Customer-scope share sees an empty /account/lists
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-05T12:20:35.518Z
    by: session:91e50fbd
    who: Aleksandra-Mitricheva
    contradicts: true
    note: "Theme 2.59.0-pr-2476-0abb: a list created by a sales rep (org context = the buyer's org) and set to scope Organization via changeWishlist appears on /account/lists AND in the sidebar for a different member of that organization (buyer, not the owner), with a 'Shared' badge, and opens at /account/lists/<id>. So an Organization-scope list belonging to someone else DOES reach the Lists roster; the 'owner-only' statement holds for Customer-scope shares, not for Organization scope."
---

Not by that page. The storefront's Lists screen issues GetWishlists, and that document passes storeId, userId, currencyCode, cultureName, first, after and sort - it does NOT pass the scope argument the schema offers on Query.wishlists, and userId is always the signed-in user. So the roster is defined as lists you own, and an organization-scoped list belonging to someone else cannot reach it by any path, however the platform resolves organization visibility. The only storefront route that reaches a list you do not own is /shared-list/<sharingKey>, which needs the link. Read this as a surface gap rather than a permission answer: the schema exposes a scope filter that the shipped UI never exercises, so what Query.wishlists does with scope is untested from the storefront and must be settled with a direct call.
