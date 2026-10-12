---
id: KB-C8D613FA
subject: POST /api/sales-rep ignores the sent fullName and derives it from firstName + lastName
plane: experiential
question: Does POST /api/sales-rep keep the fullName in the request body?
status: active
appliesTo:
  - axis: module
    value: sales-rep
  - axis: surface
    value: platform-rest
anchors:
  - coordinate: POST /api/sales-rep
  - coordinate: PUT /api/sales-rep
  - coordinate: POST /api/sales-rep/search
evidence:
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-12T09:03:09.457Z
    by: session:c1b20b2e
    who: Aleksandra-Mitricheva
---
Created a sales rep via POST /api/sales-rep with firstName "Perf", lastName "Rep", fullName "<prefix> Rep". GET /api/sales-rep/{id} returned fullName "Perf Rep", so the sent fullName was dropped and rebuilt from the two name parts. A later PUT /api/sales-rep with a changed firstName did update fullName to match. Consequence: a lookup by fullName via POST /api/sales-rep/search misses the rep; look it up by email instead (the search keyword matches the email).
