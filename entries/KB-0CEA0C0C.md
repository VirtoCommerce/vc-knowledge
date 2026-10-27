---
id: KB-0CEA0C0C
subject: Opening Admin Customers > Add > Contact logs an unhandled TypeError "Cannot set properties of undefined (setting 'status')" from the Customer module, but the blade still works and Create succeeds
plane: experiential
question: Does the Admin New contact blade throw a console error when it opens?
status: active
appliesTo:
  - axis: module
    value: customer
  - axis: surface
    value: admin-spa
anchors:
  - coordinate: POST /api/members
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 1daf7ccef257ac50
    conditions: platform=3.1085.0; module:VirtoCommerce.Customer=3.1029.0
    defect: suspected
    at: 2026-10-27T21:39:51.967Z
    by: session:e75fe8a4
    who: Aleksandra-Mitricheva
---
In Admin SPA (#!/workspace/customers) Companies and contacts, Add > Contact opened the 'New contact' blade and the console logged 'TypeError: Cannot set properties of undefined (setting 'status')' from the VirtoCommerce.Customer module app.js followed by 'Possibly unhandled rejection: {}'. Status still defaulted to New; Create (POST /api/members) made the contact and POST /api/members/search found it.
