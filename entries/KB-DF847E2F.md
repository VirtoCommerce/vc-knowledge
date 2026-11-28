---
id: KB-DF847E2F
subject: "POST /api/platform/security/users/apikeys does not generate a key: the caller must supply apiKey in the body, otherwise a key row with apiKey=null is stored and never authenticates"
plane: experiential
question: Does POST /api/platform/security/users/apikeys generate the API key value, and what body does it need?
status: active
appliesTo:
  - axis: module
    value: platform-security
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/platform/security/users/apikeys
  - coordinate: GET /api/platform/security/users/{id}/apikeys
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T17:55:41.843Z
    by: session:624d35a1
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T18:52:58.105Z
    by: session:624d35a1
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    build: 78242a4a9ed03804
    conditions: platform=3.1105.0-pr-3182
    at: 2026-11-28T11:38:44.602Z
    by: session:d73f2151
    bodyHash: e581645d3c0e
    who: kutasinaelena
    contradicts: true
    note: On the hashed-API-key build, POST /api/platform/security/users/apikeys with {userId, userName, isActive:true} and no apiKey answered 200 with a one-element array holding the platform-generated key (vc_ + random, shown once) plus apiKeyHint; the key authenticated as its owner at once. A client-supplied value under 32 chars gets 400, a mask-shaped value gets 400, 32+ chars gets 200 and is echoed back. GET users/{id}/apikeys then returns only the hint (in both apiKey and apiKeyHint). The entry describes the pre-change build (3.1076.1); behaviour changed with the API-key hashing work.
  - method: observation
    deployment: vcptcore_qa1
    build: 78242a4a9ed03804
    conditions: platform=3.1105.0-pr-3182-27bb
    at: 2026-11-28T11:48:34.427Z
    by: session:d73f2151
    bodyHash: e581645d3c0e
    who: kutasinaelena
    contradicts: true
    note: "With hashed API keys (vc-platform PR #3182), POST /api/platform/security/users/apikeys with body {userId, userName, isActive:true} and no apiKey returns 200 with a UserApiKey[] array holding one key that the server generated: \"vc_\" followed by 43 characters. The full value appears only in that response. Later GETs return the hint (first 4 characters…last 4) in both apiKey and apiKeyHint, and apiKeyHash is always null. The generated key authenticates as its owner in the header and in the query string. The entry's claim (no generation, empty body) describes the build before this change."
---
Stable 16 (platform 3.1076.1). POST /api/platform/security/users/apikeys with body {userId, userName, isActive:true} answered HTTP 200 with an EMPTY body; GET /api/platform/security/users/{id}/apikeys then listed one row with isActive=true and apiKey=null, and GET /api/platform/security/currentuser?api_key=... could not use it (no key to send). Posting {userId, userName, isActive:true, apiKey:<client-generated GUID>} also returned 200 with an empty body and stored exactly that GUID; the key then authenticated (currentuser returned the user). So the key value is client-generated (the Admin widget generates it), the POST response carries neither the key nor its id, and the id must be read back via GET users/{id}/apikeys. Separately: a user created with userType "Administrator" is NOT isAdministrator and has no permissions, so GET /api/platform/security/users/{name}?api_key=... answers 403 for it; set isAdministrator or grant security:query for a 200 baseline.
