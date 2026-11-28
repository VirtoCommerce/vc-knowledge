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
---
A user holding only platform:security:access/read/update/delete (not administrator) ran PUT /api/platform/security/users with another (AGENT-TEST) user's object and isAdministrator=true: 200 {succeeded:true}, and GET shows isAdministrator true; the user's Changes widget logs "IsAdministrator: False -> True" by that manager. An API key the manager had created for that user before the promotion then got 200 on admin-only endpoints (GET /api/platform/modules was 403 before). After the promotion the same manager got 403 for creating, regenerating or deactivating that user's keys, so it cannot revoke the admin-powered key it holds. Demotion back by the same manager also succeeded. Whether the promotion itself is older than the API-key change was not checked on an older build.
