---
id: KB-EC1EDCE2
subject: GET loyalty balance/organization/{id} returns 200 with a balance for an organization id that no longer exists; the ledger outlives the organization
plane: experiential
question: What does the loyalty organization balance REST route return when the organization has been deleted?
questions:
  - text: Do a company's loyalty points still exist after the company is deleted?
  - text: GET /api/loyalty-program-operation-log/balance/organization/{organizationId} deleted organization 200
  - text: Does the loyalty organization balance route validate that the organization exists?
  - text: What does a storefront customer whose organization was deleted see as loyalty balance?
concepts:
  - id: loyalty
  - id: organization
status: active
appliesTo:
  - axis: mode
    value: customer
  - axis: module
    value: loyalty
  - axis: surface
    value: rest
anchors:
  - coordinate: GET /api/loyalty-program-operation-log/balance/organization/{organizationId}
  - coordinate: GET /api/loyalty-program-operation-log/balance/user/{userId}
evidence:
  - method: observation
    deployment: vcst
    at: 2026-10-01T08:22:39.254Z
    by: session:4b2eac5d
    who: Lenajava1
---
With the store in Customer mode, organization-scope loyalty ledger rows existed for two organizations. After both organizations were deleted from Contacts (GET /api/members/{id} returns empty, member search by name returns none, their former contacts show no organizations), GET /api/loyalty-program-operation-log/balance/organization/{id} still answered 200 {"balance": N} with the old non-zero totals, and GET balance/user/{id} for those members read 0. The route does not validate that the organization exists, and nothing purges the ledger. A storefront customer who lost the organization sees a personal-account sidebar and Balance 0 with 'No records found'.
