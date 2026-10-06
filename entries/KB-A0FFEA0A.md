---
id: KB-A0FFEA0A
subject: Target organization members read a Customer-scope shared list through its sharing link
plane: experiential
question: How does a target organization member read a wishlist a sales rep shared with scope Customer?
questions:
  - text: How do I open a product list my sales representative recommended to my company?
  - text: Does a member of a targeted company get read access to a Customer-scope list via its share link?
  - text: What does sharedWishlist(sharingKey) return for a Customer-scope list read by a target organization member?
  - text: What happens to a shared-list link for a member whose organization was removed from the targets?
  - text: Can a member's own lists include non-owner lists shared at organization scope?
concepts:
  - id: list-sharing
  - id: wishlist
  - id: sales-rep
status: superseded
supersededBy: KB-2F64A447
appliesTo:
  - axis: surface
    value: xapi
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: Query.wishlists
  - coordinate: Mutation.changeWishlist
evidence:
  - method: observation
    deployment: vcst
    at: 2026-09-29T09:39:55.644Z
    by: session:p46820
    who: Lenajava1
    splitFrom: KB-AA3C8721
  - method: observation
    deployment: vcst
    at: 2026-09-29T10:34:52.098Z
    by: session:p43100
    who: Lenajava1
    contradicts: true
    note: "The title overstates it: the target member DOES get read access, through sharedWishlist(sharingKey) (access Read, isOwner false, current message). Only wishlist(listId) is Forbidden and the list is absent from wishlists(). Also the member's wishlists() is not necessarily totalCount 0 / free of non-owner lists: it returned an Organization-scope list of the member's own org with isOwner false (the Customer share was absent). Observed on x-cart 3.1037.0-pr-141, cart 3.1011.0-pr-194, sales-rep 3.1012.0-pr-21."
    splitFrom: KB-AA3C8721
  - method: observation
    deployment: vcptcore
    at: 2026-09-29T11:45:54.532Z
    by: session:p27476
    who: Aleksandra-Mitricheva
    contradicts: true
    note: "vcptcore-qa (x-cart pr-141, cart pr-194, sales-rep pr-21): AcmeCorp and TechFlow members both read a Customer-scope list via sharedWishlist(sharingKey) with access Read; only /account/lists/<id> (wishlist(listId)) returns 403 and the list is absent from the reader's own lists."
    splitFrom: KB-AA3C8721
  - method: observation
    deployment: vcptcore_qa
    at: 2026-09-29T12:22:17.248Z
    by: session:p20808
    who: Aleksandra-Mitricheva
    contradicts: true
    note: "Theme 2.59.0-pr-2476 on vcptcore-qa: members of both targeted organizations (AcmeCorp buyer, TechFlow member) opened /shared-list/<key> of a Customer-scope list and saw the list with 'Recommended by your sales representative'; a removed org's member got /403. Read access via link works; only the recipient's own /account/lists does not show the list."
    splitFrom: KB-AA3C8721
  - method: observation
    deployment: vcptcore
    at: 2026-10-02T08:44:55.953Z
    by: session:7c4c6f53
    who: Aleksandra-Mitricheva
    contradicts: true
    note: "2026-10-02 (theme 2.59.0-pr-2476-31b5): a member of the targeted organization reads a Customer-scope list via sharedWishlist(sharingKey) with access Read, isOwner false; only non-target orgs get Access denied."
    splitFrom: KB-AA3C8721
---
The target organization's member does get read access to a Customer-scope list, through sharedWishlist(sharingKey) (access Read, isOwner false, current message); members of each targeted organization opened /shared-list/<key> and saw the list with 'Recommended by your sales representative', while a removed organization's member got /403. The member's wishlists() is not necessarily totalCount 0 or free of non-owner lists: it returned an Organization-scope list of the member's own organization with isOwner false, while the Customer share was absent.
