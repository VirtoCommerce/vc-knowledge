---
id: KB-F6BD88E7
subject: GET /api/white-labeling/store/{storeId} for a store with no white-labeling record answers 200 with the JSON body null, not 204 or 404
plane: experiential
question: What does GET /api/white-labeling/store/{storeId} return when the store has no white-labeling record?
questions:
  - text: How do I detect that a store has no white-labeling record over REST?
  - text: Does the white-labeling store endpoint 404 for a store without a record?
status: active
appliesTo:
  - axis: feature
    value: white-labeling
  - axis: surface
    value: rest
anchors:
  - coordinate: GET /api/white-labeling/store/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: 56f07d69d90abb81
    at: 2026-11-27T14:40:17.113Z
    by: session:d2ee4088
    who: Lenajava1
---
On vcst_qa 2026-11-27 GET /api/white-labeling/store/{storeId} with an admin token returned HTTP 200, content-type application/json, body the literal null for every store without a record (6 of 8 stores), and the record object for stores that have one. A client must test the body, not the status; a 404 there means the route is absent (module not installed).
