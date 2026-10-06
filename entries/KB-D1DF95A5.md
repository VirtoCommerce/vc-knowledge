---
id: KB-D1DF95A5
subject: How a member of a target organization can read a Customer-scope shared wishlist
plane: experiential
question: "sharedWishlist vs wishlist(listId) vs wishlists for a Customer-scope list: what does a target-org member, a non-target member and an anonymous caller get?"
questions:
  - text: My sales rep shared a list with my company, so why is it missing from my Lists page?
  - text: Can a member of a targeted organization open a customer-scoped shared list any way other than the share link?
  - text: What does a member of a served but not targeted organization get when opening a rep's shared list?
  - text: For a Customer-scope list, how do sharedWishlist, wishlist by id and wishlists differ for a target member versus an anonymous caller?
  - text: Which owner-only fields come back empty when a recipient reads a shared list by its sharing key?
concepts:
  - id: list-sharing
  - id: sales-rep
status: superseded
supersededBy: KB-2F64A447
appliesTo:
  - axis: surface
    value: xapi
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: Query.sharedWishlist
  - coordinate: Query.wishlist
  - coordinate: Query.wishlists
  - coordinate: /shared-list/:sharingKey
evidence:
  - method: observation
    deployment: vcst
    at: 2026-09-29T09:40:11.914Z
    by: session:p27968
    who: Lenajava1
  - method: observation
    deployment: vcst
    at: 2026-09-29T10:34:50.960Z
    by: session:p21476
    who: Lenajava1
    note: "Re-observed on x-cart 3.1037.0-pr-141, cart 3.1011.0-pr-194, sales-rep 3.1012.0-pr-21 for a Customer share with one target: target member reads via sharedWishlist(key) with Read/isOwner false/targets []; wishlist(listId) 'Access denied.'; not in wishlists(); served-but-not-targeted org member gets Forbidden from sharedWishlist. Anonymous not re-tested."
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-06T01:00:26.810Z
    by: session:a839a1fd
    who: Aleksandra-Mitricheva
    note: "2026-10-06 x-cart 3.1038.0-pr-141-b404 / sales-rep 3.1012.0-pr-21-8964, 3-target Customer list, 2 runs: target-org member reads via sharedWishlist(key) access Read, isOwner false, targets [], sharedWithId null; wishlist(listId) Forbidden; wishlists() totalCount 0. Owner sees access Write, isOwner true, all targets. After the org is removed from targets, the very next sharedWishlist(key) call (~200 ms) returns errors[Forbidden 'Access denied.'] data null."
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-06T01:22:45.759Z
    by: session:6878a3e3
    who: Aleksandra-Mitricheva
    note: "2026-10-06 b404/8964: same read paths re-observed; an org-less authenticated user gets Forbidden from sharedWishlist, anonymous Unauthorized."
---
For a list a sales rep shared with scope Customer: a member of a TARGET organization gets the list through sharedWishlist(sharingKey) with sharingSetting.access Read, isOwner false, targets [] and sharedWithId null (owner-only fields stay empty), but wishlist(listId) for the same list returns data null with the error 'Access denied.', and the member's wishlists query does not include the list at all (totalCount 0), so the storefront Lists page shows 'You have not created any lists yet'. The only way the recipient reaches the list is the /shared-list/<sharingKey> link. A member of a served but NOT targeted organization gets 'Access denied.' with extensions.code Forbidden from sharedWishlist; an anonymous caller gets Unauthorized. Observed on XCart 3.1037.0-pr-141, SalesRep 3.1012.0-pr-21, Cart 3.1011.0-pr-194.
