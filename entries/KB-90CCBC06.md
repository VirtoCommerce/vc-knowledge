---
id: KB-90CCBC06
subject: POST /api/customer/organization-memberships with userId "" (empty string) answers 200 and persists a membership with an empty userId
plane: experiential
question: Can an organization membership be created over REST with an empty-string userId?
questions:
  - text: Does the organization-memberships create endpoint reject an empty userId?
  - text: How can a membership row without a user end up in an organization?
concepts:
  - id: organization-membership
  - id: rest-api
status: active
appliesTo:
  - axis: module
    value: customer
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/customer/organization-memberships
  - coordinate: DELETE /api/customer/organization-memberships
evidence:
  - method: observation
    deployment: vcptcore_regression
    build: 9648699210e92bd7
    conditions: platform=3.1076.13; module:VirtoCommerce.Customer=3.1029.1; role=platform admin
    defect: suspected
    at: 2026-11-10T07:55:54.479Z
    by: session:cb5a6bb9
    who: Aleksandra-Mitricheva
  - method: observation
    deployment: vcst_qa
    build: a1bd5cbdd8350608
    conditions: module:VirtoCommerce.Customer=3.1033.0; role=platform admin
    at: 2026-11-12T08:59:12.096Z
    by: session:9c36bcd2
    bodyHash: c8247ca8ca69
    defect: suspected
    who: Aleksandra-Mitricheva
    note: "Same on Customer 3.1033.0: userId \"\" -> 200 persisted (GET by id 200); first DELETE ?ids= answered 500, a second DELETE answered 204 and GET then 404."
---
Admin bearer, AGENT-TEST organization. POST /api/customer/organization-memberships {userId:"", organizationId, roles:[org-employee]} -> HTTP 200, the response and POST .../search {organizationId} show a membership with userId "" and its role row. Unlike null (500 at the NOT NULL column) the empty string passes the database, because Create validates nothing. DELETE /api/customer/organization-memberships?ids=<it> then answered 500 (body not kept) although GET by id afterwards answered 404 and search returned 0.
