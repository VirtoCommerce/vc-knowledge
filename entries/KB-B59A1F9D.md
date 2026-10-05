---
id: KB-B59A1F9D
subject: "Organization-scope wishlist: any Write member who sends changeWishlist with a scope becomes the list owner"
plane: experiential
question: Can a non-owner org member with Write access change a wishlist's sharing scope via changeWishlist, and what happens to ownership?
status: active
appliesTo:
  - axis: surface
    value: graphql
anchors:
  - coordinate: Mutation.changeWishlist
evidence:
  - method: observation
    deployment: vcptcore
    at: 2026-09-30T11:57:56.393Z
    by: session:p11964
    who: Aleksandra-Mitricheva
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-05T12:15:08.373Z
    by: session:91e50fbd
    who: Aleksandra-Mitricheva
    contradicts: true
    note: "No longer holds from x-cart 3.1038.0-pr-141 (cart 3.1011.0-pr-194, sales-rep 3.1012.0-pr-21), 2026-10-05, 3/3 fresh lists: a non-owner Write member of an Organization-scope list sending changeWishlist with scope AnyoneAnonymous, Private, the unchanged Organization, or Customer+addSharedWithIds(+message) gets errors[Access denied., code Forbidden], data null; customerId/customerName/isOwner and scope stay the owner's. removeWishlist by the member is also Forbidden and the list persists. Rename and item add/update/remove by the member still succeed. The entry describes the pre-fix 3.1037.0-pr-141 build only."
---
Multi-target sharing build (x-cart 3.1037.0-pr-141, cart 3.1011.0-pr-194, sales-rep 3.1012.0-pr-21): on a scope Organization wishlist, an org member with access Write and isOwner false calls changeWishlist with any scope value, even the unchanged Organization; it succeeds and customerId/customerName become the caller's (isOwner true for caller, false for the original owner). After AnyoneAnonymous or Private the original owner gets Access denied on wishlist(listId); AnyoneAnonymous also exposes it to anonymous sharedWishlist(sharingKey). A rename-only changeWishlist (listName/description) does not change ownership. Write members can also add/update/remove items and removeWishlist.
