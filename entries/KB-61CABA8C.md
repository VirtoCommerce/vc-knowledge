---
id: KB-61CABA8C
subject: "Deleting a platform user invalidates their API keys at once: the old key gets 401 and no key row survives the user"
plane: experiential
question: What happens to a user's API key when the user is deleted — does the key still authenticate, and does a key row remain?
questions:
  - text: Does an API key of a deleted platform user still work?
  - text: Are a deleted user's API keys removed together with the user?
status: active
appliesTo:
  - axis: module
    value: platform-security
  - axis: surface
    value: rest-api
anchors:
  - coordinate: DELETE /api/platform/security/users
  - coordinate: GET /api/platform/security/users/{id}/apikeys
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: 78242a4a9ed03804
    conditions: platform=3.1105.0-pr-3182-27bb
    at: 2026-11-28T12:10:41.760Z
    by: session:d73f2151
    who: kutasinaelena
---
On a platform build with hashed API keys (vc-platform PR #3182), a back-office user that had an active API key was deleted through DELETE /api/platform/security/users. Immediately afterwards the old key, sent in the api_key header or the query string, got 401 on protected endpoints (GET /api/platform/modules, GET /api/platform/security/users?api_key=…), and GET /api/platform/security/users/{id}/apikeys for that user id returned []. A Security-only platform backup taken afterwards had no UserApiKey row for that user, so no orphan key row was left. Seen twice in one run, by two separate executors (a checklist runner and regression case PLAT-018).
