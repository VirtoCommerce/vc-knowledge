---
id: KB-DF9FBE2F
subject: A signed-in personal account with no organization deep-linking /company/dashboard lands on /account/dashboard and never sees Sales Rep hub content
plane: experiential
question: What happens when a signed-in user with no organization opens /company/dashboard directly, given the rep routes clear requiresOrganization?
questions:
  - text: Can a personal (no-org) storefront user reach the Sales Rep hub dashboard by URL?
  - text: Does the federated Sales Rep plugin let a user without an organization see /company/dashboard?
status: active
appliesTo:
  - axis: actor
    value: personal-account-no-organization
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/dashboard
  - coordinate: /account/dashboard
evidence:
  - method: observation
    deployment: vcst_qa
    build: f6080d062139e9f0
    conditions: theme=3.1.0-pr-11-d67b; module:VirtoCommerce.SalesRep=3.1014.0; store=B2B-store
    at: 2026-10-20T13:58:46.379Z
    by: session:d992339e
    who: Lenajava1
    note: "distinct: The existing entries cover B2B organization members without sales-rep:access; this one is a personal account with NO organization at all, the case the cleared requiresOrganization on the hub routes could have opened."
---
Signed in as a personal account (no organization membership: account menu shows only Purchasing, Marketing, User, no Corporate), waited 4 s on the home page for the plugin to load, then full-loaded /company/dashboard. It ended on /account/dashboard (heading Dashboard, no Sales Rep hub rail section, no hub widgets). The HAR holds zero requests to /graphql/sales-rep and no salesRep* operation. The cleared requiresOrganization on the hub routes does not open them to a no-org non-rep; the sales-rep:access gate redirects first.
