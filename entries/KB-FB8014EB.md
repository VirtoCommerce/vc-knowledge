---
id: KB-FB8014EB
subject: security-user deletion takes names, and a wrong parameter returns a vacuous success
plane: experiential
question: Which query parameter does DELETE /api/platform/security/users take, and which lookup reliably finds a user?
status: active
appliesTo:
  - axis: surface
    value: rest-api
anchors:
  - coordinate: DELETE /api/platform/security/users
  - coordinate: POST /api/platform/security/users/search
evidence:
  - method: observation
    deployment: vcst-qa
    at: 2026-09-25T11:18:14.694Z
    by: session:memimpor
    who: Lenajava1
---
DELETE /api/platform/security/users takes the user names in the names parameter. Sending userNames instead binds an empty list and returns succeeded true while deleting nothing, so a success reply is not proof of deletion. GET /api/platform/security/users/{id} resolves by user name only, so an id returns null, and GET by user name can return a stale or empty body. POST /api/platform/security/users/search with a keyword is the reliable existence and role check (its results include roles).
