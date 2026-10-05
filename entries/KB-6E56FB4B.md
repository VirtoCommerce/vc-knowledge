---
id: KB-6E56FB4B
subject: "Organization-scope wishlist: only the owner may change scope/sharing or remove the list; members keep rename and item edits"
plane: experiential
question: Can a non-owner member of an Organization-scope wishlist change its scope, share it, or delete it?
status: active
appliesTo:
  - axis: module
    value: x-cart
  - axis: surface
    value: xapi-graphql
anchors:
  - coordinate: Mutation.changeWishlist
  - coordinate: Mutation.removeWishlist
  - coordinate: Query.sharedWishlist
evidence:
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-05T12:15:08.429Z
    by: session:91e50fbd
    who: Aleksandra-Mitricheva
---
On x-cart 3.1038.0-pr-141 (cart 3.1011.0-pr-194, sales-rep 3.1012.0-pr-21), list owned by a sales rep under an org-scoped token, scope Organization, member has access Write isOwner false. Member changeWishlist with scope AnyoneAnonymous, Private, Organization (unchanged), Customer+addSharedWithIds, or Customer+addSharedWithIds+message: errors[Access denied., Forbidden], data.changeWishlist null, nothing written. Member removeWishlist: Access denied. Forbidden, list persists. Member changeWishlist with listName/description only succeeds and keeps the owner; addWishlistItem, updateWishListItems, removeWishlistItem succeed. Owner can switch Private/Organization/AnyoneAnonymous and removeWishlist. Platform administrator token removes another user's org list via xAPI removeWishlist (true). Anonymous and other-org callers get Unauthorized / Forbidden on sharedWishlist(sharingKey) and wishlist(listId) while scope is Organization. Reproduced 3/3 on fresh lists.
