---
id: KB-DF95C2F6
subject: "Admin header global search (Ctrl/Cmd+K) is client-side: built-in providers call settings v2 schema and security users/roles search, there is no /api/platform/search endpoint"
plane: experiential
question: Which endpoints does the Admin SPA header global search (Ctrl/Cmd+K) call, and which providers does it have?
questions:
  - text: How does Ctrl+K search work in the Virto admin?
  - text: Can modules add results to the admin header search?
status: active
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: GET /api/platform/settings/v2/global/schema
  - coordinate: POST /api/platform/security/users/search
  - coordinate: POST /api/platform/security/roles/search
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: db5e6e6e527dec12
    conditions: platform=3.1087.0-pr-3134-a3d2; role=Administrator
    at: 2026-10-21T13:57:55.752Z
    by: session:638251b6
    who: kutasinaelena
---
The header global search added in vc-platform PR #3134 is an AngularJS service (globalSearchService) with built-in providers: menu items and apps (client-side, from the menu/apps services), settings (calls GET /api/platform/settings/v2/global/schema), users (POST /api/platform/security/users/search) and roles (POST /api/platform/security/roles/search) via securitySearch.js. No server-side /api/platform/search endpoint exists. Typing is debounced (~150 ms); a new keystroke aborts in-flight provider requests (observed ERR_ABORTED then 200); providers time out after ~8 s and show "Search failed". Aliases (user:, role:, setting:, menu:, app:) scope a query. Modules can add providers via globalSearchService.registerProvider (docs/developer-guide/global-search.md). Live on vcptcore_qa1: "ord" returned Menu (Orders), Settings, Users and Roles groups; Enter opened the Orders workspace.
