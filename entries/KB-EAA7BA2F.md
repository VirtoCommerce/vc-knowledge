---
id: KB-EAA7BA2F
subject: The storefront Lists page shows your own lists plus your organization's Organization-scope lists; a Customer-scope share reaches its recipient only through its link
plane: experiential
question: Will a list a colleague scoped to the organization show up on my Lists page?
questions:
  - text: Why can't I see the list my colleague shared with our company on my Lists page?
  - text: Does switching the active organization change which lists appear on a buyer's account lists screen?
  - text: Does the storefront lists query send the scope argument, or only the signed-in user's id?
  - text: How does a recipient of a customer-scoped shared list actually open it from the storefront?
  - text: Is organization visibility of wish lists ever exercised by the shipped storefront UI?
concepts:
  - id: wishlist
  - id: list-sharing
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
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
    resolved: claim-amended
    resolvedAt: 2026-10-08T10:46:56.885Z
    resolvedBy: session:d3bcc6b6
    resolvedIn: judge/wishlist-owner-part-20261008
    resolution: An Organization-scope list of a colleague reaches /account/lists on theme 2.51.2 and 2.59.0 alike (live 2026-10-08); the owner-only claim held only for Customer-scope shares. Subject and body corrected.
  - method: observation
    deployment: vcptcore_stable
    conditions: platform=3.1039.12; module:VirtoCommerce.XCart=3.1021.2; module:VirtoCommerce.Cart=3.1006.1; module:VirtoCommerce.Xapi=3.1012.3; theme=2.51.2; store=B2B-store; role=Purchasing agent
    at: 2026-10-08T10:46:48.039Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "2026-10-08: member A set a list to Organization; it appears on member B /account/lists with badge Shared and opens at /account/lists/<id>; B GetWishlists passes no scope argument."
  - method: observation
    deployment: vcst_qa
    conditions: platform=3.1076.0; module:VirtoCommerce.XCart=3.1039.0; module:VirtoCommerce.Cart=3.1012.0; module:VirtoCommerce.Xapi=3.1026.0; theme=2.59.0; store=B2B-store; role=Organization maintainer (owner) / Purchasing agent (member)
    at: 2026-10-08T10:46:48.832Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "2026-10-08: same - A Organization list on B /account/lists (Shared badge, sidebar), opens for B, customerId A, isOwner false, access Write; after A set Private it left B roster and wishlist(listId) was Forbidden for B."
---
The storefront's Lists screen issues GetWishlists with storeId, userId (the signed-in user), cultureName, currencyCode, first, after and sort - no scope argument - and the platform answers with the user's own lists PLUS every list another member of the user's organization has set to scope Organization. Observed live 2026-10-08 on theme 2.51.2 (XCart 3.1021.2) and theme 2.59.0 (XCart 3.1039.0): a list member A set to Organization appears on member B's /account/lists with the 'Shared' badge and in the sidebar, opens at /account/lists/<id>, and reads with customerId = A, isOwner false, access Write; once A sets it back to Private it leaves B's roster and wishlist(listId) is Forbidden for B.

What never reaches the Lists page is a Customer-scope share (the sales-rep share to customer organizations): its recipients get an empty /account/lists ('You have not created any lists yet') and reach the list only through /shared-list/<sharingKey> (see KB-2F64A447). So the page is not an owner-only roster; it is "mine plus my organization's".
