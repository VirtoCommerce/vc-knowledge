---
id: KB-6EA87C1F
subject: Admin dynamic-properties blade does not enforce Required for Dictionary-type properties, and the REST API enforces Required for no type
plane: experiential
question: Does a Required dictionary dynamic property block OK/Save in the Admin member blade, and does PUT /api/members enforce Required?
status: active
appliesTo:
  - axis: entity
    value: member-dynamic-properties
  - axis: surface
    value: admin-spa
  - axis: surface
    value: rest-api
anchors:
  - coordinate: PUT /api/members
  - coordinate: /workspace/customers
  - coordinate: POST /api/platform/dynamic/properties
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T19:07:26.858Z
    by: session:624d35a1
    who: kutasinaelena
---
Contact with two Required dynamic properties (ShortText dictionary with 2 items, plain ShortText). Admin Dynamic properties blade: OK disabled while plain ShortText empty; after filling it OK enables with the dictionary still empty; Save -> PUT /api/members 204, dictionary value absent on read-back. Same on Organization. PUT /api/members with BOTH required values emptied -> 204, so Required is a client-side check only. Cause: genericValueInput.tpl.html wires ng-required / min-tags on every non-dictionary template but none of the four dictionary templates (ui-select without required). Same on Platform 3.1039.13 and 3.1076.1; template unchanged back to 3.800.0.
