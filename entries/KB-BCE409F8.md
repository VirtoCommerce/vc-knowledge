---
id: KB-BCE409F8
subject: A non-admin with platform:security:update can set isAdministrator=true on another user, so a key it issued to that user beforehand becomes an administrator key it can no longer revoke
plane: experiential
question: Can a non-administrator with platform:security:update set isAdministrator=true on another user via PUT /api/platform/security/users?
questions:
  - text: Can a security manager promote a user to administrator?
  - text: Does the admin-key guard on user API keys stop a non-admin from getting an administrator API key?
  - text: What happens to an existing API key when its owner is made an administrator?
concepts:
  - id: permission
  - id: security-account
  - id: platform-role
status: active
appliesTo:
  - axis: actor
    value: non-admin-security-manager
  - axis: surface
    value: rest-api
anchors:
  - coordinate: PUT /api/platform/security/users
  - coordinate: POST /api/platform/security/users/apikeys
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: 78242a4a9ed03804
    conditions: platform=3.1105.0-pr-3182; role=platform:security:access+read+update+delete
    defect: suspected
    at: 2026-11-28T11:38:31.197Z
    by: session:d73f2151
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    build: 78242a4a9ed03804
    conditions: platform=3.1105.0-pr-3182-27bb; role=platform:security:access+read+update+delete
    at: 2026-11-28T12:13:53.002Z
    by: session:d73f2151
    bodyHash: 0b911a9883a1
    defect: VCST-7074
    who: kutasinaelena
    note: "Reproduced 3/3 over REST: key 403 -> 200 on GET /api/platform/modules after the non-admin PUT users isAdministrator=true; that manager then gets 403 on PUT isActive=false, POST regenerate, DELETE and POST a new key for the user. An administrator can deactivate (key -> 401) and delete it. Source: PUT users has no isAdministrator check on dev before the hashed-key change either (promotion pre-existing); the 403 on revoke comes from the new admin-owner guard on the API-key endpoints."
---
A user holding only platform:security:access/read/update/delete (not administrator) ran PUT /api/platform/security/users with another (AGENT-TEST) user's object and isAdministrator=true: 200 {succeeded:true}, and GET shows isAdministrator true; the user's Changes widget logs "IsAdministrator: False -> True" by that manager. An API key the manager had created for that user before the promotion then got 200 on admin-only endpoints (GET /api/platform/modules was 403 before). After the promotion the same manager got 403 for creating, regenerating or deactivating that user's keys, so it cannot revoke the admin-powered key it holds. Demotion back by the same manager also succeeded. Whether the promotion itself is older than the API-key change was not checked on an older build.
