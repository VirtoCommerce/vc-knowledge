---
id: KB-F9884EDD
subject: Opening the Admin Shell Account panel as a user without security read permission logs a 403 console error for GET /api/platform/security/users/{own userName}
plane: experiential
question: Why does the Admin Shell log a 403 when a limited back-office user opens the Account panel?
questions:
  - text: Does the Admin Shell Account panel need security:read?
  - text: Console error 403 api/platform/security/users on Account click
status: active
appliesTo:
  - axis: role
    value: limited-back-office
  - axis: surface
    value: admin-shell
anchors:
  - coordinate: GET /api/platform/security/users/{id}
  - coordinate: /apps/admin-shell
evidence:
  - method: observation
    deployment: vcst_qa
    build: 01d892bf697c7bac
    conditions: platform=3.1096.0; vc-shell=2.10.4
    defect: suspected
    at: 2026-11-10T15:38:36.737Z
    by: session:40688c0a
    who: Lenajava1
---
vcst_qa 15:19 and 15:22Z, {BACK_URL}/apps/admin-shell (vc-shell 2.10.4): signed in as a user whose only roles are MCP back-office roles (one with mcp:approve + mcp:audit:read, one with mcp:manage), clicking the Account rail button logged "Failed to load resource: 403" for GET /api/platform/security/users/<own email, url-encoded>. The panel still opened (Profile, Sign out, language) and Sign out worked. A super-admin gets no error. Not checked: which field of the panel goes missing without that response.
