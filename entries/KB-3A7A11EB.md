---
id: KB-3A7A11EB
subject: After sign-out and sign-in of a different user in the same Admin Shell tab, the previous user's section rail and Recent items stay rendered even though the new user's POST /api/admin-shell/menu returned 403
plane: experiential
question: Does the Admin Shell clear the previous user's menu and recent items on sign-out before another user signs in?
questions:
  - text: Is the Admin Shell menu state reset between users signing in on the same tab?
  - text: Can a second user see the first user's recent menu items in the Admin Shell?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.adminshell
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /apps/admin-shell
  - coordinate: POST /api/admin-shell/menu
  - coordinate: GET /api/platform/security/logout
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: 4a14ffabb4298717
    conditions: platform=3.1086.0-pr-3137-8aa4; module:VirtoCommerce.AdminShell=3.1000.0-pr-1-f481
    defect: suspected
    at: 2026-10-23T10:42:33.281Z
    by: session:5de4e186
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    build: 4a14ffabb4298717
    conditions: platform=3.1086.0-pr-3137-8aa4; module:VirtoCommerce.AdminShell=3.1000.0-pr-1-f481
    at: 2026-10-23T12:53:42.095Z
    by: session:5de4e186
    bodyHash: 6df3e706e62e
    who: kutasinaelena
    note: "Reproduced again (multi-role -> orders user -> no-access user, same tab). Condition: the stale rail/Recent stays only when the next user's POST /api/admin-shell/menu fails (403); switching to a user who HAS admin-shell:access rebuilt the rail and pins from the new 200 payload. The no-access user could open the stale Orders item and the list loaded; POST me/recent/orders answered 403."
---
User A (admin-shell:access + order:access + order:read) visited Orders and Insights > Reports, then used Account > Sign out (GET /api/platform/security/logout 204, landed on #/login, no full page reload). User B (order permissions, no admin-shell:access) then signed in on the same tab. B's POST /api/admin-shell/menu returned 403 and a "403" toast appeared, yet the rail still showed A's Home/Orders/Insights and the Home panel showed a "Recent" group listing A's Orders and Reports. A full page load (navigate away and back) cleared it: the rail then showed no sections. So the in-memory menu from the previous session survives an in-app sign-out and sign-in.
