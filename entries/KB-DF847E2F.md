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
---
Stable 16 (platform 3.1076.1). POST /api/platform/security/users/apikeys with body {userId, userName, isActive:true} answered HTTP 200 with an EMPTY body; GET /api/platform/security/users/{id}/apikeys then listed one row with isActive=true and apiKey=null, and GET /api/platform/security/currentuser?api_key=... could not use it (no key to send). Posting {userId, userName, isActive:true, apiKey:<client-generated GUID>} also returned 200 with an empty body and stored exactly that GUID; the key then authenticated (currentuser returned the user). So the key value is client-generated (the Admin widget generates it), the POST response carries neither the key nor its id, and the id must be read back via GET users/{id}/apikeys. Separately: a user created with userType "Administrator" is NOT isAdministrator and has no permissions, so GET /api/platform/security/users/{name}?api_key=... answers 403 for it; set isAdministrator or grant security:query for a 200 baseline.
