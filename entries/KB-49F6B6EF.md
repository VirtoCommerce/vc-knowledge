---
id: KB-49F6B6EF
subject: roles search returns every role with an empty permissions array; only GET roles/{name} carries them
plane: experiential
question: Does POST /api/platform/security/roles/search return the permissions of each role?
questions:
  - text: Why does the role search show no permissions for roles that have them?
  - text: POST /api/platform/security/roles/search permissions empty array
  - text: Which endpoint returns a role's full permissions list?
  - text: Why does a role seeder report permission drift that does not exist?
  - text: Does PUT /api/platform/security/roles with a stable id upsert the role?
concepts:
  - id: platform-role
  - id: permission
status: active
appliesTo:
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/platform/security/roles/search
  - coordinate: GET /api/platform/security/roles/{}
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-01T21:15:59.860Z
    by: session:527ae7ed
    who: kutasinaelena
---
POST /api/platform/security/roles/search returns matching roles with permissions: [] even when the role holds permissions. GET /api/platform/security/roles/{roleName} returns the same role with its full permissions list. A seeder or check that compares permissions from the search result reports a drift that does not exist (seen 2026-10-02 on roles with customer:access, customer:read, punchout:read that a Manager token then exercised correctly: search 200, DELETE 403). PUT /api/platform/security/roles with a stable id upserts the role.
