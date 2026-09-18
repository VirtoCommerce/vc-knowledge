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
---

Not by that page. The storefront's Lists screen issues GetWishlists, and that document passes storeId, userId, currencyCode, cultureName, first, after and sort - it does NOT pass the scope argument the schema offers on Query.wishlists, and userId is always the signed-in user. So the roster is defined as lists you own, and an organization-scoped list belonging to someone else cannot reach it by any path, however the platform resolves organization visibility. The only storefront route that reaches a list you do not own is /shared-list/<sharingKey>, which needs the link. Read this as a surface gap rather than a permission answer: the schema exposes a scope filter that the shipped UI never exercises, so what Query.wishlists does with scope is untested from the storefront and must be settled with a direct call.
