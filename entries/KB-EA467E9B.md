---
id: KB-EA467E9B
subject: a per-organization sales-rep membership is the served-customer relationship itself
plane: experiential
question: Can a sales rep hold sales-rep:access and serve no customers, and what does salesRepCustomers return then?
questions:
  - text: Why does My customers send me back to the dashboard even though I am a sales representative?
  - text: Can a rep be given the sales-rep permission without being assigned any customer companies?
  - text: Does an empty salesRepCustomers result prove that the caller is a sales rep?
  - text: Is the sales-rep customers query gated on the permission or only on being authenticated?
  - text: Where is the sales-rep module's GraphQL schema served?
concepts:
  - id: sales-rep
  - id: organization-membership
  - id: permission
status: active
appliesTo:
  - axis: surface
    value: xapi
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: Query.salesRepCustomers
  - coordinate: /company/my-customers
  - coordinate: /graphql/sales-rep
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T11:18:42.170Z
    by: session:memimpor
    who: Lenajava1
    build: e7c4faf10f640e7c
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    at: 2026-09-30T15:34:07.337Z
    by: session:ce6ed7b9
    who: Lenajava1
    note: A rep with per-organization memberships sees exactly those organizations at /company/my-customers (storefront 2.59.0); /sales-reps/my-customers returns 404
    build: 7b7404aab478a7c7
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    build: 7ec352df61232ca8
    conditions: theme=2.60.0-alpha.2579; platform=3.1083.0; module:VirtoCommerce.SalesRep=3.1014.0-pr-13-4fbb; store=B2B-store; role=Sales Representative; setting:SalesRep.Enabled=true
    at: 2026-10-15T13:12:31.518Z
    by: session:6ee2ca49
    bodyHash: a2af2b4353b9
    who: Lenajava1
    contradicts: true
    contested: virtocommerce.customer 3.1029.0 vs 3.1026.0; virtocommerce.fileexperienceapi 3.1008.0 vs 3.1004.0; virtocommerce.profileexperienceapimodule 3.1021.0 vs 3.1019.0; virtocommerce.return 3.1005.0 vs 3.1001.0; virtocommerce.xapi 3.1027.0 vs 3.1023.0; virtocommerce.xcart 3.1042.0 vs 3.1036.0; …
    note: "2026-10-15, theme 2.60.0-alpha.2579 with the hub delivered by the SalesRep plugin: a rep fixture with sales-rep:access and no served organizations (no Corporate group, no org in the header) signed in, the Sales Rep hub rail showed Dashboard / My customers (no count badge) / Tasks, and clicking My customers stayed on /company/my-customers and rendered the heading \"My customers\" with \"No customers found\" - no redirect to /account/dashboard. So the empty My customers table IS reachable on this build (consistent with the VCST-5494 fix that cleared requiresOrganization on the rep-facing routes); the \"unreachable / redirected\" part of the body did not hold here."
  - method: observation
    deployment: vcst_qa
    build: 7ec352df61232ca8
    conditions: platform=3.1083.0; module:VirtoCommerce.SalesRep=3.1014.0; theme=2.60.0-alpha.2579; role=global sales-rep access, zero org memberships
    at: 2026-10-15T13:56:00.321Z
    by: session:6ee2ca49
    bodyHash: a2af2b4353b9
    who: Lenajava1
    contradicts: true
    contested: virtocommerce.customer 3.1029.0 vs 3.1026.0; virtocommerce.fileexperienceapi 3.1008.0 vs 3.1004.0; virtocommerce.profileexperienceapimodule 3.1021.0 vs 3.1019.0; virtocommerce.return 3.1005.0 vs 3.1001.0; virtocommerce.xapi 3.1027.0 vs 3.1023.0; virtocommerce.xcart 3.1042.0 vs 3.1036.0; …
    note: "Signed in on the storefront as a rep holding the sales-rep role globally with zero organization memberships, navigated to /company/my-customers: the route was NOT redirected (final URL stayed /company/my-customers), the Sales Rep hub section rendered in the account rail, the page showed 'No customers found', SalesRepCustomers and SalesRepCustomersCount returned 200 with totalCount 0 and no errors[]. The 'global-only rep is redirected to /account/dashboard, empty My customers is unreachable' part no longer holds on this build (route meta requiresOrganization:false)."
---
A user is a sales rep when they hold sales-rep:access, granted either globally or per organization through an organization membership - and that per-organization membership is also what makes the organization one of the rep's customers, so the two cannot be separated. salesRepCustomers gates on authentication, not the permission: a plain signed-in buyer and a global-only rep both get 200 with totalCount 0, so an empty result does not prove someone is a rep. The storefront /company/my-customers guard resolves the permission in an organization context; a global-only rep has none and is redirected to /account/dashboard, so an empty My customers table is unreachable. The module's schema is served at /graphql/sales-rep.
