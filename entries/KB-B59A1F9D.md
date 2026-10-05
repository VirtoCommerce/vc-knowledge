---
id: KB-B59A1F9D
subject: "Organization-scope wishlist: any Write member who sends changeWishlist with a scope becomes the list owner"
plane: experiential
question: Can a non-owner org member with Write access change a wishlist's sharing scope via changeWishlist, and what happens to ownership?
questions:
  - text: Can a colleague with edit rights on a company shopping list take it over from the person who made it?
  - text: Mutation.changeWishlist scope Organization Write member ownership customerId
  - text: Does calling changeWishlist with an unchanged Organization scope transfer list ownership to the caller?
  - text: After a non-owner switches an organization list to Private, can the original owner still open it?
  - text: Does a rename-only changeWishlist by a Write member change who owns the list?
concepts:
  - id: wishlist
  - id: list-sharing
  - id: organization-member
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.changeWishlist
evidence:
  - method: observation
    deployment: vcptcore
    at: 2026-09-30T11:57:56.393Z
    by: session:p11964
    who: Aleksandra-Mitricheva
---
Multi-target sharing build (x-cart 3.1037.0-pr-141, cart 3.1011.0-pr-194, sales-rep 3.1012.0-pr-21): on a scope Organization wishlist, an org member with access Write and isOwner false calls changeWishlist with any scope value, even the unchanged Organization; it succeeds and customerId/customerName become the caller's (isOwner true for caller, false for the original owner). After AnyoneAnonymous or Private the original owner gets Access denied on wishlist(listId); AnyoneAnonymous also exposes it to anonymous sharedWishlist(sharingKey). A rename-only changeWishlist (listName/description) does not change ownership. Write members can also add/update/remove items and removeWishlist.
