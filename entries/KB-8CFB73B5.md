---
id: KB-8CFB73B5
subject: "AngularJS user blade opened by a Manager without customer/store read rights logs 403s and an unhandled \"SyntaxError: Unexpected end of JSON input\" from the Customer module's transformResponse"
plane: experiential
question: What does the Backoffice console show when a Manager with only platform:security:access/read and mfa:read opens a customer's User information blade?
status: active
appliesTo:
  - axis: surface
    value: admin-spa
anchors:
  - coordinate: GET /api/members/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    conditions: platform=3.1094.0; role=platform:security:access+platform:security:read+mfa:read
    defect: suspected
    at: 2026-11-04T15:19:23.677Z
    by: session:7f21950d
    who: Lenajava1
---
A run Manager role holding platform:security:access, platform:security:read and mfa:read (or + mfa:manage) opened Security > Users > a customer > User information. The blade itself works (MFA widget, Mode blade, GET /api/mfa/modes 200), but the console logs 403s for POST /api/stores/search, GET /api/members/{contactId}, /api/platform/localizable-settings, /api/platform/modules and the AccountTypes/AccountStatuses settings, and an unhandled "SyntaxError: Unexpected end of JSON input" thrown in modules/$(VirtoCommerce.Customer)/dist/app.js transformResponse when parsing the empty 403 body of GET /api/members/{id}. The Container (store) select stays "Loading..." and the contact widget shows "No contact".
