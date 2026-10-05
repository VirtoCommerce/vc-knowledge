---
id: KB-EFFB28BE
subject: Member addresses carry separate name and description fields; the storefront address Description column reads description
plane: experiential
question: Why is the Description column empty in the storefront select-address table when the organization address has a name set?
questions:
  - text: Why is the Description column blank when choosing a company address at checkout?
  - text: POST /api/members address name description Query.me organization addresses
  - text: Which address field does the storefront select-address Description column read?
  - text: Are a member address's name and description stored as separate fields?
concepts:
  - id: address
  - id: organization
  - id: checkout
status: active
appliesTo:
  - axis: module
    value: customer
  - axis: surface
    value: xapi
  - axis: surface
    value: storefront-ui
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
