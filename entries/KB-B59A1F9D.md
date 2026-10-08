---
id: KB-B59A1F9D
subject: "Organization-scope wishlist: up to XCart 3.1037 a non-owner member who sends changeWishlist with a scope becomes the list owner; from XCart 3.1038 it is Forbidden"
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
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-05T12:15:08.373Z
    by: session:91e50fbd
    who: Aleksandra-Mitricheva
    contradicts: true
    note: "No longer holds from x-cart 3.1038.0-pr-141 (cart 3.1011.0-pr-194, sales-rep 3.1012.0-pr-21), 2026-10-05, 3/3 fresh lists: a non-owner Write member of an Organization-scope list sending changeWishlist with scope AnyoneAnonymous, Private, the unchanged Organization, or Customer+addSharedWithIds(+message) gets errors[Access denied., code Forbidden], data null; customerId/customerName/isOwner and scope stay the owner's. removeWishlist by the member is also Forbidden and the list persists. Rename and item add/update/remove by the member still succeed. The entry describes the pre-fix 3.1037.0-pr-141 build only."
    resolved: version-scoped
    resolvedAt: 2026-10-08T10:46:57.701Z
    resolvedBy: session:d3bcc6b6
    resolvedIn: judge/wishlist-owner-part-20261008
    resolution: Forbidden from XCart 3.1038 (live on released 3.1039.0); the takeover is live on released XCart 3.1021.2, so it is not only a pre-fix PR build. Body scoped by build.
  - method: observation
    deployment: vcptcore_stable
    conditions: platform=3.1039.12; module:VirtoCommerce.XCart=3.1021.2; module:VirtoCommerce.Cart=3.1006.1; module:VirtoCommerce.Xapi=3.1012.3; theme=2.51.2; store=B2B-store; role=Purchasing agent
    at: 2026-10-08T10:46:49.626Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "2026-10-08: on A Organization lists, B changeWishlist with scope Private / AnyoneAnonymous / the unchanged Organization (the payload the List settings Save sends) succeeded and moved customerId to B; A lost the list; rename-only kept ownership; B removeWishlist succeeded."
  - method: observation
    deployment: vcst_qa
    conditions: platform=3.1076.0; module:VirtoCommerce.XCart=3.1039.0; module:VirtoCommerce.Cart=3.1012.0; module:VirtoCommerce.Xapi=3.1026.0; theme=2.59.0; store=B2B-store; role=Organization maintainer (owner) / Purchasing agent (member)
    at: 2026-10-08T10:46:50.427Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "2026-10-08: B changeWishlist with scope AnyoneAnonymous / Private / unchanged Organization, with and without sharingKey: errors[Forbidden Access denied.], nothing changed; rename-only succeeded with customerId and scope unchanged; B removeWishlist Forbidden."
---
What a non-owner organization member (access Write, isOwner false) can do to a colleague's Organization-scope list through changeWishlist depends on the XCart build.

Up to XCart 3.1037 (observed 2026-09-30 on x-cart 3.1037.0-pr-141, and live 2026-10-08 on the RELEASED XCart 3.1021.2 with theme 2.51.2): a changeWishlist carrying ANY scope value - even the unchanged Organization - succeeds and makes the caller the owner: customerId becomes the caller, isOwner true for the caller, and the original owner loses the list (wishlist(listId) null, gone from their roster). Private or AnyoneAnonymous also clears organizationId; AnyoneAnonymous exposes the list to anonymous sharedWishlist(sharingKey). The storefront's List settings Save always sends scope, so on these builds an ordinary Save by a colleague takes the list over. A rename-only changeWishlist (no scope) keeps ownership, and removeWishlist by the non-owner succeeds.

From XCart 3.1038 (x-cart PR #141; live 2026-10-08 on the released 3.1039.0 with theme 2.59.0): any changeWishlist from the non-owner that carries a scope - AnyoneAnonymous, Private or the unchanged Organization, with or without sharingKey - returns errors[Access denied., code Forbidden] with data null and changes nothing; removeWishlist by the non-owner is Forbidden too. Rename by the non-owner still succeeds (the UI offers only Rename to a non-owner) and keeps customerId and scope; the API accepted a 32-character name the UI would refuse (25-character limit).
