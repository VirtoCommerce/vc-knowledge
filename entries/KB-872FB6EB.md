---
id: KB-872FB6EB
subject: A sales rep with the global Sales Representative role and no organization membership reaches /company/my-customers and sees "No customers found"; a non-matching search shows "No customers match your search" with Reset search
plane: experiential
question: What does /company/my-customers show for a sales rep who serves no organizations?
status: active
appliesTo:
  - axis: role
    value: sales-rep-no-org
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/my-customers
  - coordinate: /company/sales-reps
evidence:
  - method: observation
    deployment: vcst_qa
    build: e00c89f4da0e9c98
    conditions: platform=3.1083.0; module:VirtoCommerce.SalesRep=3.1014.0-pr-13-4fbb; theme=2.60.0-alpha.2579; role=Sales Representative (global), zero org memberships
    at: 2026-10-15T11:35:26.359Z
    by: session:6ee2ca49
    who: Lenajava1
---
Signed in as the zero-served rep fixture (global sales-rep:access, zero org memberships). /company/my-customers renders heading "My customers", a "Search by name" box and the empty state "No customers found" — no redirect; the rail shows Sales Rep hub (Dashboard, My customers with no badge, Tasks) and no Corporate section. Searching "zzqx" shows a distinct state "No customers match your search" with a "Reset search" button and a Clear control. The same rep opening /company/sales-reps (buyer-facing, org-gated) is redirected to /account/dashboard.
