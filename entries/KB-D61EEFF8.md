---
id: KB-D61EEFF8
subject: On vcptcore_qa B2B-store the storefront has no Returns entry and /account/returns renders 404 although the Return module is installed
plane: experiential
question: Is the storefront Returns page reachable on the vcptcore_qa B2B-store, e.g. to check the Returns icon?
status: active
appliesTo:
  - axis: role
    value: sales-rep
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/returns
  - coordinate: /account/orders
evidence:
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-06T11:33:23.841Z
    by: session:caca34e0
    who: Lenajava1
---
2026-10-06, theme 2.59.0-pr-2476-43b1, VirtoCommerce.Return 3.1003.0 listed in /api/platform/modules. Signed in on B2B-store as a sales-rep account (a member of several orgs), the account sidebar showed Dashboard, Orders, Lists, Quote requests, Purchase requests, Saved for later, Back-in-stock list, and no Returns item. Navigating directly to /account/returns rendered the storefront 404 'Page not found'. So a Returns-icon build-marker check cannot be run on this stand with this account. The cause (store-level returns setting or a permission) was not established.
