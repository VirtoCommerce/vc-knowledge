---
id: KB-7D03DE5C
subject: "A user without admin-shell:access who signs in to /apps/admin-shell/ is not refused: the shell boots with an empty section rail, a raw \"403\" toast, and the Home dashboard widgets still load data through the module APIs the user's other permissions allow"
plane: experiential
question: What happens when a user without admin-shell:access signs in to the Admin Shell?
questions:
  - text: Does the Admin Shell refuse sign-in for a user lacking admin-shell:access?
  - text: What does POST /api/admin-shell/menu return for a user without admin-shell:access and how does the UI react?
  - text: Is the Admin Shell dashboard shown to a user who has order permissions but no admin-shell:access?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.adminshell
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /apps/admin-shell
  - coordinate: POST /api/admin-shell/menu
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: 4a14ffabb4298717
    conditions: platform=3.1086.0-pr-3137-8aa4; module:VirtoCommerce.AdminShell=3.1000.0-pr-1-f481; role=custom role with order:access+order:read, no admin-shell:access
    at: 2026-10-23T10:42:32.751Z
    by: session:5de4e186
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    build: 4a14ffabb4298717
    conditions: platform=3.1086.0-pr-3137-8aa4; module:VirtoCommerce.AdminShell=3.1000.0-pr-1-f481; role=order:access+order:read, no admin-shell:access
    at: 2026-10-23T12:53:42.584Z
    by: session:5de4e186
    bodyHash: 00aa4c0535c5
    who: kutasinaelena
    note: Reproduced on a full page load. In addition, typing #/orders rendered the Orders list inside the shell for this user.
  - method: observation
    deployment: vcptcore_qa1
    build: 3c977d7c206bc258
    conditions: platform=3.1086.0-pr-3137-8aa4; module:VirtoCommerce.AdminShell=3.1000.0-pr-1-34b3; role=order:access+order:read, no admin-shell:access
    at: 2026-10-26T09:33:27.604Z
    by: session:5de4e186
    bodyHash: 00aa4c0535c5
    who: kutasinaelena
    contradicts: true
    contested: module:VirtoCommerce.AdminShell 3.1000.0-pr-1-34b3 vs 3.1000.0-pr-1-f481; role order:access+order:read, no admin-shell:access vs custom role with order:access+order:read, no admin-shell:access; virtocommerce.adminshell 3.1000.0-pr-1-34b3 vs 3.1000.0-pr-1-f481
    note: "On AdminShell 3.1000.0-pr-1-34b3 the same user (order:access+order:read, no admin-shell:access) signs in and gets a working shell: currentuser returns canAccessAdminUI=true, POST /api/admin-shell/menu returns 200 with sections dashboard/orders/insights, no 403 toast, rail Home/Orders/Insights. Entry to the shell is now decided by the platform PlatformUI:Access policy (default: any user with at least one permission), not by admin-shell:access. The entry holds only for build pr-1-f481."
---
Signed in through the Admin Shell login form as a non-administrator holding order:access/order:read but NOT admin-shell:access, then did a full page load of /apps/admin-shell/. Sign-in succeeded (login 200, /connect/token 200, currentuser 200). POST /api/admin-shell/menu returned 403; the section rail rendered only the logo, Notifications and Account (no Home/Orders/Insights), and a toast showing only the text "403" appeared. No refusal or "no access" screen was shown. The Home dashboard still rendered its five widgets (Recent Orders, Orders to process, Outstanding Payments, Fulfillment, Alerts) with live order data, because the widgets call /api/order/customerOrders/search and /api/order/payments/search, which the user's order permissions allow. So admin-shell:access gates the menu endpoint, not entry to the shell or the dashboard widgets.
