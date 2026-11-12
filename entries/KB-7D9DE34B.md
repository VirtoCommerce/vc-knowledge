---
id: KB-7D9DE34B
subject: POST /api/customer/organization-memberships with a null or omitted userId answers 500 whose message is the raw SQL Server error (database, table, column, FK name); it is not validated to a 4xx
plane: experiential
question: What does creating an organization membership over REST return when userId is null or missing?
questions:
  - text: Does POST /api/customer/organization-memberships validate userId?
  - text: Why does an organization membership create return 500 with a SQL error?
  - text: Does the platform expose exception messages in REST 500 responses?
  - text: Was the organization-memberships null userId 500 already present in Customer 3.1011?
status: active
appliesTo:
  - axis: module
    value: customer
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/customer/organization-memberships
evidence:
  - method: observation
    deployment: vcptcore_regression
    build: 9648699210e92bd7
    conditions: platform=3.1076.13; module:VirtoCommerce.Customer=3.1029.1; role=platform admin
    defect: suspected
    at: 2026-11-10T07:55:45.071Z
    by: session:cb5a6bb9
    who: Aleksandra-Mitricheva
  - method: observation
    deployment: vcst_qa
    build: a1bd5cbdd8350608
    conditions: module:VirtoCommerce.Customer=3.1033.0; role=platform admin
    at: 2026-11-12T08:58:56.168Z
    by: session:9c36bcd2
    bodyHash: bc600fcfbd3a
    defect: suspected
    who: Aleksandra-Mitricheva
    note: "Re-observed on Customer 3.1033.0: omitted userId -> 500 with raw SQL message naming database, table, column and FK."
---
Admin bearer, fresh AGENT-TEST organization. POST /api/customer/organization-memberships {userId:null, organizationId, roles:[org-employee]} and the same body without userId -> HTTP 500 {"message":"Cannot insert the value NULL into column 'UserId', table '<db>.dbo.CustomerOrganizationMembership' ... FOREIGN KEY constraint FK_CustomerOrganizationMembershipRole_CustomerOrganizationMembership_MembershipId ... database <db> ...","stackTrace":null}, 4/4; with roles:[] only the NOT NULL sentence. Nothing is persisted. Cause in source: OrganizationMembershipController.Create (Customer 3.1029.1) saves without validating UserId, and the platform ApiErrorWrappingMiddleware returns ex.InnerException.Message in every /api/ 500 body (stack trace only hidden outside Development). Same on vcptcore_stable (Customer 3.1011.3), where the stand runs in Development mode and the message is the full exception text - so not new in stable-16.
