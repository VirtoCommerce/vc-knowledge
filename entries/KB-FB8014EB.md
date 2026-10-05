---
id: KB-FB8014EB
subject: DELETE /api/platform/security/users takes 'names'; a wrong parameter returns succeeded:true and deletes nothing
plane: experiential
question: Which query parameter does DELETE /api/platform/security/users take, and which lookup reliably finds a user?
questions:
  - text: I removed a back-office user and got a success message, so why do they still exist?
  - text: Which query parameter does the platform user delete endpoint expect for the user names?
  - text: Why does fetching a platform user by id return null?
  - text: What is the reliable way to check that a platform user exists and which roles it has?
concepts:
  - id: security-account
  - id: rest-api
status: active
appliesTo:
  - axis: surface
    value: rest
anchors:
  - coordinate: DELETE /api/platform/security/users
  - coordinate: POST /api/platform/security/users/search
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T11:18:14.694Z
    by: session:memimpor
    who: Lenajava1
---
DELETE /api/platform/security/users takes the user names in the names parameter. Sending userNames instead binds an empty list and returns succeeded true while deleting nothing, so a success reply is not proof of deletion. GET /api/platform/security/users/{id} resolves by user name only, so an id returns null, and GET by user name can return a stale or empty body. POST /api/platform/security/users/search with a keyword is the reliable existence and role check (its results include roles).
