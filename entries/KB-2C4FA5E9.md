---
id: KB-2C4FA5E9
subject: Platform account lock and unlock succeed only when called with the user id, and unlock leaves lockoutEnd at the minimum date
plane: experiential
question: Why does POST /api/platform/security/users/{userName}/lock return succeeded:false and change nothing?
questions:
  - text: I tried to block a back-office user through the API and nothing happened even though it said OK, why?
  - text: Does the platform lock endpoint accept a login name, or must a QA script pass the account id?
  - text: Why does the lock call return HTTP 200 with succeeded false and an empty errors array?
  - text: After unlocking an account, can lockoutEnd be set back to null through a user update?
  - text: What lockoutEnd value does an account carry after an admin locks it permanently over REST?
concepts:
  - id: account-lockout
  - id: security-account
  - id: rest-api
status: active
appliesTo:
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/platform/security/users/{id}/lock
  - coordinate: POST /api/platform/security/users/{id}/unlock
  - coordinate: PUT /api/platform/security/users
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-19T14:07:54.272Z
    by: session:local_3b
    splitFrom: KB-B9D1132A
---
Measured on vcst-qa 2026-09-19. POST /api/platform/security/users/{userName}/lock returns HTTP 200 with body {"succeeded":false,"errors":[]} and changes nothing — an empty errors array, so there is no message to act on. The same call with the account's GUID id in place of the userName returns {"succeeded":true,"errors":[]} and sets lockoutEnd to 9999-12-31T23:59:59.9999999+00:00. The matching /unlock also takes the id. Note that unlock does not restore lockoutEnd to null: it becomes 0001-01-01T00:00:00+00:00, and a PUT /api/platform/security/users carrying lockoutEnd:null is accepted (succeeded:true) but normalized back to 0001-01-01, so null is not reachable again once an account has been locked. Functionally the min date is unlocked.
