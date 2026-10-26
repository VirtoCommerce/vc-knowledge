---
id: KB-9D6FADFD
subject: GET /api/platform/security/users/id/{id} for a nonexistent user returns 200 with body null, not 404
plane: experiential
question: "GET /api/platform/security/users/id/{id} for a deleted or nonexistent user: status 204, 404 or 200 empty body?"
status: active
appliesTo:
  - axis: module
    value: platform-security
  - axis: surface
    value: rest-api
anchors:
  - coordinate: GET /api/platform/security/users/id/{id}
  - coordinate: GET /api/platform/security/users/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: 9c3a20bc84b64e74
    at: 2026-10-26T19:58:48.398Z
    by: session:7ed484a4
    who: kutasinaelena
    note: "distinct: KB-4974045E is about reading an EXISTING user (which route returns it); this is the NOT-FOUND response shape of the by-id route (200 null, no 404) — what a deletion/residue check must interpret"
---
Measured on vcst-qa 2026-10-26 with an admin bearer: GET /api/platform/security/users/id/{id} for an id that does not exist answers HTTP 200 with the JSON body null (not 204, not 404); the by-name GET /api/platform/security/users/{userName} for a nonexistent name answers the same 200 null. After a DELETE /api/platform/security/users?names=..., a by-id read of the deleted user's id returns that same 200 null. A residue check therefore reads gone as exactly that body; a thrown error or a non-null body without the id is not gone.
