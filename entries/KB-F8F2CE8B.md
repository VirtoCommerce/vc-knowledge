---
id: KB-F8F2CE8B
subject: A wishlist scope can be Private, AnyoneAnonymous or Organization, more than the schema doc names
plane: experiential
question: What values can a wishlist's scope actually be set to?
questions:
  - text: Can I share my shopping list so that people without an account can see it?
  - text: Which sharing options does the list settings dropdown offer, and what does each one store?
  - text: Is the scope field description on the create-wishlist input complete, or are there undocumented values?
  - text: Which wishlist scope value makes a list readable by anonymous callers?
concepts:
  - id: wishlist
  - id: list-sharing
status: active
appliesTo:
  - axis: surface
    value: xapi
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: InputCreateWishlistType.scope
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
    resolved: split
    resolvedAt: 2026-10-08T10:46:56.073Z
    resolvedBy: session:d3bcc6b6
    resolvedIn: judge/wishlist-owner-part-20261008
    resolution: Copied here by the split of KB-E367DA11; it is about the lists-page badge (KB-B38AEE58), not the scope values, which re-observed live as stated.
  - method: observation
    deployment: vcptcore_stable
    conditions: platform=3.1039.12; module:VirtoCommerce.XCart=3.1021.2; module:VirtoCommerce.Cart=3.1006.1; module:VirtoCommerce.Xapi=3.1012.3; theme=2.51.2; store=B2B-store; role=Purchasing agent
    at: 2026-10-08T10:46:46.427Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "2026-10-08: List settings Sharing options Private / Anyone (readonly) / Organization persist as scope Private / AnyoneAnonymous / Organization."
  - method: observation
    deployment: vcst_qa
    conditions: platform=3.1076.0; module:VirtoCommerce.XCart=3.1039.0; module:VirtoCommerce.Cart=3.1012.0; module:VirtoCommerce.Xapi=3.1026.0; theme=2.59.0; store=B2B-store; role=Organization maintainer (owner) / Purchasing agent (member)
    at: 2026-10-08T10:46:47.239Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "2026-10-08: Share dialog Private / Anyone with link / My organization persist as scope Private / AnyoneAnonymous / Organization; WishlistScopeType enum: Private, AnyoneAnonymous, AnyoneAuthorized, Organization, User, Customer."
---
Three scope values persist from the storefront, not the two the schema's doc string names. InputCreateWishlistType.scope is documented 'List scope (private or organization)', but an ordinary organization member's sharing control offers three options that persist as scope Private, AnyoneAnonymous and Organization: before the sharing rework (theme 2.51.2) the List settings dropdown labels them Private, 'Anyone (readonly)' and Organization; from theme 2.59.0 the Share dialog labels them Private, 'Anyone with link' and 'My organization'. Observed live on both 2026-10-08. The schema's WishlistScopeType enum lists more (Private, AnyoneAnonymous, AnyoneAuthorized, Organization, User, Customer; Customer is the sales-rep share). AnyoneAnonymous is the value that exposes a list to callers with no account, and no documentation string names it. Treat the gloss on that field as incomplete and read the value back after writing it.
