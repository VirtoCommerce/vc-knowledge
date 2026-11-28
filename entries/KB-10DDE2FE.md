---
id: KB-10DDE2FE
subject: "An active API key of a locked user authenticates nothing: protected endpoints return 401 with an empty body and no lock-specific message"
plane: experiential
question: What does the platform return for a request with ?api_key= of an active key whose user is locked out?
status: active
appliesTo:
  - axis: auth
    value: api-key
  - axis: surface
    value: platform-rest
anchors:
  - coordinate: GET /api/platform/security/users/{userName}
  - coordinate: GET /api/platform/security/currentuser
  - coordinate: POST /api/platform/security/users/{id}/lock
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T15:51:37.984Z
    by: session:624d35a1
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T17:55:52.499Z
    by: session:624d35a1
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T18:52:58.294Z
    by: session:624d35a1
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    build: 78242a4a9ed03804
    conditions: platform=3.1105.0-pr-3182-27bb
    at: 2026-11-28T11:48:42.332Z
    by: session:d73f2151
    bodyHash: d05ce3e537df
    who: kutasinaelena
    note: Still true with hashed keys. After locking the owner by id, its generated vc_ key got currentuser 200 {} and 401 with an empty body on /api/platform/modules and POST /api/order/customerOrders/search, in both the header and the query string. After unlock the same key authenticated again (200, owner).
---
On Stable 16 an AGENT-TEST admin-type user with an active API key got 200 JSON from /api/platform/security/currentuser?api_key=<key> while unlocked. After POST /api/platform/security/users/{id}/lock (lockoutEnd=9999-12-31), the same key returned 200 {} from currentuser (same as an invalid key = anonymous) and 401 with an empty body, no content-type, from /api/platform/security/users/{userName}. No 'account locked' text is ever returned for API-key auth; a bogus key gives the identical 401/empty and 200 {} pattern on Stable 15 and 16.
