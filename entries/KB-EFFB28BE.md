---
id: KB-EFFB28BE
subject: Member addresses carry separate name and description fields; the storefront address Description column reads description
plane: experiential
question: Why is the Description column empty in the storefront select-address table when the organization address has a name set?
status: active
appliesTo:
  - axis: module
    value: customer
  - axis: surface
    value: xapi
anchors:
  - coordinate: POST /api/members
  - coordinate: Query.me
  - coordinate: /company/info
evidence:
  - method: observation
    deployment: vcst
    at: 2026-10-05T07:27:09.334Z
    by: session:51fcfb97
    who: kutasinaelena
---
A customer Address on POST /api/members has both a name and a description field and they are stored independently. An organization address written with only name set reads back with description null, and xAPI Query.me contact.organization.addresses returns both fields separately. The storefront checkout Select-address table and the company info address list show the Description column from description, so such addresses render with an empty Description. Setting description on the address (POST /api/members with the full member) makes xAPI return it immediately.
